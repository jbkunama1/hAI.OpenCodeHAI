# MCP-Setup für OpenCode im Container

## Ausgangsprobleme

Zwei Probleme mussten gelöst werden:

1. **Filesystem-MCP**: `npx @modelcontextprotocol/server-filesystem --help` schlug fehl, weil der Server keine CLI-Flags wie `--help` kennt — er interpretiert alle Argumente als Verzeichnispfade. Das Ergebnis war ein `ENOENT`-Fehler auf `/workspace/--help`.

2. **GitHub-MCP**: Der Copilot-Endpoint `https://api.githubcopilot.com/mcp/` ist nicht kompatibel mit OpenCode (OAuth-Flow-Konflikt). Lösung: GitHub-MCP lokal als HTTP-Server betreiben und OpenCode darauf verweisen.

---

## Lösung 1 — Filesystem-MCP korrekt konfigurieren

Keine Flags übergeben — nur erlaubte **Verzeichnisse als Argumente**:

```json
"filesystem": {
  "type": "local",
  "command": [
    "npx",
    "-y",
    "@modelcontextprotocol/server-filesystem",
    "/workspace",
    "/root/.config/opencode"
  ],
  "enabled": true
}
```

Weitere Verzeichnisse können beliebig hinten angehängt werden.

---

## Lösung 2 — GitHub-MCP lokal als HTTP-Server betreiben

### Startskript anlegen

Datei: `/workspace/bin/github-mcp.sh`

```sh
#!/bin/sh
set -eu

BASE_DIR="/workspace"
LOG_DIR="$BASE_DIR/log"
PID_FILE="$BASE_DIR/github-mcp.pid"
OUT_LOG="$LOG_DIR/github-mcp.out.log"
ERR_LOG="$LOG_DIR/github-mcp.err.log"
PORT="${GITHUB_MCP_PORT:-3001}"
HOST="${GITHUB_MCP_HOST:-127.0.0.1}"
URL="http://$HOST:$PORT/mcp"

is_running() {
  [ -f "$PID_FILE" ] || return 1
  PID="$(cat "$PID_FILE" 2>/dev/null || true)"
  [ -n "$PID" ] || return 1
  kill -0 "$PID" 2>/dev/null
}

start_server() {
  mkdir -p "$LOG_DIR"
  if [ -z "${GITHUB_TOKEN:-}" ]; then
    echo "GITHUB_TOKEN ist nicht gesetzt." >&2; exit 1
  fi
  if is_running; then
    echo "github-mcp läuft bereits mit PID $(cat "$PID_FILE")"
    echo "$URL"; exit 0
  fi
  nohup sh -c 'exec npx github-mcp-server-kosta --transport http --http-port "$0" --host "$1"' "$PORT" "$HOST" >"$OUT_LOG" 2>"$ERR_LOG" &
  PID=$!
  echo "$PID" > "$PID_FILE"
  sleep 3
  if is_running; then
    echo "github-mcp gestartet (PID $PID)"; echo "$URL"
  else
    echo "Fehler. Logs prüfen: $OUT_LOG / $ERR_LOG" >&2; exit 1
  fi
}

stop_server() {
  if is_running; then
    PID="$(cat "$PID_FILE")"
    kill "$PID" 2>/dev/null || true
    sleep 1; kill -9 "$PID" 2>/dev/null || true
    rm -f "$PID_FILE"
    echo "github-mcp gestoppt (PID $PID)"
  else
    rm -f "$PID_FILE"; echo "github-mcp läuft nicht"
  fi
}

status_server() {
  if is_running; then
    echo "github-mcp läuft (PID $(cat "$PID_FILE"))"; echo "$URL"
  else
    echo "github-mcp läuft nicht"; exit 1
  fi
}

logs_server() {
  touch "$OUT_LOG" "$ERR_LOG"
  tail -n 50 "$OUT_LOG" "$ERR_LOG"
}

case "${1:-}" in
  start)   start_server ;;
  stop)    stop_server ;;
  restart) stop_server || true; start_server ;;
  status)  status_server ;;
  logs)    logs_server ;;
  *)
    echo "Verwendung: $0 {start|stop|restart|status|logs}" >&2
    exit 1
    ;;
esac
```

### Rechte setzen

```sh
chmod +x /workspace/bin/github-mcp.sh
```

### OpenCode-Config eintragen

```json
"github-local": {
  "type": "remote",
  "url": "http://127.0.0.1:3001/mcp",
  "enabled": true
}
```

---

## Kompletter MCP-Block in `opencode.json`

```json
"mcp": {
  "filesystem": {
    "type": "local",
    "command": [
      "npx",
      "-y",
      "@modelcontextprotocol/server-filesystem",
      "/workspace",
      "/root/.config/opencode"
    ],
    "enabled": true
  },
  "github-local": {
    "type": "remote",
    "url": "http://127.0.0.1:3001/mcp",
    "enabled": true
  }
}
```

---

## Tägliche Nutzung

```sh
export GITHUB_TOKEN='github_pat_xxxx'

/workspace/bin/github-mcp.sh start    # Server starten
/workspace/bin/github-mcp.sh status   # Prüfen ob läuft
/workspace/bin/github-mcp.sh logs     # Letzte 50 Log-Zeilen
/workspace/bin/github-mcp.sh stop     # Server stoppen
/workspace/bin/github-mcp.sh restart  # Neustart
```

Optional als Alias in der Shell-RC (z. B. `~/.bashrc` oder `~/.profile`):

```sh
alias github-mcp='/workspace/bin/github-mcp.sh'
```

---

## Datei- und Verzeichnisstruktur

| Pfad | Funktion |
|---|---|
| `/workspace/bin/github-mcp.sh` | Startskript |
| `/workspace/log/github-mcp.out.log` | stdout-Log |
| `/workspace/log/github-mcp.err.log` | stderr-Log |
| `/workspace/github-mcp.pid` | PID-Datei (laufender Prozess) |
| `/root/.config/opencode/opencode.json` | OpenCode-Konfiguration |

---

## Hinweise

- **Container-Neustart**: Der GitHub-MCP-Server startet **nicht automatisch** nach einem Container-Neustart. Das Skript muss erneut aufgerufen werden. Für Autostart: Eintrag im Container-Entrypoint oder im Docker-Compose-Setup ergänzen.
- **GITHUB_TOKEN**: Muss in derselben Shell gesetzt sein, bevor das Skript aufgerufen wird. Am besten in eine `.env`-Datei oder ein Wrapper-Skript auslagern, das `source` nutzt.
- **Port-Konflikt**: Standardport ist `3001`. Über die Umgebungsvariable `GITHUB_MCP_PORT` kann ein anderer Port gesetzt werden.
- **Beschreibbarkeit**: `/workspace/bin/` muss im Container beschreibbar gemountet sein. `/root` und `/root/.config` waren in diesem Setup nicht beschreibbar.
