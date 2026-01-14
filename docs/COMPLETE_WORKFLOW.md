# Complete Workflow - Parallel Claude Architecture

## Overview

The AI West build system uses three coordinated agents:

1. **Cash (Discovery)** - Analyzes requirements, creates GitHub repo with 14 files
2. **Claude Project (Orchestrator)** - Monitors GitHub, reviews, deploys
3. **Claude Code (Builder)** - Clones repo, builds locally, commits/pushes

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  PHASE 1: DISCOVERY (Cash)                                          │
│                                                                      │
│  Trigger: "Pull my conversation with [client] from Fireflies"       │
│                                                                      │
│  1. Retrieve Fireflies transcript                                    │
│  2. Extract requirements                                             │
│  3. Generate 14 files                                                │
│  4. Create GitHub repository                                         │
│  5. Push all files                                                   │
│  6. Report to Josh                                                   │
│                                                                      │
│  Output: https://github.com/joshmartin1186/[project-name]           │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  PHASE 2: SETUP (Josh)                                              │
│                                                                      │
│  1. Open GitHub repo                                                 │
│  2. Create Claude Desktop Window #1 (Claude Project)                 │
│     - Open PROJECT_INSTRUCTIONS.md                                   │
│     - Copy entire contents                                           │
│     - Paste as instructions                                          │
│  3. Create Claude Desktop Window #2 (Claude Code)                    │
│     - Open CODE_STARTER_PROMPT.md                                    │
│     - Copy entire contents                                           │
│     - Paste to start building                                        │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  PHASE 3: PARALLEL BUILDING                                         │
│                                                                      │
│  ┌───────────────────┐         ┌───────────────────┐                │
│  │   Claude Project  │         │   Claude Code     │                │
│  │   (Orchestrator)  │         │   (Builder)       │                │
│  │                   │         │                   │                │
│  │ - GitHub only     │◄───────►│ - Local clone     │                │
│  │ - Monitors        │ GitHub  │ - Builds code     │                │
│  │ - Reviews         │ commits │ - Tests locally   │                │
│  │ - Deploys         │         │ - Commits often   │                │
│  │                   │         │ - Pushes always   │                │
│  └───────────────────┘         └───────────────────┘                │
│                                                                      │
│  Communication: GitHub commits ONLY                                  │
│  Claude Code pushes → Claude Project sees commits                    │
│  Claude Project writes issues → Claude Code pulls                    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  PHASE 4: DEPLOYMENT                                                │
│                                                                      │
│  Claude Project guides Josh through:                                 │
│  1. Connect Vercel to GitHub repo                                    │
│  2. Configure environment variables                                  │
│  3. Deploy                                                           │
│  4. Post-deploy configuration (OAuth, Stripe webhook)                │
│  5. Production testing                                               │
│  6. Client handoff                                                   │
│                                                                      │
│  Output: Live production app at https://[project].vercel.app        │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Phase Details

### Phase 1: Discovery (Cash)

**Trigger:** "Pull my conversation with [client] from Fireflies"

**Steps:**

1. **Retrieve transcript:**
```javascript
fireflies:get_transcripts({
  search: "[client name]",
  sort_by: "date",
  limit: 1
})
```

2. **Extract requirements:**
   - Client info (name, company, email)
   - Problem statement
   - Solution requirements
   - Integrations needed
   - Success criteria
   - Timeline/budget hints
   - User count (pricing tier)

3. **Determine business context:**
   - Demographic: Investor / Solopreneur / Startup
   - Pricing tier: Based on user count and brains needed
   - Productization potential

4. **Generate 14 files:**
   - Files 1-11: Specification files
   - File 12: EXECUTION_PLAN.md (overshared commands)
   - File 13: PROJECT_INSTRUCTIONS.md (for Claude Project)
   - File 14: CODE_STARTER_PROMPT.md (for Claude Code)

5. **Create GitHub repository:**
```javascript
github:create_repository({
  name: "[project-name]",
  description: "[description]",
  private: false
})
```

6. **Push all files:**
```javascript
github:push_files({
  owner: "joshmartin1186",
  repo: "[project-name]",
  branch: "main",
  files: [/* all 14 files */],
  message: "Initial project documentation"
})
```

7. **Report to Josh:**
```
Project Repository Created ✅

**Repository:** https://github.com/joshmartin1186/[project-name]

**Business Model:**
- Demographic: [X]
- Configuration: [X] Brain(s)
- Monthly: $[X]/month

**Next Steps:**
1. Open repository
2. Create TWO Claude Desktop windows
3. Window 1: Paste PROJECT_INSTRUCTIONS.md
4. Window 2: Paste CODE_STARTER_PROMPT.md
```

**Cash's job is complete.**

---

### Phase 2: Setup (Josh)

**Manual steps by Josh:**

1. Open GitHub repo in browser
2. **Window 1 - Claude Project:**
   - Open new Claude Desktop conversation
   - Navigate to PROJECT_INSTRUCTIONS.md on GitHub
   - Copy entire file contents
   - Paste into Claude as instructions
3. **Window 2 - Claude Code:**
   - Open new Claude Desktop conversation
   - Navigate to CODE_STARTER_PROMPT.md on GitHub
   - Copy entire file contents
   - Paste into Claude to start building

**Result:** Two Claude instances running in parallel

---

### Phase 3: Parallel Building

**Claude Code (Builder):**

1. Clones repository to ~/projects/[project-name]/
2. Reads EXECUTION_PLAN.md for exact commands
3. Executes tasks one by one
4. Tests each task locally
5. Commits every 2-3 tasks
6. Pushes immediately after every commit
7. Creates GitHub issues for blockers

**Claude Project (Orchestrator):**

1. Monitors GitHub commits
2. Reviews code quality when Josh asks
3. Creates issues with feedback (CRITICAL/HIGH/MEDIUM/LOW)
4. Updates spec files on GitHub if needed
5. Prepares for deployment

**Communication Protocol:**

```
Claude Code commits and pushes
           ↓
GitHub receives commits
           ↓
Claude Project reads commits
           ↓
Claude Project creates issue with feedback
           ↓
Claude Code pulls and reads issue
           ↓
Claude Code fixes and pushes
           ↓
Repeat
```

**Rules:**
- ✅ All communication via GitHub
- ✅ Claude Code commits every 2-3 tasks
- ✅ Claude Project only uses GitHub MCP
- ❌ No direct communication
- ❌ Claude Project never touches local files

---

### Phase 4: Deployment

**When Josh says "Deploy to Vercel":**

Claude Project guides through:

1. **Verify ready:**
   - Check all phases committed
   - Check no open issues

2. **Connect Vercel:**
   - Go to vercel.com/new
   - Import GitHub repo
   - Root directory: code

3. **Environment variables:**
   - Add all from API_INTEGRATIONS.md

4. **Deploy:**
   - Click Deploy
   - Wait for build

5. **Post-deploy:**
   - Update Supabase redirect URLs
   - Configure Stripe webhook
   - Redeploy if needed

6. **Test production:**
   - All critical paths
   - Authentication flow
   - Stripe checkout

7. **Client handoff:**
   - Pre-seed accounts
   - Send handoff email
   - Schedule walkthrough

---

## Timeline

| Phase | Duration | Owner |
|-------|----------|-------|
| Discovery | 30-45 min | Cash |
| Setup | 5 min | Josh |
| Foundation (Phase 1) | 2-3 hours | Claude Code |
| Core UI (Phase 2) | 3-4 hours | Claude Code |
| Automation (Phase 3) | 4-6 hours | Claude Code |
| Deploy & Handoff | 1-2 hours | Claude Project + Josh |
| **Total** | **2-3 days** | |

---

## Success Criteria

### Cash (Discovery)
- [ ] GitHub repo created with 14 files
- [ ] All files comprehensive and actionable
- [ ] EXECUTION_PLAN overshared (exact commands)
- [ ] PROJECT_INSTRUCTIONS defines GitHub-only orchestration
- [ ] CODE_STARTER_PROMPT starts with clone command

### Claude Project (Orchestrator)
- [ ] Uses GitHub MCP only
- [ ] Never accesses local files
- [ ] Reviews code thoroughly
- [ ] Categorizes issues correctly
- [ ] Guides deployment successfully

### Claude Code (Builder)
- [ ] Clones repo correctly
- [ ] Follows EXECUTION_PLAN exactly
- [ ] Commits every 2-3 tasks
- [ ] Pushes after every commit
- [ ] Tests before committing
- [ ] Creates issues for blockers

### Josh (Coordinator)
- [ ] Creates two separate Claude windows
- [ ] Pastes correct files to each
- [ ] Monitors progress
- [ ] Follows deployment guidance
- [ ] Completes client handoff

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Cash can't find transcript | Search Fireflies manually, provide transcript ID |
| Claude Code won't start | Verify CODE_STARTER_PROMPT was pasted |
| Claude Code stopped pushing | Ask Claude Code to push current work |
| Claude Project can't see code | Verify Claude Code pushed to GitHub |
| Claudes talking directly | STOP - redirect all comms through GitHub |
| Lost work | `git add . && git commit -m "Save" && git push` |
| Build failed on Vercel | Check build logs, fix errors, push to trigger redeploy |
| Webhook not working | Verify endpoint URL and secret in Stripe + Vercel |