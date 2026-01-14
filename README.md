# AI West - Cash v5 System

**Streamlined Two-Claude Development System for Production Applications**

Cash v5 is AI West's proprietary system for building production applications in 2-3 days. This version eliminates the Claude Project middleman for a direct Cash → Claude Code workflow.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      CASH (Discovery)                        │
│                                                              │
│   Analyzes requirements → Creates GitHub repo + Local folder │
│   Generates Claude Code starter prompt                       │
│                                                              │
│                           ↓                                  │
│   ┌─────────────────────────────────────────────────────┐   │
│   │                    OUTPUTS                           │   │
│   │  1. GitHub Repo (14 files)                          │   │
│   │  2. Local Project Folder ~/projects/[name]/         │   │
│   │  3. Claude Code Starter Prompt                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                           ↓                                  │
│              ┌───────────────────────┐                       │
│              │     Claude Code       │                       │
│              │      (Builder)        │                       │
│              │                       │                       │
│              │ - Uses local clone    │                       │
│              │ - Builds code         │                       │
│              │ - Commits every 2-3   │                       │
│              │ - Pushes immediately  │                       │
│              │ - Deploys to Vercel   │                       │
│              └───────────────────────┘                       │
│                           ↓                                  │
│                    PRODUCTION APP                            │
└─────────────────────────────────────────────────────────────┘
```

**Two Coordinated Agents:**
1. **Cash (Discovery)** - Analyzes requirements, creates GitHub repo + local folder, generates starter prompt
2. **Claude Code (Builder)** - Builds, commits, deploys - the only builder

**Timeline:** 2-3 days from requirements to production (5-7 deploy cycles)

---

## The 13 Files

Cash generates 13 files for every project:

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

### Execution Files (12-13)

| File | Purpose |
|------|---------|
| EXECUTION_PLAN.md | Task-by-task build sequence with exact commands |
| CODE_STARTER_PROMPT.md | Paste into Claude Code to begin building |

---

## Quick Start

### Phase 1: Discovery (Cash)

1. **Trigger:** "Pull my conversation with [client] from Fireflies and create a project"
2. **Cash:** Retrieves transcript, extracts requirements
3. **Cash:** Generates all 13 files
4. **Cash:** Creates GitHub repository and pushes files
5. **Cash:** Creates local project folder using Desktop Commander
6. **Cash:** Generates Claude Code starter prompt

### Phase 2: Building (Josh + Claude Code)

1. **Josh:** Opens Claude Code (Claude Desktop with MCP tools)
2. **Josh:** Pastes the CODE_STARTER_PROMPT.md content
3. **Claude Code:** Clones repo, builds, commits, pushes, deploys
4. **Done:** Production app live

---

## Documentation Structure

### Core Documentation (`/docs/`)

📋 **[CASH_OPERATING_INSTRUCTIONS.md](docs/CASH_OPERATING_INSTRUCTIONS.md)**
- Complete operating instructions for Cash
- Fireflies transcript retrieval
- 13-file generation process
- GitHub repo + local folder creation

📖 **[COMPLETE_WORKFLOW.md](docs/COMPLETE_WORKFLOW.md)**
- End-to-end workflow from requirements to production
- Streamlined two-Claude coordination

📝 **[13_FILE_TEMPLATE.md](docs/13_FILE_TEMPLATE.md)**
- Templates for all 13 project files
- Overshared instruction examples

🚀 **[DEPLOYMENT_CHECKLIST.md](docs/DEPLOYMENT_CHECKLIST.md)**
- Continuous deployment procedures
- GitHub → Vercel workflow

### Templates (`/templates/`)

🔧 **[CODE_STARTER_PROMPT_TEMPLATE.md](templates/CODE_STARTER_PROMPT_TEMPLATE.md)**
- Template for Claude Code builder instructions
- Overshared task definitions
- Commit/push/deploy protocols

💬 **[COMMUNICATION_TEMPLATES.md](templates/COMMUNICATION_TEMPLATES.md)**
- Client communication templates
- Handoff emails

### YouTube Assets (`/youtube/`)

🎬 **[VIDEO_DESCRIPTION.md](youtube/VIDEO_DESCRIPTION.md)**
🖼️ **[THUMBNAIL_PROMPT.md](youtube/THUMBNAIL_PROMPT.md)**

---

## Key Principles

### GitHub-First Architecture
- ✅ Cash creates GitHub repo with all files
- ✅ Cash creates local folder as exact clone
- ✅ GitHub is the source of truth
- ✅ Claude Code works from local clone
- ✅ Continuous commits keep everything in sync

### Oversharing Instructions
Every task in EXECUTION_PLAN.md includes:
- Exact terminal commands
- Full file contents
- Expected outputs
- Testing commands
- Troubleshooting steps

### No Middleman
- ❌ No Claude Project orchestrator
- ✅ Cash hands off directly to Claude Code
- ✅ One Claude builds everything
- ✅ Simpler = faster = better

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
**Simplicity:** Two Claudes, not three. One builder, not two.
**Result:** Professional applications ready for immediate use

---

## Questions or Issues?

Contact: josh@aiwest.co
Website: aiwest.co

---

**Last Updated:** January 2026
**Version:** 5.0 (Streamlined Two-Claude Architecture)

© 2025 AI West LLC. All rights reserved.