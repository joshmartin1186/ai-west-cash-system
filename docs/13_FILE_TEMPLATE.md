# 13-File Generation Template (v5)

## Overview

Cash generates 13 files for every project:
- **Files 1-11:** Specification files (requirements, architecture, schema, etc.)
- **Files 12-13:** Execution files (build plan, builder instructions)

**Note:** v5 removed PROJECT_INSTRUCTIONS.md (was File 13 in v4) since there's no Claude Project orchestrator anymore.

---

## Quick Checklist

- [ ] File 1: PROJECT_OVERVIEW.md
- [ ] File 2: TECHNICAL_ARCHITECTURE.md
- [ ] File 3: DATABASE_SCHEMA.md
- [ ] File 4: API_INTEGRATIONS.md
- [ ] File 5: UI_SPECIFICATIONS.md
- [ ] File 6: BUILD_PHASES.md
- [ ] File 7: DEBUGGING_GUIDE.md
- [ ] File 8: CLIENT_REQUIREMENTS.md
- [ ] File 9: PRODUCTIZATION_GUIDE.md
- [ ] File 10: DEPLOYMENT_CHECKLIST.md
- [ ] File 11: AI_WEST_DESIGN_SYSTEM.md
- [ ] File 12: EXECUTION_PLAN.md
- [ ] File 13: CODE_STARTER_PROMPT.md

---

## FILE 1: PROJECT_OVERVIEW.md

```markdown
# [Project Name] - Project Overview

## Client Information
- **Client Name:** [Full Name]
- **Company:** [Company Name]
- **Email:** [Contact Email]
- **Phone:** [If available]
- **Team Size:** [Number of users]

## Problem Statement
[2-3 sentences describing their pain]

## Solution Overview
[What we're building - 3-4 sentences]

## AI West Platform Configuration

**Brains Included:**
- [ ] Outreach Brain - [What it will do for them]
- [ ] Content Brain - [What it will do for them]
- [ ] Business Brain - [What it will do for them]

**Custom Features:**
- [Any bespoke features beyond standard Brains]

## Core Features
1. [Feature 1]
2. [Feature 2]
3. [Feature 3]
4. [Feature 4]

## Business Model

### Client Revenue

**Pricing Model:** [Solopreneur / Company / Investor]
**Configuration:** [1 Brain / 2 Brains / 3 Brains / Custom]

**If Solopreneur Model:**
- Monthly Cost: $[X]/month
- Annual Value: $[Y]/year

**If Company Model:**
- Base Cost: $[X]/month
- Users Included: [N] users
- Additional Users: [N] users × $[Y]/seat = $[Z]
- Total Monthly: $[Total]/month
- Annual Value: $[Total × 12]/year

### Productization Potential
- **Target Market:** [Who else needs this]
- **Market Size:** [Estimated similar customers]
- **Potential MRR:** $[Z]/month from [N] customers

## Success Criteria
- [ ] [Criterion 1 with metric]
- [ ] [Criterion 2 with metric]
- [ ] [Criterion 3 with metric]

## Timeline
- **Phase 1 (Foundation):** 2-3 hours
- **Phase 2 (Core UI):** 3-4 hours
- **Phase 3 (Automation):** 4-6 hours
- **Phase 4 (Deploy & Handoff):** 1-2 hours
- **Total Timeline:** 2-3 days to production

## Key Stakeholders
- **Primary User:** [Role/persona]
- **Secondary Users:** [Other roles]
- **Decision Maker:** [Who signs off]
```

---

## FILE 2: TECHNICAL_ARCHITECTURE.md

```markdown
# Technical Architecture

## Tech Stack

### Frontend
- **Framework:** Next.js 14 (App Router)
- **Language:** TypeScript
- **Styling:** Tailwind CSS + shadcn/ui
- **State Management:** React Context + Hooks

### Backend
- **Database:** Supabase (PostgreSQL)
- **API:** Next.js API Routes
- **Authentication:** Supabase Auth
- **File Storage:** Supabase Storage (if needed)

### Infrastructure
- **Hosting:** Vercel
- **Database:** Supabase Cloud
- **CDN:** Vercel Edge Network
- **Monitoring:** Vercel Analytics + System Logs

### Payments
- **Provider:** Stripe
- **Products:** [List Stripe products needed]
- **Webhooks:** checkout.session.completed, subscription.updated, etc.

### AI/ML
- **Provider:** Google Gemini
- **Model:** gemini-1.5-flash
- **Use Cases:** [List AI features]

## Multi-Tenant Architecture

### Data Isolation Pattern
```sql
-- Every table includes organization_id
CREATE TABLE example_table (
  id UUID PRIMARY KEY,
  organization_id UUID REFERENCES organizations(id) NOT NULL,
  -- other fields
);

-- RLS policy ensures isolation
CREATE POLICY "Org isolation" ON example_table
  FOR ALL USING (organization_id = (
    SELECT organization_id FROM users WHERE id = auth.uid()
  ));
```

### Role-Based Access Control
- **Owner:** Full access including billing
- **Admin:** All features except billing
- **Developer:** Debug console + logs access
- **Viewer:** Read-only access

## API Design

### Public Routes (No Auth)
- `POST /api/auth/signup` - Create account
- `POST /api/auth/login` - Authenticate
- `POST /api/webhooks/stripe` - Stripe webhooks

### Protected Routes (Auth Required)
[List all API routes needed]

## Security Considerations
- All queries use organization_id filter
- RLS policies on every table
- API routes verify authentication
- Rate limiting on public endpoints
- Input validation on all forms
```

---

## FILE 3: DATABASE_SCHEMA.md

[Include complete SQL schema with all tables needed for enabled Brains]

---

## FILE 4: API_INTEGRATIONS.md

[Include all external API integrations with setup steps, auth, rate limits]

---

## FILE 5: UI_SPECIFICATIONS.md

[Include all page layouts, components, user flows]

---

## FILE 6: BUILD_PHASES.md

[Include all phases with atomic tasks]

---

## FILE 7: DEBUGGING_GUIDE.md

[Include non-technical troubleshooting guide]

---

## FILE 8: CLIENT_REQUIREMENTS.md

[Include client-specific configuration, ICP, business rules]

---

## FILE 9: PRODUCTIZATION_GUIDE.md

[Include SaaS opportunity, pricing, launch strategy]

---

## FILE 10: DEPLOYMENT_CHECKLIST.md

[Include step-by-step deployment and handoff procedures]

---

## FILE 11: AI_WEST_DESIGN_SYSTEM.md

**[COPY MASTER FILE EXACTLY FROM ai-west-cash-system REPO - NEVER MODIFY]**

---

## FILE 12: EXECUTION_PLAN.md

```markdown
# Execution Plan

## Repository Information

**GitHub Repo:** https://github.com/joshmartin1186/[project-name]
**Local Clone:** ~/projects/[project-name]/

## Phase 1: Foundation

### Task 1: Initialize Next.js Project

**Commands:**
```bash
cd ~/projects/[project-name]
mkdir -p code
cd code
npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir=false --import-alias="@/*"
```

**Expected Output:**
```
Success! Created [project-name] at ~/projects/[project-name]/code
```

**Verification:**
```bash
ls -la
# Should see: app/, public/, package.json, etc.
```

**Commit:**
```bash
cd ~/projects/[project-name]
git add .
git commit -m "Phase 1 Task 1: Initialize Next.js project"
git push
```

---

### Task 2: Install Dependencies

[Continue with overshared commands for every task...]

---

## Commit Protocol

**Every 2-3 tasks:**
```bash
git add .
git commit -m "Phase X Task Y-Z: [Description]"
git push
```

## Testing Protocol

**After each task:**
1. Run `npm run dev`
2. Check for errors in terminal
3. Check for errors in browser console
4. Verify feature works as expected
```

---

## FILE 13: CODE_STARTER_PROMPT.md

```markdown
# [Project Name] - Claude Code Build Instructions

## Your Role

You are Claude Code, the sole builder for this project. You will:
1. Build the complete application
2. Commit every 2-3 tasks
3. Push immediately after each commit
4. Deploy to Vercel when ready
5. Complete client handoff

**There is no orchestrator. You handle everything.**

## Project Location

**Git Remote:** https://github.com/joshmartin1186/[project-name]
**Local Folder:** ~/projects/[project-name]/

## First Steps

1. Navigate to the project:
```bash
cd ~/projects/[project-name]
```

2. Verify you're in the right place:
```bash
ls -la
# Should see: README.md, PROJECT_OVERVIEW.md, EXECUTION_PLAN.md, etc.
```

3. Read the execution plan:
```bash
cat EXECUTION_PLAN.md
```

4. Begin with Task 1!

## Build Protocol

- Follow EXECUTION_PLAN.md exactly
- Commit every 2-3 tasks
- Push immediately after every commit
- Test before committing (never push broken code)
- When blocked, tell Josh and create GitHub issue

## Commit Messages

```
Phase 1 Task 1: Initialize Next.js project
Phase 1 Task 2-3: Dependencies and Supabase connection
Phase 2 Task 9-10: Dashboard and metrics
```

## When You Hit a Blocker

1. Tell Josh what's wrong
2. Create a GitHub issue with details
3. Commit what you have (even if incomplete)
4. Push to GitHub
5. Wait for guidance

## Deployment (Phase 4)

When all features are built:
1. Follow DEPLOYMENT_CHECKLIST.md
2. Deploy to Vercel
3. Configure production environment
4. Set up Stripe production webhook
5. Pre-seed client as Owner
6. Pre-seed Josh as Developer
7. Send handoff email
8. Schedule walkthrough call

## Reference Files

All in ~/projects/[project-name]/:
- EXECUTION_PLAN.md - Your task-by-task guide
- TECHNICAL_ARCHITECTURE.md - Tech decisions
- DATABASE_SCHEMA.md - All SQL
- UI_SPECIFICATIONS.md - Page layouts
- AI_WEST_DESIGN_SYSTEM.md - Styling rules
- DEPLOYMENT_CHECKLIST.md - How to deploy

---

**START NOW: Read EXECUTION_PLAN.md and begin Task 1!**
```

---

## Remember When Generating These Files

**Every file must be:**
- **Comprehensive** - No gaps, no assumptions
- **Actionable** - Clear next steps
- **Specific** - Exact commands, exact code
- **Complete** - Full SQL, full file contents

**EXECUTION_PLAN.md is critical:**
- Every command needed
- Every file to create
- Every expected output
- Every troubleshooting step

**CODE_STARTER_PROMPT.md gets Claude Code going:**
- Project location
- First steps
- Build protocol
- How to handle blockers

**The 13 files ARE the blueprint. Make them perfect.**