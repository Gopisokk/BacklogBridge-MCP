# BacklogBridge-MCP

<p align="center">
  <a href="https://github.com/Gopisokk/BacklogBridge-MCP"><img src="https://img.shields.io/badge/Project-BacklogBridge--MCP-10b981?style=for-the-badge&logo=github" alt="Project"></a>
  <a href="https://modelcontextprotocol.io"><img src="https://img.shields.io/badge/MCP-Native-7c3aed?style=for-the-badge" alt="MCP Native"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache--2.0-2563eb?style=for-the-badge" alt="License"></a>
  <a href="https://www.rust-lang.org"><img src="https://img.shields.io/badge/Rust-2024%20Edition-f97316?style=for-the-badge&logo=rust" alt="Rust"></a>
  <a href="https://svelte.dev"><img src="https://img.shields.io/badge/Frontend-Svelte%205-ff3e00?style=for-the-badge&logo=svelte" alt="Svelte 5"></a>
</p>

<p align="center">
  <strong>Next-Generation Issue Tracker & Task Management Engine for Autonomous AI Agents and Engineering Teams.</strong><br>
  Single Self-Contained Binary &bull; SQLite-Backed &bull; Deep Model Context Protocol (MCP) Integration
</p>

---

## 🚀 Why BacklogBridge-MCP?

Modern agentic coding workflows fail when issue state, architecture plans, and sprint milestones are crammed into LLM prompt contexts. Context windows fill up, agent sessions expire, and coordination between multiple AI agents and human engineers gets lost.

**BacklogBridge-MCP** solves this fundamentally:
- **Persistent Agent Memory**: Backlogs, issue dependencies, verification proof, and step plans live on your persistent SQLite instance rather than in transient LLM context windows.
- **Ultra-Lean MCP Protocol**: **31 MCP tools packaged into ~5,781 tokens**—minimal token overhead leaves maximum context for actual code and logic.
- **Single Self-Contained Binary**: Rust backend + SQLite database + full reactive web UI bundled together (~25–32 MB). Zero external infrastructure: no Postgres, no Redis, no Docker required.
- **Autonomous Multi-Agent Collaboration**: AI agents (Claude Code, Cursor, OpenCode, Codex, Gemini, Windsurf, Zed) connect as first-class bots under your instance to pick up tasks, submit verification proofs, and update status in real time.

---

## ⚡ 60-Second Quickstart

### 1. Build and Run

```bash
# Clone repository
git clone https://github.com/Gopisokk/BacklogBridge-MCP.git
cd BacklogBridge-MCP

# Initialize config, SQLite database, and admin account
cargo run -- init

# Connect your AI agents & MCP clients
cargo run -- connect
```

The interactive Web UI will immediately be live at **`http://localhost:3456`**.

### 2. Connect Your AI Clients

BacklogBridge-MCP detects and configures your AI tooling automatically:

```bash
cargo run -- connect                                        # Interactive client selector
cargo run -- connect --client cursor --client opencode --yes # Headless auto-configure
cargo run -- connect --client claude-code --scope project   # Repo-local .mcp.json setup
cargo run -- connect --client zed --stdio                   # Zero-server direct SQLite over stdio
```

Supported out-of-the-box:
- **Cursor**
- **Claude Code** / **Claude Desktop**
- **OpenCode**
- **VS Code** / **Windsurf**
- **Zed**
- **Goose** / **Gemini CLI** / **Codex**

---

## 🛠️ MCP Tool Suite

BacklogBridge-MCP delivers **31 high-performance MCP tools** organized into clear functional families:

| Category | Tools | Purpose |
| :--- | :--- | :--- |
| **Issues** | `list_issues`, `get_issue`, `create_issue`, `update_issue`, `bulk_update`, `edit_issue`, `get_board` | Manage tickets, fetch workable tasks, update status, and inspect boards |
| **Relations & Blockers** | `link_issues`, `unlink_issues` | Express dependencies: blockers, issue parent-child relationships, and human waits |
| **Step Plans** | `create_plan`, `get_plan`, `edit_plan_step`, `update_plan_step` | Hierarchical step plans that outlive LLM sessions; resolve steps alongside issues |
| **Documentation & Pages** | `get_page`, `create_page`, `update_page`, `edit_page` | Maintain architecture specs, RFCs, and guidelines inside the project workspace |
| **Comments & Evidence** | `add_comment`, `list_comments`, `edit_comment`, `delete_comment` | Record verification proofs (e.g. `cargo test passed`) and collaboration logs |
| **Activity & Briefing** | `get_briefing`, `search`, `get_activity` | Project resume briefings (`get_briefing(project="APP")`) and audit feeds |
| **Management & Export** | `list_resources`, `manage_resource`, `delete`, `export` | Workspace resource hierarchy and full JSON/archive export |

---

## 💻 Developer CLI

Inspect and drive your backlog directly from the terminal or automated shell pipelines:

```bash
# View workable issues without pending blockers
backlogbridge-mcp issue list --project CORE --workable

# Create a prioritized feature or bug ticket
backlogbridge-mcp issue create --project CORE --title "Implement JWT rotation" --priority high

# Update issue status with verification note
backlogbridge-mcp issue update CORE-14 --status done

# Instant full-text search across all issues and pages
backlogbridge-mcp search "authentication token" --project CORE

# Diagnostics and health check
backlogbridge-mcp doctor
```

---

## 🔌 MCP Client Configuration

### Remote (Streamable HTTP)
```json
{
  "mcpServers": {
    "backlogbridge": {
      "type": "remote",
      "url": "http://localhost:3456/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

### Local (Direct Stdio / Embedded)
```json
{
  "mcpServers": {
    "backlogbridge": {
      "command": "backlogbridge-mcp",
      "args": ["--db", "backlog.db", "mcp"]
    }
  }
}
```

---

## 📦 Deployment & Backups

### Docker (Optional)
```bash
docker build -t backlogbridge-mcp .
docker run -d -p 3456:3456 -v backlog_data:/data \
  -e LIFIC_INIT_ADMIN_NAME="Gopi Rk" \
  -e LIFIC_INIT_ADMIN_PASSWORD="your-secure-password" \
  backlogbridge-mcp
```

### Automated Disaster Recovery & Backups
BacklogBridge-MCP creates atomic backups every hour and rotates retention cycles automatically:
```bash
# Create safe hot backup while the server is serving traffic
cargo run -- dump

# Restore from backup archive
cargo run -- restore backup_snapshot.tar.gz
```

---

## 🏗️ Architecture & Technology Stack

- **Backend**: Rust 2024 Edition (Axum 0.8, Tokio, Rusqlite, Tower-HTTP)
- **Frontend**: Svelte 5, Tailwind CSS v4, Lucide Icons, Vite, Bun
- **Transport**: Model Context Protocol (stdio & HTTP Streamable), WebSockets, Server-Sent Events (SSE)
- **Database**: Embedded SQLite with WAL mode and atomic transactions


---

## 📜 License

Licensed under the [Apache-2.0 License](LICENSE).
