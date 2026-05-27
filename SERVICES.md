# JiraCli MCP Services Documentation

**Hermes Agent** • MIT License

**Author**: Paolino Salamone

> Complete reference of all MCP tools implemented in jira-cli server.

---

## 🎯 Overview

The jira-cli MCP server exposes the following tools for use with Claude Desktop and GitHub Copilot CLI:

- **9 Issue Management Tools**: Get, create, update, search, transition, comment, worklog, attach
- **3 Attachment Tools**: List, download single, download recursive
- **4 Project/Context Tools**: Project list, sprint details, user info, current context
- **1 Field Reference Tool**: List available fields

**Total**: 17 MCP tools available

---

## 📋 Issue Management Tools

### 1. `get_issue`

**Purpose**: Retrieve a single Jira issue with full details

**Input**:
```json
{
  "issue_key": "PROJ-123"
}
```

**Output**:
```json
{
  "key": "PROJ-123",
  "summary": "Fix login bug",
  "description": "Users cannot login...",
  "status": "In Progress",
  "assignee": "john@example.com",
  "created": "2025-01-10T10:00:00Z",
  "updated": "2025-01-15T14:30:00Z",
  "fields": {
    "customfield_10000": "value"
  }
}
```

**Example Usage** (Claude):
```
Get me the details of PROJ-456
```

---

### 2. `create_issue`

**Purpose**: Create a new Jira issue

**Input**:
```json
{
  "project": "PROJ",
  "type": "Task",
  "summary": "Add dark mode",
  "description": "Implement dark mode theme",
  "assignee": "john@example.com"
}
```

**Output**:
```json
{
  "key": "PROJ-999",
  "id": "10999",
  "summary": "Add dark mode"
}
```

**Example Usage** (Claude):
```
Create a new bug in PROJ project titled "Database connection fails"
```

---

### 3. `update_issue`

**Purpose**: Update fields on an existing issue

**Input**:
```json
{
  "issue_key": "PROJ-123",
  "summary": "New title",
  "description": "Updated description",
  "status": "Done",
  "assignee": "jane@example.com"
}
```

**Output**:
```json
{
  "success": true,
  "message": "PROJ-123 updated"
}
```

**Example Usage** (Claude):
```
Assign PROJ-789 to me and mark as In Progress
```

---

### 4. `search_issues_jql`

**Purpose**: Search issues using Jira Query Language (JQL)

**Input**:
```json
{
  "jql": "project = PROJ AND assignee = currentUser() AND status != Done",
  "max_results": 10
}
```

**Output**:
```json
{
  "total": 3,
  "issues": [
    {
      "key": "PROJ-100",
      "summary": "Task 1",
      "status": "In Progress"
    },
    {
      "key": "PROJ-101",
      "summary": "Task 2",
      "status": "Open"
    }
  ]
}
```

**JQL Examples**:
- `assignee = currentUser()` - My issues
- `status = "In Progress"` - Active work
- `created >= -7d` - Last 7 days
- `priority = Highest` - Critical issues
- `project = PROJ AND type = Bug` - Project bugs

**Example Usage** (Claude):
```
Show me all open bugs assigned to me in the last week
```

---

### 5. `list_issue_transitions`

**Purpose**: Get available status transitions for an issue

**Input**:
```json
{
  "issue_key": "PROJ-123"
}
```

**Output**:
```json
{
  "transitions": [
    {
      "id": "11",
      "name": "Start Progress",
      "to": "In Progress"
    },
    {
      "id": "41",
      "name": "Done",
      "to": "Done"
    }
  ]
}
```

---

### 6. `transition_issue`

**Purpose**: Move an issue to a new status

**Input**:
```json
{
  "issue_key": "PROJ-123",
  "transition": "In Progress"
}
```

**Output**:
```json
{
  "success": true,
  "message": "PROJ-123 transitioned to In Progress"
}
```

**Example Usage** (Claude):
```
Move PROJ-500 to Done
```

---

### 7. `add_issue_comment`

**Purpose**: Add a comment to an issue

**Input**:
```json
{
  "issue_key": "PROJ-123",
  "comment": "Fixed in commit abc123"
}
```

**Output**:
```json
{
  "success": true,
  "comment_id": "10999"
}
```

**Example Usage** (Claude):
```
Comment on PROJ-456: "Ready for testing on staging"
```

---

### 8. `add_worklog`

**Purpose**: Log time spent on an issue

**Input**:
```json
{
  "issue_key": "PROJ-123",
  "time_spent": "2h 30m",
  "comment": "Debugging API integration"
}
```

**Output**:
```json
{
  "success": true,
  "worklog_id": "10999"
}
```

**Format**:
- `1h` - 1 hour
- `30m` - 30 minutes
- `2h 30m` - 2.5 hours
- `1d` - 1 day (8 hours)

**Example Usage** (Claude):
```
Log 3 hours on PROJ-789 for "Code review"
```

---

### 9. `attach_file_to_issue`

**Purpose**: Upload a file attachment to an issue

**Input**:
```json
{
  "issue_key": "PROJ-123",
  "file_path": "/path/to/screenshot.png"
}
```

**Output**:
```json
{
  "success": true,
  "attachment_id": "10999"
}
```

**Example Usage** (Claude):
```
Attach the error screenshot to PROJ-111
```

---

## 📎 Attachment Tools

### 10. `list_attachments`

**Purpose**: List all attachments on an issue

**Input**:
```json
{
  "issue_key": "PROJ-123"
}
```

**Output**:
```json
{
  "attachments": [
    {
      "id": "10999",
      "filename": "screenshot.png",
      "size": 245632,
      "created": "2025-01-10T10:00:00Z"
    }
  ]
}
```

---

### 11. `download_attachment`

**Purpose**: Download a single attachment file

**Input**:
```json
{
  "issue_key": "PROJ-123",
  "attachment_id": "10999"
}
```

**Output**:
```json
{
  "success": true,
  "filepath": "/tmp/screenshot.png",
  "size": 245632
}
```

---

### 12. `download_all_attachments`

**Purpose**: Download all attachments from an issue and subtasks recursively

**Input**:
```json
{
  "issue_key": "PROJ-123"
}
```

**Output**:
```json
{
  "success": true,
  "files_downloaded": 5,
  "directory": "/tmp/PROJ-123-attachments"
}
```

---

## 🎯 Project & Context Tools

### 13. `get_all_projects`

**Purpose**: List all projects the user can access

**Output**:
```json
{
  "projects": [
    {
      "key": "PROJ",
      "name": "Main Project",
      "type": "software"
    },
    {
      "key": "INFRA",
      "name": "Infrastructure",
      "type": "software"
    }
  ]
}
```

**Example Usage** (Claude):
```
Show me all available projects
```

---

### 14. `get_sprint`

**Purpose**: Get details of a sprint (Agile boards only)

**Input**:
```json
{
  "sprint_id": "10"
}
```

**Output**:
```json
{
  "sprint_id": "10",
  "name": "Sprint 45",
  "state": "ACTIVE",
  "start_date": "2025-01-13T09:00:00Z",
  "end_date": "2025-01-27T09:00:00Z",
  "issues": [
    {
      "key": "PROJ-100",
      "summary": "Task 1",
      "status": "In Progress"
    }
  ]
}
```

---

### 15. `get_user_me`

**Purpose**: Get current authenticated user info

**Output**:
```json
{
  "account_id": "user123",
  "email": "user@example.com",
  "name": "John Doe",
  "display_name": "John Doe"
}
```

**Example Usage** (Claude):
```
Who am I logged in as?
```

---

### 16. `jira_context`

**Purpose**: Show current Jira configuration and connection status

**Output**:
```json
{
  "default_site": "myco",
  "url": "https://myco.atlassian.net",
  "auth_type": "basic",
  "api_version": 3,
  "user": "user@example.com",
  "connected": true
}
```

**Example Usage** (Claude):
```
Show my Jira configuration
```

---

### 17. `get_fields`

**Purpose**: List all available fields in Jira

**Output**:
```json
{
  "fields": [
    {
      "id": "summary",
      "name": "Summary",
      "type": "text"
    },
    {
      "id": "customfield_10000",
      "name": "Story Points",
      "type": "number"
    }
  ]
}
```

---

## 🔄 Common Workflows

### Workflow 1: Create and Track an Issue

```
Claude: "Create a bug in PROJECT with title 'Login fails on mobile'"
→ Tool: create_issue (PROJECT, Bug, "Login fails on mobile")
→ Result: PROJ-999 created

Claude: "Assign PROJ-999 to john and log 1h"
→ Tool: update_issue (PROJ-999, assignee=john)
→ Tool: add_worklog (PROJ-999, 1h, "Initial investigation")
→ Result: Done
```

### Workflow 2: Search and Update

```
Claude: "Find all my open issues and mark them as In Progress"
→ Tool: search_issues_jql (assignee = currentUser() AND status = Open)
→ For each issue: transition_issue (issue_key, "In Progress")
→ Result: All issues transitioned
```

### Workflow 3: Get Issue with Attachments

```
Claude: "Show me PROJ-123 details and download all attachments"
→ Tool: get_issue (PROJ-123)
→ Tool: download_all_attachments (PROJ-123)
→ Result: Issue details + files in /tmp/PROJ-123-attachments/
```

---

## 🚨 Error Handling

All tools return errors in this format:

```json
{
  "error": true,
  "message": "Issue PROJ-999 not found",
  "code": "NOT_FOUND"
}
```

**Common Error Codes**:
- `NOT_FOUND` - Issue, project, or resource not found
- `UNAUTHORIZED` - Authentication failed
- `FORBIDDEN` - No permission to access resource
- `BAD_REQUEST` - Invalid input parameters
- `SERVER_ERROR` - Jira server error

---

## 💡 Tips for Claude

1. **Use search**: Most powerful tool for finding issues
   - `assignee = currentUser()` - My work
   - `updated >= -1d` - Recent activity
   - `status = "To Do"` - Next tasks

2. **Batch operations**: Claude can call multiple tools
   - Find issues → Update all → Add comments

3. **Provide context**: Always include issue key in responses

4. **Format time**: Use `1h`, `2h 30m`, `1d` format

5. **Read errors**: Error messages guide next steps

---

## 📞 Support

For issues or questions about MCP tools:

1. Check [MCP-SETUP.md](MCP-SETUP.md) for connection issues
2. Verify Jira credentials in [CONFIGURATION.md](CONFIGURATION.md)
3. Check tool syntax in this guide
4. Review [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for common problems

---

**License**: MIT  
**Hermes Agent** • MCP Services Reference  

**Author**: Paolino Salamone
**Last Updated**: 2025-01-15
