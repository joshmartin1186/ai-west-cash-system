# AI West - Cash System v6

**Streamlined Two-Claude Development System with Supabase-First Architecture**

Cash v6 is AI West's proprietary system for building production applications in 2-3 days. This version adds Supabase-first workflow where the database is created and schema applied before any code work begins.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      CASH WORKFLOW (v6)                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Step 1: RETRIEVE REQUIREMENTS                                   │
│          └─ Fireflies transcript / RFP / Direct input           │
│                           ↓                                      │
│  Step 2: DETERMINE BUSINESS CONTEXT                              │
│          └─ Demographic, pricing, brains, productization        │
│                           ↓                                      │
│  Step 3: CREATE SUPABASE PROJECT  ◄── NEW IN v6                 │
│          └─ supabase:create_project                              │
│          └─ Capture URL, anon key, service role key             │
│                           ↓                                      │
│  Step 4: APPLY DATABASE SCHEMA                                   │
│          └─ supabase:apply_migration                             │
│          └─ Verify tables created                                │
│                           ↓                                      │
│  Step 5: GENERATE 13 FILES                                       │
│          └─ Embed Supabase credentials in relevant files        │
│                           ↓                                      │
│  Step 6: CREATE GITHUB REPOSITORY                                │
│          └─ github:create_repository                             │
│                           ↓                                      │
│  Step 7: PUSH ALL FILES TO GITHUB                                │
│          └─ github:push_files (13 files)                        │
│                           ↓                                      │
│  Step 8: CREATE LOCAL PROJECT FOLDER                             │
│          └─ Desktop Commander:create_directory                   │
│                           ↓                                      │
│  Step 9: CLONE REPO TO LOCAL                                     │
│          └─ Desktop Commander:start_process (git clone)         │
│                           ↓                                      │
│  Step 10: DELIVER STARTER PROMPT TO JOSH                         │
│           └─ Complete prompt with all credentials               │
│                                                                  │
│  ════════════════════════════════════════════════════════════   │
│                      CASH JOB COMPLETE                           │
│  ════════════════════════════════════════════════════════════   │
│                           ↓                                      │
│  JOSH: Paste starter prompt into Claude Code                     │
│                           ↓                                      │
│  CLAUDE CODE: Build → Commit → Deploy → Handoff                 │
│                           ↓                                      │
│                    PRODUCTION APP LIVE                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Two Coordinated Agents:**
1. **Cash (Discovery)** - Analyzes requirements, creates Supabase + GitHub + local folder, generates starter prompt
2. **Claude Code (Builder)** - Builds, commits, deploys, handles client handoff

**Timeline:** 2-3 days from requirements to production (5-7 deploy cycles)

---

## Key Difference in v6: Supabase-First

Cash now creates the Supabase project and applies the database schema BEFORE creating the GitHub repo. This means:

- ✅ Database is ready when Claude Code starts
- ✅ All credentials are captured and embedded in starter prompt
- ✅ No manual Supabase setup required
- ✅ Schema is version-controlled via migrations

---

## The 13 Files

Cash generates 13 files for every project:

### Specification Files (1-11)

| File | Purpose |
|------|---------||
| PROJECT_OVERVIEW.md | Client info, problem/solution, business model, timeline |
| TECHNICAL_ARCHITECTURE.md | Tech stack, Brain integration, multi-tenant design |
| DATABASE_SCHEMA.md | Complete SQL (also applied to Supabase) |
| API_INTEGRATIONS.md | External services, Stripe setup, OAuth config |
| UI_SPECIFICATIONS.md | Page layouts, component library, user flows |
| BUILD_PHASES.md | Development phases with atomic tasks |
| DEBUGGING_GUIDE.md | Non-technical troubleshooting |
| CLIENT_REQUIREMENTS.md | Client-specific config, ICP, business rules |
| PRODUCTIZATION_GUIDE.md | SaaS opportunity, pricing, launch strategy |
| DEPLOYMENT_CHECKLIST.md | Iterative deployment procedures |
| AI_WEST_DESIGN_SYSTEM.md | Copy master file exactly, never modify |

### Execution Files (12-13)

| File | Purpose |
|------|---------|
| EXECUTION_PLAN.md | Task-by-task build sequence with exact commands |
| CODE_STARTER_PROMPT.md | Paste into Claude Code to begin building |

---

## Quick Start

### Phase 1: Discovery & Setup (Cash)

1. **Trigger:** "Pull my conversation with [client] from Fireflies and create a project"
2. **Cash:** Retrieves transcript, extracts requirements
3. **Cash:** Creates Supabase project and applies schema
4. **Cash:** Generates all 13 files with Supabase credentials
5. **Cash:** Creates GitHub repo and pushes files
6. **Cash:** Creates local folder and clones repo
7. **Cash:** Delivers starter prompt with all credentials

### Phase 2: Building (Josh + Claude Code)

1. **Josh:** Opens Claude Code (Claude Desktop with MCP tools)
2. **Josh:** Pastes the starter prompt Cash provided
3. **Claude Code:** Builds, commits, deploys, handles handoff
4. **Done:** Production app live

---

## Documentation Structure

### Core Documentation (`/docs/`)

📋 **[CASH_OPERATING_INSTRUCTIONS.md](docs/CASH_OPERATING_INSTRUCTIONS.md)**
- Complete operating instructions for Cash v6
- Supabase-first workflow
- 13-file generation process

📖 **[COMPLETE_WORKFLOW.md](docs/COMPLETE_WORKFLOW.md)**
- End-to-end workflow from requirements to production
- Supabase → GitHub → Local → Starter Prompt

📝 **[13_FILE_TEMPLATE.md](docs/13_FILE_TEMPLATE.md)**
- Templates for all 13 project files
- Overshared instruction examples

🚀 **[DEPLOYMENT_CHECKLIST.md](docs/DEPLOYMENT_CHECKLIST.md)**
- Deployment procedures
- Client handoff steps

### Templates (`/templates/`)

🔧 **[CODE_STARTER_PROMPT_TEMPLATE.md](templates/CODE_STARTER_PROMPT_TEMPLATE.md)**
- Template for Claude Code builder instructions
- Includes Supabase credentials section

💬 **[COMMUNICATION_TEMPLATES.md](templates/COMMUNICATION_TEMPLATES.md)**
- Client communication templates
- Handoff emails

### YouTube Assets (`/youtube/`)

🎬 **[VIDEO_DESCRIPTION.md](youtube/VIDEO_DESCRIPTION.md)**
🖼️ **[THUMBNAIL_PROMPT.md](youtube/THUMBNAIL_PROMPT.md)**

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

**Speed:** 2-3 days from requirements to production
**Quality:** Database ready before code starts
**Simplicity:** Two Claudes, Supabase-first, direct handoff
**Result:** Professional applications ready for immediate use

---

## Questions or Issues?

Contact: josh@aiwest.co
Website: aiwest.co

---

**Last Updated:** January 2026
**Version:** 6.0 (Supabase-First Two-Claude Architecture)

© 2025 AI West LLC. All rights reserved.