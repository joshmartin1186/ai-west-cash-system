# Complete Workflow - Supabase-First Two-Claude Architecture (v6)

## Overview

The AI West build system uses two coordinated agents:

1. **Cash (Discovery)** - Analyzes requirements, creates Supabase project, creates GitHub repo, sets up local folder, generates starter prompt
2. **Claude Code (Builder)** - Builds, commits, deploys, handles client handoff

**Key Change in v6:** Supabase project is created and schema applied BEFORE GitHub repo creation.

**No middleman. No orchestrator. Supabase-first. Direct handoff.**

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  PHASE 1: DISCOVERY & COMPLETE SETUP (Cash)                         │
│                                                                      │
│  Trigger: "Pull my conversation with [client] from Fireflies"       │
│                                                                      │
│  Cash executes in this exact order:                                  │
│                                                                      │
│  1. Retrieve Fireflies transcript                                    │
│  2. Extract requirements & determine business context                │
│  3. CREATE SUPABASE PROJECT  ◄── Supabase First!                    │
│     - supabase:list_organizations                                    │
│     - supabase:get_cost / confirm_cost                               │
│     - supabase:create_project                                        │
│     - supabase:get_project_url                                       │
│     - supabase:get_publishable_keys                                  │
│  4. APPLY DATABASE SCHEMA                                            │
│     - supabase:apply_migration (complete schema)                     │
│     - supabase:list_tables (verify)                                  │
│  5. Generate 13 files (with Supabase credentials embedded)          │
│  6. Create GitHub repository                                         │
│  7. Push all 13 files to GitHub                                      │
│  8. Create local folder ~/projects/[project-name]/                  │
│  9. Clone repo to local folder                                       │
│  10. Verify local setup                                              │
│  11. Deliver starter prompt to Josh (with all credentials)          │
│                                                                      │
│  Output:                                                             │
│  - Supabase: https://[ref].supabase.co (schema applied)             │
│  - GitHub: https://github.com/joshmartin1186/[project-name]         │
│  - Local: ~/projects/[project-name]/                                 │
│  - Prompt: Ready to paste into Claude Code                          │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  PHASE 2: BUILDING (Josh + Claude Code)                             │
│                                                                      │
│  1. Josh opens Claude Code (Claude Desktop with MCP tools)          │
│  2. Josh pastes the starter prompt Cash provided                     │
│  3. Claude Code takes over:                                          │
│                                                                      │
│     ┌───────────────────────────────────────────────┐                │
│     │           Claude Code (Builder)               │                │
│     │                                               │                │
│     │  - Database already ready (Supabase)         │                │
│     │  - Works from ~/projects/[project-name]/     │                │
│     │  - Follows EXECUTION_PLAN.md exactly          │                │
│     │  - Builds all features                        │                │
│     │  - Commits every 2-3 tasks                    │                │
│     │  - Pushes immediately after commit            │                │
│     │  - Deploys to Vercel when ready              │                │
│     │  - Handles client handoff                     │                │
│     └───────────────────────────────────────────────┘                │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  OUTPUT: PRODUCTION APP                                             │
│                                                                      │
│  - Live at https://[project-name].vercel.app                        │
│  - Database at https://[ref].supabase.co                            │
│  - Client pre-seeded as Owner                                        │
│  - Josh pre-seeded as Developer                                      │
│  - Handoff email sent                                                │
│  - Walkthrough scheduled                                             │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Phase 1 Details: Cash's Complete Workflow

### Step 1: Retrieve Requirements

```javascript
fireflies:get_transcripts({
  search: "[client name]",
  sort_by: "date",
  limit: 1
})
```

**Extract:**
- Client info (name, company, email)
- Problem statement
- Solution requirements
- Integrations needed
- Success criteria
- Timeline/budget hints
- User count (pricing tier)

---

### Step 2: Determine Business Context

- Demographic: Investor / Solopreneur / Startup
- Pricing tier: Based on user count and brains needed
- Productization potential

---

### Step 3: Create Supabase Project (NEW IN v6)

**Check organization:**
```javascript
supabase:list_organizations()
```

**Get and confirm cost:**
```javascript
supabase:get_cost({
  organization_id: "[org-id]",
  type: "project"
})

supabase:confirm_cost({
  type: "project",
  amount: [amount],
  recurrence: "monthly"
})
```

**Create project:**
```javascript
supabase:create_project({
  name: "[project-name]",
  organization_id: "[org-id]",
  region: "us-east-1",
  confirm_cost_id: "[confirmation-id]"
})
```

**Wait for initialization, then get credentials:**
```javascript
supabase:get_project({ id: "[project-id]" })
supabase:get_project_url({ project_id: "[project-id]" })
supabase:get_publishable_keys({ project_id: "[project-id]" })
```

**Capture for later:**
- SUPABASE_PROJECT_ID
- SUPABASE_URL
- SUPABASE_ANON_KEY
- SUPABASE_SERVICE_ROLE_KEY (ask Josh if needed from dashboard)

---

### Step 4: Apply Database Schema

```javascript
supabase:apply_migration({
  project_id: "[project-id]",
  name: "initial_schema",
  query: "[complete SQL schema]"
})
```

**Verify:**
```javascript
supabase:list_tables({
  project_id: "[project-id]",
  schemas: ["public"]
})
```

---

### Step 5: Generate 13 Files

Generate with Supabase credentials embedded:

1. PROJECT_OVERVIEW.md
2. TECHNICAL_ARCHITECTURE.md
3. DATABASE_SCHEMA.md (SQL that was applied)
4. API_INTEGRATIONS.md (includes Supabase URL)
5. UI_SPECIFICATIONS.md
6. BUILD_PHASES.md
7. DEBUGGING_GUIDE.md
8. CLIENT_REQUIREMENTS.md
9. PRODUCTIZATION_GUIDE.md
10. DEPLOYMENT_CHECKLIST.md
11. AI_WEST_DESIGN_SYSTEM.md
12. EXECUTION_PLAN.md
13. CODE_STARTER_PROMPT.md (includes all credentials)

---

### Steps 6-10: GitHub + Local Setup

```javascript
// Step 6: Create repo
github:create_repository({
  name: "[project-name]",
  description: "AI West Platform - [description]",
  private: false
})

// Step 7: Push files
github:push_files({
  owner: "joshmartin1186",
  repo: "[project-name]",
  branch: "main",
  files: [/* all 13 files */],
  message: "Initial project documentation"
})

// Step 8: Create local folder
Desktop Commander:create_directory({
  path: "/Users/josh/projects"
})

// Step 9: Clone repo
Desktop Commander:start_process({
  command: "cd /Users/josh/projects && git clone https://github.com/joshmartin1186/[project-name].git",
  timeout_ms: 60000
})

// Step 10: Verify
Desktop Commander:list_directory({
  path: "/Users/josh/projects/[project-name]"
})
```

---

### Step 11: Deliver Starter Prompt

```
Project Ready for Claude Code ✅

**Supabase Project:** [project-name]
**Supabase URL:** https://[ref].supabase.co
**GitHub Repository:** https://github.com/joshmartin1186/[project-name]
**Local Folder:** ~/projects/[project-name]/

**Database Status:**
✅ Supabase project created
✅ Schema applied (all tables ready)
✅ RLS policies configured

---

## Claude Code Starter Prompt

[Complete prompt with credentials and first steps]

---

Ready to build! 🚀
```

**Cash's job is complete.**

---

## Phase 2: Building

**Josh's only task:**
1. Open Claude Code
2. Paste starter prompt
3. Let Claude Code build

**Claude Code handles:**
- Foundation (Next.js, deps, auth) - Database already ready!
- Core UI (dashboard, features, settings)
- Automation (integrations, AI, webhooks)
- Deployment (Vercel, Stripe, handoff)

---

## Timeline

| Phase | Duration | Owner |
|-------|----------|-------|
| Discovery & Setup | 30-45 min | Cash |
| Paste Prompt | 1 min | Josh |
| Foundation (Phase 1) | 2-3 hours | Claude Code |
| Core UI (Phase 2) | 3-4 hours | Claude Code |
| Automation (Phase 3) | 4-6 hours | Claude Code |
| Deploy & Handoff (Phase 4) | 1-2 hours | Claude Code |
| **Total** | **2-3 days** | |

---

## Why Supabase-First Works Better

**Old Flow (v5):**
1. Generate files
2. Create GitHub
3. Clone local
4. Claude Code creates Supabase manually
5. Schema applied during build

**New Flow (v6):**
1. Create Supabase ← Database ready from start
2. Apply schema ← Tables ready before code
3. Generate files (with credentials)
4. Create GitHub
5. Clone local
6. Claude Code starts building immediately

**Benefits:**
- No manual Supabase setup during build
- Credentials embedded in starter prompt
- Schema version-controlled via migrations
- Claude Code can focus purely on code
- Fewer blockers during build phase