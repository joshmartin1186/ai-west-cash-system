# Complete Workflow - Streamlined Two-Claude Architecture (v5)

## Overview

The AI West build system uses two coordinated agents:

1. **Cash (Discovery)** - Analyzes requirements, creates GitHub repo + local folder, generates starter prompt
2. **Claude Code (Builder)** - Builds, commits, deploys - the only builder needed

**No middleman. No orchestrator. Direct handoff.**

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  PHASE 1: DISCOVERY & SETUP (Cash)                                  │
│                                                                      │
│  Trigger: "Pull my conversation with [client] from Fireflies"       │
│                                                                      │
│  Cash does ALL of this:                                              │
│  1. Retrieve Fireflies transcript                                    │
│  2. Extract requirements                                             │
│  3. Generate 13 files                                                │
│  4. Create GitHub repository                                         │
│  5. Push all files to GitHub                                         │
│  6. Create local folder ~/projects/[project-name]/                  │
│  7. Clone repo to local folder                                       │
│  8. Generate Claude Code starter prompt                              │
│  9. Report to Josh                                                   │
│                                                                      │
│  Output:                                                             │
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
│  2. Josh pastes the CODE_STARTER_PROMPT.md content                   │
│  3. Claude Code takes over:                                          │
│                                                                      │
│     ┌───────────────────────────────────────────┐                    │
│     │           Claude Code (Builder)           │                    │
│     │                                           │                    │
│     │  - Works from ~/projects/[project-name]/ │                    │
│     │  - Follows EXECUTION_PLAN.md exactly      │                    │
│     │  - Builds all features                    │                    │
│     │  - Commits every 2-3 tasks                │                    │
│     │  - Pushes immediately after commit        │                    │
│     │  - Tests before committing                │                    │
│     │  - Deploys to Vercel when ready          │                    │
│     │  - Handles client handoff                 │                    │
│     └───────────────────────────────────────────┘                    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  OUTPUT: PRODUCTION APP                                             │
│                                                                      │
│  - Live at https://[project-name].vercel.app                        │
│  - Client pre-seeded as Owner                                        │
│  - Josh pre-seeded as Developer                                      │
│  - Handoff email sent                                                │
│  - Walkthrough scheduled                                             │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Phase Details

### Phase 1: Discovery & Setup (Cash)

**Trigger:** "Pull my conversation with [client] from Fireflies and create a project"

**Cash executes this exact sequence:**

#### Step 1: Retrieve Transcript
```javascript
fireflies:get_transcripts({
  search: "[client name]",
  sort_by: "date",
  limit: 1
})
```

#### Step 2: Extract Requirements
- Client info (name, company, email)
- Problem statement
- Solution requirements
- Integrations needed
- Success criteria
- Timeline/budget hints
- User count (pricing tier)

#### Step 3: Determine Business Context
- Demographic: Investor / Solopreneur / Startup
- Pricing tier: Based on user count and brains needed
- Productization potential

#### Step 4: Generate 13 Files
- Files 1-11: Specification files
- File 12: EXECUTION_PLAN.md (overshared commands)
- File 13: CODE_STARTER_PROMPT.md (for Claude Code)

#### Step 5: Create GitHub Repository
```javascript
github:create_repository({
  name: "[project-name]",
  description: "[description]",
  private: false
})
```

#### Step 6: Push All Files to GitHub
```javascript
github:push_files({
  owner: "joshmartin1186",
  repo: "[project-name]",
  branch: "main",
  files: [/* all 13 files */],
  message: "Initial project documentation"
})
```

#### Step 7: Create Local Project Folder
```javascript
// Using Desktop Commander
Desktop Commander:create_directory({
  path: "/Users/josh/projects/[project-name]"
})
```

#### Step 8: Clone Repo to Local
```javascript
Desktop Commander:start_process({
  command: "cd /Users/josh/projects && git clone https://github.com/joshmartin1186/[project-name].git",
  timeout_ms: 30000
})
```

#### Step 9: Report to Josh
```
Project Ready for Claude Code ✅

**GitHub Repository:** https://github.com/joshmartin1186/[project-name]
**Local Folder:** ~/projects/[project-name]/

**What's Built:** [2-3 sentence summary]

**Business Model:**
- Demographic: [Investor/Solopreneur/Startup]
- Configuration: [X] Brain(s)
- Monthly: $[X]/month
- Productization: $[Y]/month potential from [N] similar customers

**Timeline:** 2-3 days with 5-7 deploy cycles

---

## Claude Code Starter Prompt

Paste this into Claude Code to begin building:

[Generated prompt with project-specific details]

---

Ready to build! 🚀
```

**Cash's job is complete.**

---

### Phase 2: Building (Josh + Claude Code)

**Josh's only task:**
1. Open Claude Code (Claude Desktop with MCP tools)
2. Paste the CODE_STARTER_PROMPT.md content
3. Let Claude Code build

**Claude Code handles EVERYTHING:**

1. **Verify Setup**
   - Check local folder exists
   - Verify git remote is correct
   - Read EXECUTION_PLAN.md

2. **Build Phase 1: Foundation**
   - Initialize Next.js
   - Install dependencies
   - Set up Supabase
   - Create database schema
   - Implement auth
   - Commit every 2-3 tasks
   - Push immediately

3. **Build Phase 2: Core UI**
   - Dashboard
   - Feature pages
   - Settings
   - User management
   - Commit every 2-3 tasks
   - Push immediately

4. **Build Phase 3: Automation**
   - External integrations
   - AI features
   - Webhooks
   - System logging
   - Commit every 2-3 tasks
   - Push immediately

5. **Build Phase 4: Deploy & Handoff**
   - Deploy to Vercel
   - Configure production
   - Set up Stripe
   - Pre-seed accounts
   - Send handoff email
   - Schedule walkthrough

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

## Success Criteria

### Cash (Discovery & Setup)
- [ ] GitHub repo created with 13 files
- [ ] Local folder created and populated
- [ ] Claude Code starter prompt generated
- [ ] All files comprehensive and actionable
- [ ] EXECUTION_PLAN overshared (exact commands)

### Claude Code (Builder)
- [ ] Works from local clone
- [ ] Follows EXECUTION_PLAN exactly
- [ ] Commits every 2-3 tasks
- [ ] Pushes after every commit
- [ ] Tests before committing
- [ ] Deploys to Vercel
- [ ] Completes client handoff

### Josh (Trigger & Monitor)
- [ ] Triggers Cash with client name
- [ ] Pastes prompt into Claude Code
- [ ] Monitors progress
- [ ] Provides Supabase/Stripe credentials when asked
- [ ] Reviews final deployment

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Cash can't find transcript | Search Fireflies manually, provide transcript ID |
| Local folder not created | Run Desktop Commander manually |
| Claude Code won't start | Verify CODE_STARTER_PROMPT was pasted |
| Claude Code stopped pushing | Ask Claude Code to push current work |
| Lost work | `git add . && git commit -m "Save" && git push` |
| Build failed on Vercel | Check build logs, fix errors, push to trigger redeploy |
| Webhook not working | Verify endpoint URL and secret in Stripe + Vercel |

---

## Why This Works Better

**Old System (3 Claudes):**
- Cash creates files
- Claude Project monitors and reviews
- Claude Code builds
- Communication via GitHub
- Overhead of coordination

**New System (2 Claudes):**
- Cash creates files AND sets up local environment
- Claude Code builds AND deploys AND hands off
- No middleman
- Direct handoff
- Faster execution

**The Claude Project orchestrator was adding friction without adding value.**