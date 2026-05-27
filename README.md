# jira-cli (mcp_jira)

**Hermes Agent** • MIT License  
**Author**: Paolino Salamone

> Production-ready Jira CLI + MCP server for GitHub Copilot CLI and Claude Desktop.

## 📚 Quick Links

- **Installation**: [INSTALLATION.md](INSTALLATION.md)
- **Configuration**: [CONFIGURATION.md](CONFIGURATION.md)
- **MCP Setup**: [MCP-SETUP.md](MCP-SETUP.md)
- **MCP Services**: [SERVICES.md](SERVICES.md) - Complete reference of 17 MCP tools
- **Troubleshooting**: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)

---

## 🎯 What is jira-cli?

**jira-cli** is a dual-mode tool:

1. **CLI Mode**: Command-line interface for Jira operations
   ```bash
   jira issue get PROJ-123
   jira issue create --project PROJ --type Task --summary "My task"
   jira search --jql "assignee = currentUser()"
   ```

2. **MCP Server Mode**: AI assistant integration
   ```bash
   jira mcp serve
   # Now Claude Desktop / GitHub Copilot can use jira-cli tools
   ```

---

## ✨ Key Features

- ✅ **Dual Jira Support**: Jira Cloud + Jira Server/Data Center
- ✅ **Automatic API Version**: Detects Cloud vs Server automatically
- ✅ **Multiple Sites**: Configure and switch between several Jira instances
- ✅ **MCP Integration**: Use with Claude Desktop or GitHub Copilot CLI
- ✅ **Attachment Management**: Download single/batch/recursive downloads
- ✅ **2FA Support**: Session cookie for 2FA-protected instances
- ✅ **Self-Contained**: No runtime dependencies required
- ✅ **Cross-Platform**: Linux, macOS, Windows (WSL2)

---

## 🚀 Getting Started

### 1. Install Binary

```bash
chmod +x publish/jira
sudo cp publish/jira /usr/local/bin/
```

See [INSTALLATION.md](INSTALLATION.md) for details.

### 2. Configure Jira Site

**Jira Cloud:**
```bash
jira config add --site myco \
  --url https://myco.atlassian.net \
  --email me@myco.com \
  --token ATATT3xFfGF0...
```

**Jira Server/DC:**
```bash
jira config add --site eng \
  --url https://jira.eng.com/jira \
  --token <PersonalAccessToken> \
  --auth-mode bearer
```

See [CONFIGURATION.md](CONFIGURATION.md) for details.

### 3. Test CLI

```bash
jira issue get PROJ-123
```

### 4. (Optional) Set Up MCP

```bash
# Manual test
jira mcp serve

# Register with Claude Desktop / GitHub Copilot CLI
# See [MCP-SETUP.md](MCP-SETUP.md)
```

---

## 📋 CLI Command Reference

### Issue Management

```bash
jira issue get PROJ-123
jira issue create --project PROJ --type Task --summary "..."
jira issue update PROJ-123 --summary "New title"
jira issue assign PROJ-123 --to <accountId>
jira issue transition PROJ-123 --to "In Progress"
jira issue comment PROJ-123 --body "..."
jira issue worklog PROJ-123 --time "2h 30m"
jira issue attach PROJ-123 --file ./screenshot.png
```

### Attachment Downloads

```bash
jira issue attachments PROJ-123                    # list
jira issue download-attachment PROJ-123 --id 1003 # single file
jira issue download-attachments PROJ-123          # all from issue
jira issue download-all-attachments PROJ-123      # issue + subtasks
```

### Search

```bash
jira search --jql "project = PROJ AND status = Open"
jira search --jql "assignee = currentUser()" --max-results 10
```

### Projects & Boards

```bash
jira project list
jira board list
jira sprint get 10
jira user me
jira field list
```

### Configuration

```bash
jira config list
jira config add --site NAME ...
jira config remove --site NAME
jira context
```

---

## 🔌 MCP Tools

When running as MCP server, these tools are available to Claude Desktop / GitHub Copilot CLI:

| Tool | Description |
|------|-------------|
| `get_issue` | Retrieve a single issue |
| `create_issue` | Create a new issue |
| `update_issue` | Update issue fields |
| `search_issues_jql` | Search issues by JQL |
| `list_attachments` | List files on an issue |
| `download_attachment` | Download a single attachment |
| `download_all_attachments` | Download attachments recursively |
| `get_all_projects` | List all projects |
| `get_sprint` | Get sprint details |
| `jira_context` | Show current configuration |

---

## 📝 Tech Stack

- **Language**: C# with .NET 8
- **CLI Framework**: System.CommandLine 2.0
- **MCP Implementation**: ModelContextProtocol 1.3.0
- **HTTP Client**: HttpClientFactory with Polly resilience
- **Distribution**: Self-contained single executable

---

## 🆘 Need Help?

- **Installation issues**: [INSTALLATION.md](INSTALLATION.md#-troubleshooting)
- **Configuration help**: [CONFIGURATION.md](CONFIGURATION.md#-configuration-troubleshooting)
- **MCP setup**: [MCP-SETUP.md](MCP-SETUP.md)
- **General troubleshooting**: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)

---

## 📝 Tech Stack

- **Language**: C# with .NET 8
- **CLI Framework**: System.CommandLine 2.0
- **MCP Implementation**: ModelContextProtocol 1.3.0
- **HTTP Client**: HttpClientFactory with Polly resilience
- **Build**: Self-contained single executable

---

## 👤 Author & Credits

**Hermes Agent** documentation suite by **Paolino Salamone**

All documentation in this repository is:
- ✅ Written and maintained by Paolino Salamone
- ✅ Licensed under MIT
- ✅ Publicly available and open-source
- ✅ Distributed via HermesAgent automated publishing workflow

For questions about documentation, reach out to:
- **Name**: Paolino Salamone
- **Project**: Hermes Agent
- **License**: MIT

---

## 📄 License

MIT License — See [LICENSE](LICENSE)

---

**Hermes Agent** • MCP Orchestration Tier  
**Author**: Paolino Salamone  
Last updated: 2025-01-15
