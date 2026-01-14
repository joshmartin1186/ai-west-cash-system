# CODE_STARTER_PROMPT.md - Template for Claude Code (v5)

> **This file is pasted into Claude Code to start building.**
> **Claude Code is the ONLY builder. There is no orchestrator.**

---

# {Project Name} - Claude Code Build Instructions

## Your Role: SOLE BUILDER

You are Claude Code, the **ONLY** builder for this project. Your job is to:

1. **Build** - Execute all tasks from EXECUTION_PLAN.md
2. **Test** - Verify each task works before committing
3. **Commit** - Commit every 2-3 tasks
4. **Push** - Push immediately after every commit
5. **Deploy** - Deploy to Vercel when features are complete
6. **Handoff** - Pre-seed accounts and send handoff email

**There is no orchestrator. You handle everything from build to deployment.**

---

## Project Location

**GitHub:** https://github.com/joshmartin1186/{project-name}
**Local:** ~/projects/{project-name}/

The repo has already been cloned. You're ready to build.

---

## STEP 1: Verify Setup

```bash
# Navigate to project
cd ~/projects/{project-name}

# Verify files are present
ls -la
```

**Expected Output:**
```
README.md
PROJECT_OVERVIEW.md
TECHNICAL_ARCHITECTURE.md
DATABASE_SCHEMA.md
API_INTEGRATIONS.md
UI_SPECIFICATIONS.md
BUILD_PHASES.md
EXECUTION_PLAN.md
... (all 13 files)
```

---

## STEP 2: Read EXECUTION_PLAN.md

```bash
cat EXECUTION_PLAN.md
```

This file contains **exact commands** for every task. Follow it precisely.

---

## STEP 3: Begin Building

### Phase 1: Foundation (Tasks 1-8)

#### Task 1: Initialize Next.js Project

**Commands:**
```bash
cd ~/projects/{project-name}
mkdir -p code
cd code
npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir=false --import-alias="@/*"
```

**When prompted:**
- TypeScript: Yes
- ESLint: Yes
- Tailwind CSS: Yes
- src/ directory: No
- App Router: Yes
- Import alias: @/*

**Verify:**
```bash
ls -la
# Should see: app/, public/, package.json, tsconfig.json, etc.
```

**Test:**
```bash
npm run dev
# Should see: ▲ Next.js 14.x.x
# Open browser: http://localhost:3000
```

**Commit:**
```bash
cd ~/projects/{project-name}
git add .
git commit -m "Phase 1 Task 1: Initialize Next.js project"
git push
```

---

#### Task 2: Install Dependencies

**Commands:**
```bash
cd ~/projects/{project-name}/code

# Install Supabase
npm install @supabase/supabase-js @supabase/ssr

# Install Stripe
npm install stripe @stripe/stripe-js

# Initialize shadcn/ui
npx shadcn-ui@latest init
```

**When prompted for shadcn:**
- Style: Default
- Base color: Slate
- CSS variables: Yes

**Install components:**
```bash
npx shadcn-ui@latest add button card input label select textarea dialog dropdown-menu table tabs toast
```

**Verify:**
```bash
cat package.json | grep -E "supabase|stripe"
# Should see both packages listed
```

**Commit:**
```bash
cd ~/projects/{project-name}
git add .
git commit -m "Phase 1 Task 2: Install dependencies"
git push
```

---

#### Tasks 3-8: Continue from EXECUTION_PLAN.md

Open `~/projects/{project-name}/EXECUTION_PLAN.md` and continue with:
- Task 3: Set up Supabase connection
- Task 4: Create database schema
- Task 5: Implement authentication
- Task 6: Create base layout
- Task 7: Configure middleware
- Task 8: Test foundation complete

**Remember:** Commit every 2-3 tasks!

---

### Phase 2: Core UI (Tasks 9-15)

Continue from EXECUTION_PLAN.md...

---

### Phase 3: Automation (Tasks 16-21)

Continue from EXECUTION_PLAN.md...

---

### Phase 4: Deploy & Handoff (Tasks 22-30)

When all features are built, follow DEPLOYMENT_CHECKLIST.md:

#### Task 22: Deploy to Vercel

1. Go to vercel.com/new
2. Import GitHub repo: github.com/joshmartin1186/{project-name}
3. Root Directory: code
4. Framework: Next.js (auto-detected)
5. Click Deploy

#### Task 23: Configure Environment Variables

Add to Vercel:
```
NEXT_PUBLIC_SUPABASE_URL=[from Supabase]
NEXT_PUBLIC_SUPABASE_ANON_KEY=[from Supabase]
SUPABASE_SERVICE_ROLE_KEY=[from Supabase]
STRIPE_SECRET_KEY=sk_live_xxx
STRIPE_PUBLISHABLE_KEY=pk_live_xxx
STRIPE_WEBHOOK_SECRET=[after webhook setup]
GEMINI_API_KEY=[from Google AI Studio]
NEXT_PUBLIC_APP_URL=https://{project-name}.vercel.app
```

#### Task 24: Set Up Production Stripe Webhook

1. Go to Stripe Dashboard → Webhooks
2. Add endpoint: `https://{project-name}.vercel.app/api/webhooks/stripe`
3. Select events: checkout.session.completed, customer.subscription.updated, etc.
4. Copy webhook secret to Vercel env vars
5. Redeploy

#### Task 25: Update Supabase Redirect URLs

1. Go to Supabase → Authentication → URL Configuration
2. Add Site URL: `https://{project-name}.vercel.app`
3. Add Redirect URL: `https://{project-name}.vercel.app/auth/callback`

#### Tasks 26-30: Client Handoff

- Pre-seed client as Owner
- Pre-seed Josh as Developer
- Send handoff email (template in DEPLOYMENT_CHECKLIST.md)
- Schedule walkthrough call

---

## Commit Protocol

**CRITICAL: Commit every 2-3 tasks!**

```bash
cd ~/projects/{project-name}
git add .
git commit -m "Phase X Task Y-Z: [Brief description]"
git push
```

**Good commit messages:**
- "Phase 1 Task 1: Initialize Next.js project"
- "Phase 1 Task 2-3: Dependencies and Supabase connection"
- "Phase 2 Task 9-10: Dashboard and metrics"

**Why this matters:**
- Small commits = easier to debug if something breaks
- Continuous pushes = no lost work
- Clear messages = progress tracking for Josh

---

## Testing Protocol

**After EVERY task:**

1. **Build check:**
```bash
npm run build
```

2. **Dev server:**
```bash
npm run dev
```

3. **Browser check:**
   - Open localhost:3000
   - Check browser console for errors
   - Test the feature you just built

4. **Fix before commit:**
   - TypeScript errors? Fix them.
   - Console errors? Fix them.
   - Feature broken? Fix it.
   - THEN commit.

---

## When You Hit a Blocker

1. **Tell Josh what's wrong**

2. **Create GitHub Issue:**
```bash
# Use GitHub MCP or CLI
gh issue create --title "Blocker: [Title]" --body "## What I'm trying to do\n[Description]\n\n## What's happening\n[Error message]\n\n## What I've tried\n[Attempts]"
```

3. **Commit current work:**
```bash
git add .
git commit -m "WIP: [Task] - blocked on [issue]"
git push
```

4. **Wait for guidance**

---

## What You Should NOT Do

- ❌ Go more than 3 tasks without committing
- ❌ Push broken code (always test first)
- ❌ Skip the verification steps
- ❌ Hardcode secrets (always use environment variables)
- ❌ Skip RLS policies on database tables
- ❌ Wait until the end to deploy (deploy early, deploy often)

---

## Reference Files in ~/projects/{project-name}/

| File | What It Contains |
|------|------------------|
| EXECUTION_PLAN.md | **Start here!** Task-by-task commands |
| TECHNICAL_ARCHITECTURE.md | Tech stack decisions |
| DATABASE_SCHEMA.md | All SQL for tables |
| UI_SPECIFICATIONS.md | Page layouts and flows |
| AI_WEST_DESIGN_SYSTEM.md | Styling rules |
| API_INTEGRATIONS.md | External service setup |
| DEPLOYMENT_CHECKLIST.md | How to deploy and handoff |

---

## START BUILDING NOW

```bash
# Navigate to project
cd ~/projects/{project-name}

# Read the execution plan
cat EXECUTION_PLAN.md

# Start with Task 1!
```

---

**You are the sole builder.**

**Build. Test. Commit. Push. Deploy. Handoff.**

**Let's go! 🚀**