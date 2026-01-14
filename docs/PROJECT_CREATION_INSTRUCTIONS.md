# Project Creation Instructions (v6)

## Overview

This document describes how Cash creates a complete project setup for Claude Code, including Supabase-first database creation.

**v6 Flow:**
1. Supabase project created FIRST
2. Database schema applied via migration
3. GitHub repo created with Supabase credentials embedded
4. Local folder created and cloned
5. Starter prompt delivered with all credentials

---

## Cash's Deliverables

When triggered, Cash creates:

1. **Supabase Project** with schema applied
2. **GitHub Repository** with 13 files (credentials embedded)
3. **Local Project Folder** cloned from GitHub
4. **Claude Code Starter Prompt** with all credentials

---

## Step-by-Step Process

### 1. Retrieve Requirements

```javascript
fireflies:get_transcripts({
  search: "[client name]",
  sort_by: "date",
  limit: 1
})
```

### 2. Determine Business Context

- Demographic
- Pricing tier
- Brain configuration
- Productization potential

### 3. Create Supabase Project

```javascript
// Get org
supabase:list_organizations()

// Confirm cost
supabase:get_cost({ organization_id: "[org-id]", type: "project" })
supabase:confirm_cost({ type: "project", amount: [X], recurrence: "monthly" })

// Create
supabase:create_project({
  name: "[project-name]",
  organization_id: "[org-id]",
  region: "us-east-1",
  confirm_cost_id: "[id]"
})

// Wait for ACTIVE status, then get credentials
supabase:get_project({ id: "[project-id]" })
supabase:get_project_url({ project_id: "[project-id]" })
supabase:get_publishable_keys({ project_id: "[project-id]" })
```

### 4. Apply Database Schema

```javascript
supabase:apply_migration({
  project_id: "[project-id]",
  name: "initial_schema",
  query: "[complete SQL]"
})

supabase:list_tables({
  project_id: "[project-id]",
  schemas: ["public"]
})
```

### 5. Generate 13 Files

With Supabase credentials embedded in:
- API_INTEGRATIONS.md
- EXECUTION_PLAN.md
- CODE_STARTER_PROMPT.md

### 6-7. Create GitHub Repo & Push Files

```javascript
github:create_repository({ name: "[project-name]", private: false })
github:push_files({ owner: "joshmartin1186", repo: "[project-name]", files: [...] })
```

### 8-10. Create Local Folder & Clone

```javascript
Desktop Commander:create_directory({ path: "/Users/josh/projects" })
Desktop Commander:start_process({ command: "cd /Users/josh/projects && git clone ..." })
Desktop Commander:list_directory({ path: "/Users/josh/projects/[project-name]" })
```

### 11. Deliver Starter Prompt

Complete prompt with:
- Supabase URL and credentials
- GitHub repo URL
- Local folder path
- Database status (tables ready)
- First steps

---

## File Quality Standards

### CODE_STARTER_PROMPT.md Must Include:

- Supabase URL and credentials
- Database status (✅ ready)
- List of available tables
- Environment variables block
- Clear "database is ready, skip creation" note

### EXECUTION_PLAN.md Must:

- Note that database is already created
- Skip any "create Supabase project" tasks
- Skip any "run migrations" tasks
- Start with Next.js initialization

---

## Common Issues

### Supabase Project Not Initializing

Wait 2-3 minutes, check status:
```javascript
supabase:get_project({ id: "[project-id]" })
```

### Can't Get Service Role Key

Ask Josh: "I need the service role key from Supabase Dashboard → Settings → API"

### Migration Fails

Check SQL syntax, table dependencies, missing extensions.