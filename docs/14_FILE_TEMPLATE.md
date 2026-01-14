# 14-File Generation Template

## Overview

Cash generates 14 files for every project:
- **Files 1-11:** Specification files (requirements, architecture, schema, etc.)
- **Files 12-14:** Execution files (build plan, orchestrator instructions, builder instructions)

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
- [ ] File 13: PROJECT_INSTRUCTIONS.md (for Claude Project)
- [ ] File 14: CODE_STARTER_PROMPT.md (for Claude Code)

---

## FILE 1: PROJECT_OVERVIEW.md

```markdown
# [Project Name] - Project Overview

## Client Information
- **Client Name:** [Full Name]
- **Company:** [Company Name]
- **Email:** [Contact Email]
- **Phone:** [If available]

## Problem Statement
[2-3 sentences extracted from Fireflies transcript describing their pain]

## Solution Overview
[What we're building - 3-4 sentences]

## Core Features
1. [Feature 1 from transcript]
2. [Feature 2 from transcript]
3. [Feature 3 from transcript]
4. [Feature 4 from transcript]

## Business Model

### Client Revenue
- **Demographic:** [Investor / Solopreneur / Startup]
- **Pricing Tier:** [Base/Mid/Premium]
- **Monthly Cost:** $[X]/month
- **Annual Value:** $[Y]/year
- **Brains Included:** [List brains]

### Productization Potential
- **Target Market:** [Who else needs this]
- **Market Size:** [Estimated number of similar customers]
- **Potential MRR:** $[Z]/month from [N] customers
- **SaaS Pricing:** $[X]/month per customer

### AI West Revenue
- **Client Subscription:** $[X]/month
- **Productized SaaS:** $[Y]/month potential
- **Total Opportunity:** $[Z]/month

## Success Criteria
[Specific measurable outcomes from transcript]
- [ ] [Criterion 1 with metric]
- [ ] [Criterion 2 with metric]
- [ ] [Criterion 3 with metric]

## Timeline
- **Total Timeline:** 2-3 days to production (5-7 deploy cycles)
- **Phase 1 (Foundation):** 2-3 hours
- **Phase 2 (Core UI):** 3-4 hours
- **Phase 3 (Automation):** 4-6 hours
- **Phase 4 (Deploy & Handoff):** 1-2 hours

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
- `GET /api/users` - List organization users
- `POST /api/users/invite` - Invite user
- [List all API routes needed]

## Security Considerations
- All queries use organization_id filter
- RLS policies on every table
- API routes verify authentication
- Rate limiting on public endpoints
- Input validation on all forms
```

---

## FILE 3: DATABASE_SCHEMA.md

```markdown
# Database Schema

## Core Tables (Required in Every Project)

### organizations
```sql
CREATE TABLE organizations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  slug TEXT UNIQUE,
  
  -- Stripe integration
  stripe_customer_id TEXT,
  stripe_subscription_id TEXT,
  subscription_status TEXT DEFAULT 'trialing' 
    CHECK (subscription_status IN ('trialing', 'active', 'past_due', 'canceled', 'unpaid')),
  subscription_plan TEXT DEFAULT 'starter',
  trial_ends_at TIMESTAMPTZ DEFAULT NOW() + INTERVAL '14 days',
  
  settings JSONB DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_organizations_stripe_customer ON organizations(stripe_customer_id);
```

### users
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY REFERENCES auth.users(id),
  organization_id UUID REFERENCES organizations(id) NOT NULL,
  email TEXT NOT NULL,
  name TEXT,
  avatar_url TEXT,
  role TEXT DEFAULT 'viewer' 
    CHECK (role IN ('owner', 'admin', 'developer', 'viewer')),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_users_organization ON users(organization_id);
CREATE INDEX idx_users_email ON users(email);

ALTER TABLE users ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own organization" 
ON users FOR SELECT 
USING (organization_id = (
  SELECT organization_id FROM users WHERE id = auth.uid()
));
```

### system_logs
```sql
CREATE TABLE system_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  organization_id UUID REFERENCES organizations(id),
  user_id UUID REFERENCES users(id),
  action_type TEXT NOT NULL,
  status TEXT NOT NULL CHECK (status IN ('started', 'success', 'warning', 'error')),
  severity TEXT DEFAULT 'info' CHECK (severity IN ('debug', 'info', 'warning', 'error', 'critical')),
  message TEXT NOT NULL,
  details JSONB,
  error_message TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_system_logs_organization ON system_logs(organization_id);
CREATE INDEX idx_system_logs_created_at ON system_logs(created_at DESC);
```

## Domain-Specific Tables

[Include all tables specific to this project with complete SQL]

## Sample Data

```json
{
  "organization": {
    "id": "uuid",
    "name": "Acme Fund",
    "slug": "acme-fund"
  },
  "user": {
    "id": "uuid",
    "organization_id": "uuid",
    "email": "john@acme.com",
    "role": "owner"
  }
}
```
```

---

## FILE 4: API_INTEGRATIONS.md

```markdown
# External API Integrations

## Stripe (Payment Processing)

### Setup Steps
1. Create Stripe account (if not exists)
2. Set up products in Stripe Dashboard
3. Configure webhook endpoint
4. Add API keys to environment variables

### Products Configuration
[List all Stripe products needed]

### Webhook Events
Configure webhook at: `https://[app-url]/api/webhooks/stripe`

**Required Events:**
- `checkout.session.completed`
- `customer.subscription.updated`
- `customer.subscription.deleted`
- `invoice.payment_succeeded`
- `invoice.payment_failed`

### API Keys
```bash
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
```

---

## [Integration Name]

### Purpose
[What this integration does]

### Setup Steps
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Authentication
**Type:** [API Key / OAuth / etc.]
```bash
[INTEGRATION]_API_KEY=xxx
```

### Rate Limits
- [X] requests per [timeframe]

### Error Handling
- **Rate Limit:** Wait and retry with exponential backoff
- **Auth Error:** Check API key
- **Server Error:** Log error, retry up to 3 times

---

## Environment Variables Summary

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# Stripe
STRIPE_SECRET_KEY=
STRIPE_PUBLISHABLE_KEY=
STRIPE_WEBHOOK_SECRET=

# AI
GEMINI_API_KEY=

# App Config
NEXT_PUBLIC_APP_URL=
```
```

---

## FILE 5: UI_SPECIFICATIONS.md

```markdown
# UI Specifications

## Design System
**Follow:** AI_WEST_DESIGN_SYSTEM.md

**Key Colors:**
- Primary: #DC6B4A (Terracotta)
- Background: #F5F3EF (Off-white)
- Text: #1a1a1a (Near-black)

**Typography:**
- Font: IBM Plex Sans
- Headings: 600 weight
- Body: 400 weight

## Component Library
**Using:** shadcn/ui components

## Page Structure

### Public Pages

#### Landing Page (`/`)
- Hero with value proposition
- Key features (3-4)
- Pricing
- CTA to signup

#### Login (`/login`)
- Email input
- Password input
- Links to signup/forgot password

#### Signup (`/signup`)
1. Email + password form
2. Redirect to Stripe Checkout
3. Webhook creates organization
4. Redirect to dashboard

### Protected Pages

#### Dashboard (`/app/dashboard`)
[Describe layout with ASCII diagram]

#### [Feature Pages]
[List each feature page with layout]

#### Settings (`/app/settings`)
- Profile
- Organization
- Team (user management)
- Billing

## User Flows

### New User Onboarding
```
1. Land on signup page
2. Enter email + password
3. Redirect to Stripe Checkout
4. Enter payment (starts trial)
5. Webhook creates org
6. Redirect to dashboard
```

## Responsive Design
- **Desktop:** > 1024px (primary target)
- **Tablet:** 768px - 1024px
- **Mobile:** < 768px (functional but simplified)
```

---

## FILE 6: BUILD_PHASES.md

```markdown
# Build Phases

## Phase 1: Foundation
**Duration:** 2-3 hours
**Goal:** Working Next.js app with auth and database

### Tasks
1. [ ] Initialize Next.js project with TypeScript
2. [ ] Install dependencies
3. [ ] Set up Supabase connection
4. [ ] Create complete database schema
5. [ ] Implement authentication flow
6. [ ] Create base layout with AI West styling
7. [ ] Configure environment variables
8. [ ] Test: App runs at localhost:3000

### Success Criteria
- `npm run dev` runs without errors
- Can access localhost:3000
- Database tables created
- Can create account and log in

---

## Phase 2: Core UI
**Duration:** 3-4 hours
**Goal:** All user-facing pages built and connected

### Tasks
9. [ ] Build dashboard with metrics
10. [ ] Create [feature] pages
11. [ ] Implement settings pages
12. [ ] Build user management interface
13. [ ] Create invite system
14. [ ] Add loading and error states
15. [ ] Test all pages

### Success Criteria
- All pages render without errors
- Data displays from Supabase
- Settings save and persist

---

## Phase 3: Automation
**Duration:** 4-6 hours
**Goal:** External integrations working

### Tasks
16. [ ] Integrate [External API 1]
17. [ ] Integrate [External API 2]
18. [ ] Build [core feature] functionality
19. [ ] Set up webhook handlers
20. [ ] Create system logging
21. [ ] Test all integrations

### Success Criteria
- All external APIs connect
- Webhooks process events
- System logs capture actions

---

## Phase 4: Deploy & Handoff
**Duration:** 1-2 hours
**Goal:** Production deployment and client handoff

### Tasks
22. [ ] Final push to GitHub
23. [ ] Deploy to Vercel
24. [ ] Configure production env vars
25. [ ] Set up production Stripe webhook
26. [ ] Test production app
27. [ ] Pre-seed client as Owner
28. [ ] Pre-seed Josh as Developer
29. [ ] Send handoff email
30. [ ] Schedule walkthrough call

### Success Criteria
- Production app accessible
- All features work in production
- Client can log in
```

---

## FILE 7: DEBUGGING_GUIDE.md

```markdown
# Debugging Guide (Non-Technical)

## How to Check System Logs

1. Log in to the app
2. Go to Settings → Debug Console (Developer role required)
3. View recent logs
4. Filter by date, severity, or action type

## Common Issues

### "I can't log in"
**Check:** Is your email correct?
**Check:** Did you complete signup (including payment)?
**Fix:** Click "Forgot Password" to reset

### "Data isn't showing"
**Check:** Are you logged in?
**Check:** Do you have the right permissions?
**Fix:** Contact admin to verify your role

### "Feature isn't working"
**Check:** Look at system logs for errors
**Check:** Is the external service connected?
**Fix:** Contact Josh for technical support

## When to Contact Support

- Multiple users experiencing same issue
- Error messages in system logs
- Features stopped working that worked before

## Contact
josh@aiwest.co
```

---

## FILE 8: CLIENT_REQUIREMENTS.md

```markdown
# Client Requirements

## ICP Definition
[Who is this client? What do they do?]

## Custom Branding
- Logo: [Provided / Use AI West default]
- Colors: [Custom colors or AI West defaults]
- Domain: [Custom domain or subdomain]

## Business Rules
[Specific logic requirements from transcript]

## Pre-Built Templates
[Any templates they need pre-loaded]

## Integration Requirements
[Specific tools they use that need integration]

## User Count
- Expected users: [Number]
- Pricing tier: [Solopreneur / Company]
- Seats included: [Number]
```

---

## FILE 9: PRODUCTIZATION_GUIDE.md

```markdown
# Productization Guide

## Market Opportunity

### Target Market
[Who else needs this exact solution?]

### Market Size
[How many potential customers?]

### Competition
[What alternatives exist?]

## SaaS Pricing

### Recommended Tiers
- **Starter:** $X/month - [features]
- **Pro:** $Y/month - [features]
- **Enterprise:** $Z/month - [features]

## Landing Page Structure

### Hero Section
- Headline: [Value proposition]
- Subheadline: [Supporting text]
- CTA: "Start Free Trial"

### Features Section
- Feature 1: [Title + description]
- Feature 2: [Title + description]
- Feature 3: [Title + description]

### Pricing Section
[Display tiers]

### Social Proof
[Testimonials, logos, metrics]

## Launch Channels

### Free Channels
- [ ] Product Hunt
- [ ] BetaList
- [ ] Reddit (r/SaaS, r/microsaas)
- [ ] Indie Hackers
- [ ] Hacker News (Show HN)

### Paid Channels (Optional)
- Meta Ads ($20/day recommended start)
- LinkedIn Ads (if B2B)
```

---

## FILE 10: DEPLOYMENT_CHECKLIST.md

```markdown
# Deployment Checklist

## Pre-Deployment Verification

- [ ] All code pushed to GitHub
- [ ] No TypeScript errors
- [ ] No console errors in browser
- [ ] All features tested locally
- [ ] Environment variables documented

## Vercel Deployment

### Step 1: Connect Repository
1. Go to vercel.com
2. Import Git Repository
3. Select: github.com/joshmartin1186/[project-name]
4. Framework: Next.js (auto-detected)

### Step 2: Configure Environment Variables

```
NEXT_PUBLIC_SUPABASE_URL = [from Supabase dashboard]
NEXT_PUBLIC_SUPABASE_ANON_KEY = [from Supabase dashboard]
SUPABASE_SERVICE_ROLE_KEY = [from Supabase dashboard]
STRIPE_SECRET_KEY = sk_live_xxx
STRIPE_PUBLISHABLE_KEY = pk_live_xxx
STRIPE_WEBHOOK_SECRET = [after webhook setup]
GEMINI_API_KEY = [from Google AI Studio]
NEXT_PUBLIC_APP_URL = https://[project-name].vercel.app
```

### Step 3: Deploy
1. Click "Deploy"
2. Wait for build to complete
3. Note the production URL

### Step 4: Update OAuth Redirects
1. Go to Supabase → Authentication → URL Configuration
2. Add production URL to Site URL
3. Add to Redirect URLs:
   - `https://[project-name].vercel.app/auth/callback`

### Step 5: Configure Stripe Webhook
1. Go to Stripe Dashboard → Webhooks
2. Add endpoint: `https://[project-name].vercel.app/api/webhooks/stripe`
3. Select events: checkout.session.completed, etc.
4. Copy webhook secret to Vercel env vars
5. Redeploy

## Post-Deployment Testing

- [ ] Homepage loads
- [ ] Can create account
- [ ] Stripe checkout works
- [ ] Dashboard loads after login
- [ ] All features functional
- [ ] No errors in Vercel logs

## Client Handoff

### Pre-seed Accounts
1. Create client as Owner
2. Create Josh as Developer
3. Send magic links

### Handoff Email
[Template in COMMUNICATION_TEMPLATES.md]

### Walkthrough Call
Schedule via: calendar.app.google/oJ584xcgU3rD4akH6
```

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

## Before Starting

```bash
# Clone the repository
cd ~/projects
git clone https://github.com/joshmartin1186/[project-name].git
cd [project-name]

# Verify files
ls -la

# Create code directory
mkdir -p code
```

## Phase 1: Foundation

### Task 1: Initialize Next.js Project

**Commands:**
```bash
cd ~/projects/[project-name]/code
npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir=false --import-alias="@/*"
```

**Expected Output:**
```
✔ Would you like to use TypeScript? Yes
✔ Would you like to use ESLint? Yes
...
Success! Created [project-name] at ~/projects/[project-name]/code
```

**Verification:**
```bash
ls -la
# Should see: app/, public/, package.json, etc.
```

**Commit:**
```bash
git add .
git commit -m "Phase 1 Task 1: Initialize Next.js project"
git push
```

---

### Task 2: Install Dependencies

**Commands:**
```bash
cd ~/projects/[project-name]/code
npm install @supabase/supabase-js @supabase/ssr stripe @stripe/stripe-js
npx shadcn-ui@latest init
# Select: Default style, Slate base color, CSS variables: Yes
npx shadcn-ui@latest add button card input label select textarea dialog dropdown-menu table tabs toast
```

**Expected Output:**
```
added X packages in Ys
✔ Writing components.json...
✔ Installing dependencies...
```

**Verification:**
```bash
cat package.json | grep supabase
# Should see: "@supabase/supabase-js": "x.x.x"
```

**Commit:**
```bash
git add .
git commit -m "Phase 1 Task 2: Install dependencies"
git push
```

---

### Task 3: Set up Supabase Connection

**Step 1: Create lib directory**
```bash
mkdir -p lib
```

**Step 2: Create Supabase client**
```bash
cat > lib/supabase.ts << 'EOF'
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
EOF
```

**Step 3: Create server client**
```bash
cat > lib/supabase-server.ts << 'EOF'
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'

export async function createServerSupabaseClient() {
  const cookieStore = await cookies()
  
  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll()
        },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value, options }) => {
            cookieStore.set(name, value, options)
          })
        },
      },
    }
  )
}
EOF
```

**Step 4: Create .env.local**
```bash
cat > .env.local << 'EOF'
NEXT_PUBLIC_SUPABASE_URL=[GET FROM JOSH]
NEXT_PUBLIC_SUPABASE_ANON_KEY=[GET FROM JOSH]
SUPABASE_SERVICE_ROLE_KEY=[GET FROM JOSH]
EOF
```

**Verification:**
```bash
ls lib/
# Should see: supabase.ts, supabase-server.ts
```

**Commit:**
```bash
git add .
git commit -m "Phase 1 Task 3: Supabase connection configured"
git push
```

**Troubleshooting:**
- If import errors: Run `npm install @supabase/ssr`
- If type errors: Check tsconfig.json includes lib folder

---

[Continue with Tasks 4-30 following same overshared format]

---

## Commit Protocol

**Every 2-3 tasks:**
```bash
git add .
git commit -m "Phase X Task Y-Z: [Description]"
git push
```

**Never:**
- Go more than 3 tasks without committing
- Push broken code
- Commit without testing

## Testing Protocol

**After each task:**
1. Run `npm run dev`
2. Check for errors in terminal
3. Check for errors in browser console
4. Verify feature works as expected

**Before each commit:**
1. All tests pass
2. No TypeScript errors
3. No console errors
```

---

## FILE 13: PROJECT_INSTRUCTIONS.md (For Claude Project - Orchestrator)

```markdown
# [Project Name] - Claude Project Instructions

## Your Role

You are the **ORCHESTRATOR**. You:
- Monitor GitHub commits
- Review code quality
- Write feedback to GitHub
- Handle deployment to Vercel
- **NEVER** access local files
- **NEVER** run terminal commands

## Project Repository

**GitHub:** https://github.com/joshmartin1186/[project-name]

## How to Monitor Progress

```javascript
// Check latest commits
github:list_commits({
  owner: "joshmartin1186",
  repo: "[project-name]",
  perPage: 10
})

// Read specific file
github:get_file_contents({
  owner: "joshmartin1186",
  repo: "[project-name]",
  path: "code/app/page.tsx"
})
```

## Your Commands

When Josh says:

### "Show progress"
1. Check recent commits
2. Read key files
3. Report: tasks completed, current phase, any issues

### "Review [phase/feature]"
1. Read all relevant code files from GitHub
2. Check against specs in PROJECT_OVERVIEW.md, TECHNICAL_ARCHITECTURE.md
3. Create GitHub Issue with feedback:
   - CRITICAL issues (security, data integrity)
   - HIGH issues (functionality)
   - MEDIUM issues (code quality)
   - LOW issues (nice-to-haves)

### "Add feature X"
1. Update relevant spec file on GitHub
2. Create GitHub Issue describing the feature
3. Tell Josh: "Feature spec added. Claude Code should pull and continue."

### "Deploy to Vercel"
1. Review code is ready (check recent commits)
2. Walk Josh through Vercel connection
3. Guide environment variable setup
4. Test production deployment
5. Complete client handoff

## Quality Checklist

Before approving any phase:

**Security:**
- [ ] Multi-tenant isolation (organization_id on all tables)
- [ ] RLS policies active
- [ ] Input validation present
- [ ] No exposed secrets

**Functionality:**
- [ ] All features work as specified
- [ ] Error handling in place
- [ ] Loading states implemented

**Code Quality:**
- [ ] Follows AI West Design System
- [ ] TypeScript strict mode
- [ ] No N+1 queries

## Communication Protocol

**You write to GitHub:**
- Issues for feedback
- PR comments for code review
- Updated spec files for new requirements

**You read from GitHub:**
- Commits to see progress
- Code files to review
- Issues for any Claude Code questions

**You NEVER:**
- Access local files
- Run terminal commands
- Talk directly to Claude Code
- Push code changes

## Reference Files (All on GitHub)

- PROJECT_OVERVIEW.md - Client info, requirements
- TECHNICAL_ARCHITECTURE.md - Tech stack, API design
- DATABASE_SCHEMA.md - All SQL
- BUILD_PHASES.md - Task breakdown
- DEPLOYMENT_CHECKLIST.md - Production steps
- AI_WEST_DESIGN_SYSTEM.md - Styling rules

---

**You are the orchestrator. Monitor, review, deploy. GitHub is your only interface.**
```

---

## FILE 14: CODE_STARTER_PROMPT.md (For Claude Code - Builder)

```markdown
# [Project Name] - Claude Code Instructions

## Your Role

You are the **BUILDER**. You:
- Clone the repository locally
- Build the application
- Commit every 2-3 tasks
- Push immediately after each commit
- **NEVER** communicate with Claude Project directly

## Repository

**GitHub:** https://github.com/joshmartin1186/[project-name]
**Local:** ~/projects/[project-name]/

## FIRST: Clone the Repository

```bash
cd ~/projects
git clone https://github.com/joshmartin1186/[project-name].git
cd [project-name]
```

## Build Instructions

Follow EXECUTION_PLAN.md for exact commands.

### Quick Reference - Phase 1: Foundation

**Task 1: Initialize Next.js**
```bash
cd ~/projects/[project-name]/code
npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir=false --import-alias="@/*"
git add . && git commit -m "Phase 1 Task 1: Initialize Next.js" && git push
```

**Task 2: Install Dependencies**
```bash
npm install @supabase/supabase-js @supabase/ssr stripe @stripe/stripe-js
npx shadcn-ui@latest init
npx shadcn-ui@latest add button card input label
git add . && git commit -m "Phase 1 Task 2: Dependencies installed" && git push
```

**Task 3: Supabase Connection**
[See EXECUTION_PLAN.md for full details]

[Continue with all tasks...]

## Commit Protocol

**CRITICAL: Commit every 2-3 tasks!**

```bash
git add .
git commit -m "Phase X Task Y: [Description]"
git push
```

**Why this matters:**
- Claude Project monitors GitHub to see your progress
- Small commits = easier to review and fix
- Continuous pushes = no lost work

## Testing Protocol

After each task:
1. `npm run dev` - check for build errors
2. Open localhost:3000 - check for runtime errors
3. Test the feature you just built
4. Fix any errors before committing

## When You Hit a Blocker

1. Create a GitHub Issue describing the problem
2. Include: what you tried, error messages, relevant code
3. Commit what you have (even if incomplete)
4. Push to GitHub
5. Tell Josh: "Hit a blocker, created Issue #X"

## What You Should NOT Do

- ❌ Read PROJECT_INSTRUCTIONS.md (that's for Claude Project)
- ❌ Talk to Claude Project directly
- ❌ Go more than 3 tasks without committing
- ❌ Push broken code (test first!)
- ❌ Skip testing steps

## Reference Files

In your local clone, read:
- EXECUTION_PLAN.md - Task-by-task commands
- TECHNICAL_ARCHITECTURE.md - Tech decisions
- DATABASE_SCHEMA.md - All SQL
- UI_SPECIFICATIONS.md - Page layouts
- AI_WEST_DESIGN_SYSTEM.md - Styling rules

---

## START BUILDING NOW

1. Clone the repo (if not done)
2. Read EXECUTION_PLAN.md
3. Execute Task 1
4. Commit and push
5. Continue to Task 2
6. Repeat until complete

**You are the builder. Clone, build, commit, push. Repeat.**
```

---

## Remember When Generating These Files

**Every file must be:**
- **Comprehensive** - No gaps, no assumptions
- **Actionable** - Clear next steps
- **Specific** - Exact commands, exact code
- **Complete** - Full SQL, full file contents

**Files 13 & 14 are critical:**
- PROJECT_INSTRUCTIONS.md tells Claude Project HOW to orchestrate
- CODE_STARTER_PROMPT.md tells Claude Code HOW to build
- These files enable the parallel Claude workflow

**Overshare EVERYTHING in EXECUTION_PLAN.md and CODE_STARTER_PROMPT.md:**
- Every command needed
- Every file to create
- Every expected output
- Every troubleshooting step

**The 14 files ARE the blueprint. Make them perfect.**
```