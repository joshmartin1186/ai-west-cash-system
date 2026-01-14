# AI West - Cash v4 System

**Parallel Claude Development System for Production Applications**

Cash v4 is AI West's proprietary system for coordinating two parallel Claude instances to build production applications in 2-3 days. This repository contains all operating instructions, workflow documentation, and templates.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      CASH (Discovery)                        │
│                                                              │
│   Analyzes requirements → Creates GitHub repo with 14 files  │
│                                                              │
│                           ↓                                  │
│                      GITHUB REPO                             │
│                    (Source of Truth)                         │
│                           ↓                                  │
│              ┌───────────┴───────────┐                       │
│              ↓                       ↓                       │
│   ┌─────────────────┐     ┌─────────────────┐               │
│   │  Claude Project │     │   Claude Code   │               │
│   │  (Orchestrator) │     │    (Builder)    │               │
│   │                 │     │                 │               │
│   │ - GitHub only   │     │ - Local clone   │               │
│   │ - Monitors      │←───→│ - Commits       │               │
│   │ - Reviews       │     │ - Pushes        │               │
│   │ - Deploys       │     │ - Builds        │               │
│   └─────────────────┘     └─────────────────┘               │
│              ↑                       ↑                       │
│              └───────────┬───────────┘                       │
│                          ↓                                   │
│                Communication via                             │
│                GitHub commits ONLY                           │
└─────────────────────────────────────────────────────────────┘
```

**Three Coordinated Agents:**
1. **Cash (Discovery)** - Analyzes requirements, creates GitHub repo with 14 files
2. **Claude Project (Orchestrator)** - Monitors GitHub, reviews, deploys (NEVER touches local files)
3. **Claude Code (Builder)** - Clones repo, builds locally, commits/pushes continuously

**Communication:** GitHub commits ONLY (no direct communication between Claudes)

**Timeline:** 2-3 days from requirements to production (5-7 deploy cycles)

---

## The 14 Files

Cash generates 14 files for every project:

### Specification Files (1-11)

| File | Purpose |
|------|---------||
| PROJECT_OVERVIEW.md | Client info, problem/solution, business model, timeline |
| TECHNICAL_ARCHITECTURE.md | Tech stack, Brain integration, multi-tenant design |
| DATABASE_SCHEMA.md | Complete SQL for all tables including Brain-specific |
| API_INTEGRATIONS.md | External services, Stripe setup, OAuth config |
| UI_SPECIFICATIONS.md | Page layouts, component library, user flows |
| BUILD_PHASES.md | Development phases with atomic tasks |
| DEBUGGING_GUIDE.md | Non-technical troubleshooting |
| CLIENT_REQUIREMENTS.md | Client-specific config, ICP, business rules |
| PRODUCTIZATION_GUIDE.md | SaaS opportunity, pricing, launch strategy |
| DEPLOYMENT_CHECKLIST.md | Iterative deployment procedures |
| AI_WEST_DESIGN_SYSTEM.md | Copy master file exactly, never modify |

### Execution Files (12-14)

| File | Purpose |
|------|---------||
| EXECUTION_PLAN.md | Task-by-task build sequence with commands |
| PROJECT_INSTRUCTIONS.md | Paste into Claude Project (orchestrator context) |
| CODE_STARTER_PROMPT.md | Paste into Claude Code (builder context) |

---

## Quick Start

### Phase 1: Discovery (Cash)

1. **Trigger:** "Pull my conversation with [client] from Fireflies"
2. **Cash:** Retrieves transcript, extracts requirements
3. **Cash:** Generates all 14 files
4. **Cash:** Creates GitHub repository
5. **Cash:** Pushes all files to repo
6. **Cash:** Reports repo URL to Josh

### Phase 2: Setup (Josh)

1. **Create TWO separate Claude Desktop windows**
2. **Window 1 (Claude Project):**
   - Open PROJECT_INSTRUCTIONS.md from GitHub
   - Copy entire contents
   - Paste as instructions
3. **Window 2 (Claude Code):**
   - Open CODE_STARTER_PROMPT.md from GitHub
   - Copy entire contents
   - Paste to start building

### Phase 3: Building (Automated)

```
Claude Code commits → GitHub → Claude Project sees commits
                                        ↓
                      Claude Project reviews/writes feedback to GitHub
                                        ↓
                      Claude Code pulls and continues
```

**Rules:**
- ✅ All communication via GitHub only
- ✅ Claude Code pushes every 2-3 tasks
- ✅ Claude Project reads/writes GitHub only
- ❌ Never direct communication between Claudes
- ❌ Claude Project never touches local files
- ❌ Claude Code never reads PROJECT_INSTRUCTIONS.md

---

## Documentation Structure

### Core Documentation (`/docs/`)

📋 **[CASH_OPERATING_INSTRUCTIONS.md](docs/CASH_OPERATING_INSTRUCTIONS.md)**
- Complete operating instructions for Cash
- Fireflies transcript retrieval
- 14-file generation process
- GitHub repo creation workflow

📖 **[COMPLETE_WORKFLOW.md](docs/COMPLETE_WORKFLOW.md)**
- End-to-end workflow from requirements to production
- Parallel Claude coordination
- GitHub-based communication protocol

📝 **[14_FILE_TEMPLATE.md](docs/14_FILE_TEMPLATE.md)**
- Templates for all 14 project files
- Overshared instruction examples
- Complete specifications

🎯 **[PROJECT_CREATION_INSTRUCTIONS.md](docs/PROJECT_CREATION_INSTRUCTIONS.md)**
- How Cash creates GitHub repos first
- File generation standards
- Oversharing methodology

🚀 **[DEPLOYMENT_CHECKLIST.md](docs/DEPLOYMENT_CHECKLIST.md)**
- Continuous deployment procedures
- GitHub → Vercel workflow
- Production testing

### Templates (`/templates/`)

📄 **[PROJECT_INSTRUCTIONS_TEMPLATE.md](templates/PROJECT_INSTRUCTIONS_TEMPLATE.md)**
- Template for Claude Project orchestrator instructions
- GitHub-only monitoring and review
- Never accesses local files

🔧 **[CODE_STARTER_PROMPT_TEMPLATE.md](templates/CODE_STARTER_PROMPT_TEMPLATE.md)**
- Template for Claude Code builder instructions
- Overshared task definitions
- Commit/push protocols

💬 **[COMMUNICATION_TEMPLATES.md](templates/COMMUNICATION_TEMPLATES.md)**
- GitHub-based feedback patterns
- Issue and PR templates

### YouTube Assets (`/youtube/`)

🎬 **[VIDEO_DESCRIPTION.md](youtube/VIDEO_DESCRIPTION.md)**
🖼️ **[THUMBNAIL_PROMPT.md](youtube/THUMBNAIL_PROMPT.md)**

---

## Key Principles

### GitHub-First Architecture
- ✅ Cash creates GitHub repo BEFORE anything else
- ✅ GitHub is the ONLY source of truth
- ✅ Claude Project reads/writes GitHub only
- ✅ Claude Code clones, commits, pushes to GitHub
- ✅ No local folder confusion

### Oversharing Instructions
Every task includes:
- Exact terminal commands
- Full file contents
- Expected outputs
- Testing commands
- Troubleshooting steps

**Bad:**
```
Task 3: Set up Supabase connection
```

**Good (Overshared):**
```
Task 3: Set up Supabase connection

Step 1: Create lib directory
$ mkdir -p code/lib

Step 2: Create Supabase client
$ cat > code/lib/supabase.ts << 'EOF'
import { createClient } from '@supabase/supabase-js'
// ... full file content
EOF

Step 3: Test connection
$ cd code && npm run dev

Expected: Server starts without Supabase errors

Troubleshooting:
- If "Invalid API key": Check SUPABASE_ANON_KEY in .env.local
- If "Connection refused": Verify Supabase project is running

Step 4: Commit
$ git add . && git commit -m "Phase 1 Task 3: Supabase connection" && git push
```

### Parallel Instance Separation
- Claude Project: Orchestrator (GitHub only)
- Claude Code: Builder (local clone only)
- Communication: GitHub commits only
- No direct interaction ever

---

## Pricing Models

### Solopreneur (1-5 users)
- 1 Brain: $1,200-$1,800/month
- 2 Brains: $1,800-$2,600/month
- 3 Brains: $2,400-$3,200/month

### Company (5+ users)
- 1 Brain: $3,000/month + $200/seat
- 2 Brains: $5,000/month + $300/seat
- 3 Brains: $6,500/month + $400/seat

### Investor Pricing
- Base (1 system): $2,000/month
- Mid (2 systems): $2,600/month
- Premium (3 systems): $3,200/month

### Custom Builds
- $5,000/month minimum

---

## AI West Platform Brains

**Outreach Brain** - LinkedIn outbound, email sequences, multi-channel prospecting
**Content Brain** - Content creation, scheduling, multi-platform publishing
**Business Brain** - CRM intelligence, knowledge hub, deal tracking

---

## Success Metrics

**Speed:** 2-3 days from requirements to production (5-7 deploy cycles)
**Quality:** Zero security vulnerabilities, optimized performance
**Testing:** Continuous via GitHub PR reviews
**Documentation:** Complete handoff materials
**Result:** Professional applications ready for immediate use

---

## Questions or Issues?

Contact: josh@aiwest.co
Website: aiwest.co

---

**Last Updated:** January 2026
**Version:** 4.0 (Parallel Claude Architecture)

© 2025 AI West LLC. All rights reserved.