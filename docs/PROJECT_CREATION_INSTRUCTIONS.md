# Project Creation Instructions (v5)

## Overview

This document describes how Cash creates a complete project setup for Claude Code.

**v5 Changes:**
- Removed Claude Project orchestrator
- Cash now creates both GitHub repo AND local folder
- Direct handoff to Claude Code

---

## Cash's Deliverables

When triggered, Cash creates:

1. **GitHub Repository** with 13 files
2. **Local Project Folder** cloned from GitHub
3. **Claude Code Starter Prompt** ready to paste

---

## Step-by-Step Process

### 1. Retrieve Requirements

**From Fireflies:**
```javascript
fireflies:get_transcripts({
  search: "[client name]",
  sort_by: "date",
  limit: 1
})
```

**From Other Sources:**
- View uploaded documents
- Extract from conversation

### 2. Generate 13 Files

Using templates from `docs/13_FILE_TEMPLATE.md`:

**Specification Files (1-11):**
1. PROJECT_OVERVIEW.md
2. TECHNICAL_ARCHITECTURE.md
3. DATABASE_SCHEMA.md
4. API_INTEGRATIONS.md
5. UI_SPECIFICATIONS.md
6. BUILD_PHASES.md
7. DEBUGGING_GUIDE.md
8. CLIENT_REQUIREMENTS.md
9. PRODUCTIZATION_GUIDE.md
10. DEPLOYMENT_CHECKLIST.md
11. AI_WEST_DESIGN_SYSTEM.md

**Execution Files (12-13):**
12. EXECUTION_PLAN.md
13. CODE_STARTER_PROMPT.md

### 3. Create GitHub Repository

```javascript
github:create_repository({
  name: "[project-name]",
  description: "AI West Platform - [description]",
  private: false
})
```

### 4. Push All Files

```javascript
github:push_files({
  owner: "joshmartin1186",
  repo: "[project-name]",
  branch: "main",
  files: [/* all 13 files */],
  message: "Initial project documentation"
})
```

### 5. Create Local Folder

```javascript
Desktop Commander:create_directory({
  path: "/Users/josh/projects/[project-name]"
})
```

### 6. Clone Repository

```javascript
Desktop Commander:start_process({
  command: "cd /Users/josh/projects && git clone https://github.com/joshmartin1186/[project-name].git",
  timeout_ms: 60000
})
```

### 7. Verify Setup

```javascript
Desktop Commander:list_directory({
  path: "/Users/josh/projects/[project-name]"
})
```

### 8. Report to Josh

```
Project Ready for Claude Code ✅

**GitHub Repository:** https://github.com/joshmartin1186/[project-name]
**Local Folder:** ~/projects/[project-name]/

**What We're Building:**
[Summary]

**Business Model:**
- Demographic: [X]
- Configuration: [X] Brain(s)
- Monthly: $[X]/month

---

## Claude Code Starter Prompt

[Generated prompt]

---

Ready to build! 🚀
```

---

## File Quality Standards

### EXECUTION_PLAN.md Must Include:

- Every terminal command needed
- Full file contents for every file created
- Expected output after each command
- Verification steps
- Troubleshooting tips
- Commit points (every 2-3 tasks)

### CODE_STARTER_PROMPT.md Must Include:

- Project location (GitHub + local)
- First steps to verify setup
- Build protocol (commit every 2-3 tasks)
- What to do when blocked
- Reference file list
- Clear "START NOW" instruction

---

## Common Issues

### GitHub Push Fails

Retry once. If still fails:
"GitHub push hit a snag. Check if repo exists at github.com/joshmartin1186/[project-name]"

### Desktop Commander Fails

"Couldn't create local folder. Run manually:
```bash
cd ~/projects
git clone https://github.com/joshmartin1186/[project-name].git
```"

### Fireflies Timeout

"Having trouble with Fireflies. Can you:
1. Confirm client name
2. Or paste requirements directly"