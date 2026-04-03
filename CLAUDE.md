# tclaude

## What is tclaude?

`tclaude` is a cross-platform CLI tool written in Go that extends Claude Code with session management, conversation utilities, and developer workflow features.
It wraps Claude Code sessions in tmux for detach/reattach, provides conversation search/management (including semantic/embedding-based search), git-based conversation sync,
usage tracking, a web terminal, task management, and a custom status bar.

## Build & Test

```bash
go build ./...                    # Build all packages
go test ./...                     # Run all tests
go test ./pkg/claude/conv/...     # Run tests for a specific package
go vet ./...                      # Lint
go install .                      # Install locally
```

CI runs `go test ./...` and `go vet ./...` across Linux, macOS, and Ubuntu ARM64.
Releases are built with goreleaser (`CGO_ENABLED=0` - pure Go, no C dependencies).

## Architecture

**Entry point:** `main.go` - calls `pkg/claude.Cmd()` which builds the cobra command tree, then closes the DB on exit.

**Command framework:** Uses [cobra](https://github.com/spf13/cobra) via [boa](https://github.com/GiGurra/boa) (type-safe param wrappers). All commands use `boa.CmdT[ParamType]` with `common.DefaultParamEnricher()`.

**Package layout under `pkg/claude/`:**

| Package     | Purpose                                                                                                                                                                             |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `session`   | Core tmux-based session management (new, list, attach, focus, goto, kill, watch, prune). Sessions stored in SQLite (`~/.tclaude/db.sqlite`). Hook callbacks update session status.  |
| `conv`      | Conversation management (list, search, semantic search via embeddings, resume, copy, move, delete, prune). Reads Claude's `.jsonl` conversation files and `sessions-index.json`.    |
| `git`       | Git-based conversation sync across devices. Uses `~/.claude/projects_sync` as a separate git working directory.                                                                     |
| `worktree`  | Git worktree management for parallel Claude sessions on different branches.                                                                                                         |
| `stats`     | Activity statistics from Claude's `~/.claude/stats-cache.json`.                                                                                                                     |
| `usage`     | Subscription usage limits via Anthropic API.                                                                                                                                        |
| `statusbar` | Status bar output for Claude Code's statusline feature (hidden command, reads JSON from stdin).                                                                                     |
| `web`       | Web terminal server - serves tmux sessions via xterm.js + WebSocket with TLS and basic auth.                                                                                        |
| `setup`     | One-time setup: installs hooks in `~/.claude/settings.json`, registers protocol handler (v3), configures notifications.                                                             |
| `task`      | Task management: parses TODO.md tasks, runs them sequentially via Claude Code sessions.                                                                                             |
| `selftest`  | Hidden integration tests for manual verification of credentials and API access.                                                                                                     |
| `syncutil`  | Shared git sync utilities used by the `git` package.                                                                                                                                |

**Shared utilities under `pkg/claude/common/`:**

| Package       | Purpose                                                                                        |
|---------------|------------------------------------------------------------------------------------------------|
| `config`      | tclaude config file (`~/.tclaude/config.json`)                                                 |
| `convops`     | Shared conversation operations: `SessionEntry`, `SessionsIndex` types (used by `conv` and `convindex`) |
| `convindex`   | Conversation index management                                                                  |
| `db`          | SQLite store (`~/.tclaude/db.sqlite`) for session state, caches, and embeddings. WAL mode, pure-Go via `modernc.org/sqlite`. Singleton pattern with `sync.Once`. Schema v5 with auto-migration. |
| `notify`      | Desktop notifications (D-Bus on Linux, terminal-notifier on macOS, PowerShell on WSL)         |
| `table`       | Interactive sortable table UI using bubbletea                                                  |
| `terminal`    | Terminal detection and window focus (platform-specific)                                        |
| `usageapi`    | Anthropic usage API client with OAuth token refresh                                            |
| `wsl`         | WSL detection and PowerShell path resolution                                                   |
| `binary.go`   | Binary path resolution                                                                         |
| `completions.go` | Shell completions helpers                                                                   |
| `tmux.go`     | Tmux command wrapper                                                                           |

**`pkg/common/`:** Shared utilities (dirs, file locking, size parsing, logging).

## CLI Commands

```
tclaude [--log-level=info|debug|warn|error]
├── session                           # Tmux session management
│   ├── new [--cwd DIR]
│   ├── list [-w|--watch]
│   ├── attach [SESSION_ID]
│   ├── focus [SESSION_ID]            # Focus terminal window (macOS/Linux)
│   ├── goto [SESSION_ID]             # Switch to tmux pane (macOS/Linux)
│   ├── kill [--all] [SESSION_ID]
│   ├── prune [--all] [--dry-run] [DAYS]
│   └── watch [--interval] [SESSION_ID]
│
├── conv(ersation) [--global]         # Conversation management
│   ├── list [-w|--watch] [-s|--search TERM] [-p|--projects]
│   ├── search [-g|--global] [TERM]
│   ├── search-embeddings [-g|--global] [-t|--top N] QUERY   # Semantic search
│   ├── index-embeddings [-p|--project] [--force]            # Build embedding index
│   ├── resume [--global] [CONV_ID]
│   ├── cp [--no-history] SRC DEST
│   ├── mv SRC DEST
│   ├── delete [--force] [CONV_ID...]
│   └── prune-empty [--dry-run] [DAYS]
│
├── git                               # Git-based conversation sync
│   ├── init <REPO_URL>
│   ├── sync [--dry-run] [--skip-fetch]
│   ├── status
│   ├── fetch
│   └── repair
│
├── worktree                          # Git worktree management
│   ├── add [-b|--branch] <NAME> [<REF>]
│   ├── list
│   ├── remove <NAME> [--prune]
│   ├── restore <NAME>
│   └── switch <NAME>
│
├── stats [-d|--days N] [-j|--json] [-t|--tokens]
├── usage [--json]
├── setup [-c|--check] [-f|--force]
├── web [--port N] [--user U] [--pass P] [--bind ADDR] [--no-tls] [SESSION_ID]
├── task [--dir DIR]                  # Task management
│   ├── add <TITLE> [--plan] [--plan-auto-accept]
│   ├── list
│   └── run [--continue] [TASK_ID]
├── statusbar                         # Hidden: invoked by Claude Code statusline
└── selftest [--token TOKEN] [--no-api]  # Hidden: manual integration tests
```

## Key Data Structures

- **SessionState** (`pkg/claude/session/session.go`) - tmux session with ID, status, CWD, ConvID, PID, timestamps
- **SessionEntry** (`pkg/claude/common/convops/convops.go`) - conversation record with SessionID, path, summary, git branch, message count
- **SessionsIndex** (`pkg/claude/common/convops/convops.go`) - wrapper around `[]SessionEntry` with version
- **Config** (`pkg/claude/common/config/config.go`) - notifications, AutoCompactPercent, LogLevel, TransitionRules
- **Task / TaskResult** (`pkg/claude/task/task.go`) - task title/prompt, plan mode flags, execution results

## Key Patterns

- Platform-specific code uses Go build tags: `_linux.go`, `_darwin.go`, `_windows.go`, `_unix.go`
- Session state is stored in SQLite with WAL mode for concurrent access from hook callbacks
- DB uses a singleton pattern (`sync.Once`) in `pkg/claude/common/db/db.go`; `ResetForTest()` resets it in tests
- Interactive list views (sessions, conversations) use bubbletea with the shared `table` package
- The `statusbar` and `selftest` commands are hidden (`cmd.Hidden = true`)
- All tests use `t.TempDir()` and set `HOME` to isolate config/DB from the real user environment
- Semantic search uses locally stored embeddings in the SQLite DB (table `embeddings`, schema v5)

## Database Schema

SQLite at `~/.tclaude/db.sqlite` with WAL mode. Current schema version: **5**.

| Table        | Purpose                                    |
|--------------|--------------------------------------------|
| `sessions`   | Session CRUD (status, tmux, conv ID, PID)  |
| `notify_state` | Notification cooldown tracking           |
| `usage_cache`  | Cached API usage data                    |
| `git_cache`    | Cached git sync state                    |
| `conv_index`   | Conversation index for fast search       |
| `embeddings`   | Semantic search embedding vectors        |

Migrations run automatically from v0→v5 on first open (`pkg/claude/common/db/migrate.go`).

## Configuration Files

| Location                          | Purpose                                         |
|-----------------------------------|-------------------------------------------------|
| `~/.tclaude/config.json`          | tclaude configuration                           |
| `~/.tclaude/db.sqlite`            | Session state, caches, embeddings               |
| `~/.tclaude/output.log`           | Structured log output (slog text format)        |
| `~/.claude/projects/`            | Claude's conversation directories (.jsonl files) |
| `~/.claude/projects_sync/`       | Git sync working directory                      |
| `~/.claude/settings.json`        | Claude Code settings + tclaude hooks            |
| `~/.claude/stats-cache.json`     | Claude Code activity statistics                 |

## Claude Code Integration Points

- **Protocol handler:** `tclaude://` URI scheme registered by `tclaude setup` (protocol version 3)
- **Hooks:** `tclaude setup` installs callbacks in `~/.claude/settings.json` for session start/stop/status events
- **Status bar:** `tclaude statusbar` reads JSON from stdin and outputs formatted status; invoked by Claude Code's statusline feature
- **Notifications:** Triggered by hook callbacks when sessions complete or need attention
