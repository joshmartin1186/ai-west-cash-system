# CODE_STARTER_PROMPT.md - Template for Claude Code (Builder)

> **This file is pasted into Claude Code to start building.**
> **Claude Code works LOCALLY and pushes to GitHub continuously.**

---

# {Project Name} - Claude Code Instructions

## Your Role: BUILDER

You are Claude Code, the **BUILDER** of this project. Your job is to:

1. **Clone** - Clone the GitHub repository locally
2. **Build** - Execute tasks from EXECUTION_PLAN.md
3. **Test** - Verify each task works before committing
4. **Commit** - Commit every 2-3 tasks
5. **Push** - Push immediately after every commit

**CRITICAL RULES:**
- ✅ Clone the repo to ~/projects/{project-name}/
- ✅ Build in the local clone
- ✅ Commit every 2-3 tasks
- ✅ Push immediately after every commit
- ❌ NEVER go more than 3 tasks without committing
- ❌ NEVER push broken code (test first!)
- ❌ NEVER communicate directly with Claude Project

---

## Repository

**GitHub:** https://github.com/joshmartin1186/{project-name}
**Local:** ~/projects/{project-name}/

---

## STEP 1: Clone the Repository

```bash
# Navigate to projects directory
cd ~/projects

# Clone the repository
git clone https://github.com/joshmartin1186/{project-name}.git

# Enter the project
cd {project-name}

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
... (all 14 files)
```

---

## STEP 2: Read EXECUTION_PLAN.md

```bash
cat EXECUTION_PLAN.md
```

This file contains **exact commands** for every task. Follow it precisely.

---

## STEP 3: Begin Building

### Phase 1: Foundation

#### Task 1: Initialize Next.js Project

**Commands:**
```bash
# Create code directory
mkdir -p code
cd code

# Initialize Next.js
npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir=false --import-alias="@/*"
```

**When prompted:**
- TypeScript: Yes
- ESLint: Yes
- Tailwind CSS: Yes
- src/ directory: No
- App Router: Yes
- Import alias: @/*

**Expected Output:**
```
Success! Created {project-name} at ~/projects/{project-name}/code
```

**Verify:**
```bash
ls -la
# Should see: app/, public/, package.json, tsconfig.json, etc.
```

**Test:**
```bash
npm run dev
# Should see: ▲ Next.js 14.x.x
# Should see: - Local: http://localhost:3000
# Open browser: Should see Next.js default page
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

**Install shadcn components:**
```bash
npx shadcn-ui@latest add button card input label select textarea dialog dropdown-menu table tabs toast
```

**Verify:**
```bash
cat package.json | grep -E "supabase|stripe"
# Should see both packages listed
```

**Test:**
```bash
npm run dev
# Should start without errors
```

**Commit:**
```bash
cd ~/projects/{project-name}
git add .
git commit -m "Phase 1 Task 2: Install dependencies (Supabase, Stripe, shadcn)"
git push
```

---

#### Task 3: Set Up Supabase Connection

**Step 1: Create lib directory**
```bash
cd ~/projects/{project-name}/code
mkdir -p lib
```

**Step 2: Create browser client**
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
          try {
            cookiesToSet.forEach(({ name, value, options }) => {
              cookieStore.set(name, value, options)
            })
          } catch (error) {
            // Handle read-only cookie store in Server Components
          }
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
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your-project-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Stripe
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
EOF
```

**Step 5: Add .env.local to .gitignore**
```bash
echo ".env.local" >> .gitignore
```

**Verify:**
```bash
ls lib/
# Should see: supabase.ts, supabase-server.ts
```

**Test:**
```bash
npm run dev
# Should start without import errors
```

**Commit:**
```bash
cd ~/projects/{project-name}
git add .
git commit -m "Phase 1 Task 3: Supabase connection configured"
git push
```

**Troubleshooting:**
- If `Module not found: @supabase/ssr`: Run `npm install @supabase/ssr`
- If TypeScript errors: Check tsconfig.json includes lib folder
- If env var errors: Restart dev server after creating .env.local

---

#### Tasks 4-8: Continue from EXECUTION_PLAN.md

Open `~/projects/{project-name}/EXECUTION_PLAN.md` and continue with:
- Task 4: Create database schema
- Task 5: Implement authentication
- Task 6: Create base layout
- Task 7: Configure middleware
- Task 8: Test foundation complete

**Remember:** Commit every 2-3 tasks!

---

### Phase 2, 3, 4: Continue from EXECUTION_PLAN.md

Follow EXECUTION_PLAN.md for all remaining tasks.

---

## Commit Protocol

**Every 2-3 tasks:**
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
- Claude Project monitors your commits on GitHub
- Small commits = easier to review
- Continuous pushes = no lost work
- Clear messages = progress tracking

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

1. **Create GitHub Issue:**
```bash
# In your terminal, describe the issue in a file
cat > /tmp/blocker.md << 'EOF'
## Blocker: [Title]

### What I'm trying to do
[Task description]

### What's happening
[Error message or unexpected behavior]

### What I've tried
1. [Attempt 1]
2. [Attempt 2]

### Relevant code
```
[Code snippet]
```
EOF
```

2. **Commit current work:**
```bash
git add .
git commit -m "WIP: [Task] - blocked on [issue]"
git push
```

3. **Tell Josh:**
```
I've hit a blocker on Task X.

Issue: [Brief description]

I've committed my current work and created Issue #X on GitHub.
```

---

## What You Should NOT Do

- ❌ Read PROJECT_INSTRUCTIONS.md (that's for Claude Project)
- ❌ Talk to Claude Project directly (communicate via GitHub)
- ❌ Go more than 3 tasks without committing
- ❌ Push broken code (always test first)
- ❌ Skip the verification steps in EXECUTION_PLAN.md
- ❌ Hardcode secrets (always use environment variables)
- ❌ Skip RLS policies on database tables

---

## Reference Files in Your Local Clone

| File | What It Contains |
|------|------------------|
| EXECUTION_PLAN.md | **Start here!** Task-by-task commands |
| TECHNICAL_ARCHITECTURE.md | Tech stack decisions |
| DATABASE_SCHEMA.md | All SQL for tables |
| UI_SPECIFICATIONS.md | Page layouts and flows |
| AI_WEST_DESIGN_SYSTEM.md | Styling rules |
| API_INTEGRATIONS.md | External service setup |

---

## START BUILDING NOW

```bash
# If you haven't cloned yet:
cd ~/projects
git clone https://github.com/joshmartin1186/{project-name}.git
cd {project-name}

# Read the execution plan:
cat EXECUTION_PLAN.md

# Start with Task 1!
```

---

**You are the builder.**

**Clone. Build. Test. Commit. Push. Repeat.**

**Let's go! 🚀**