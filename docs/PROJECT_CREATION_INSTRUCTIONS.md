# Project Creation Instructions

## Overview

Cash creates a complete GitHub repository with 14 files for every project. This is the **first step** - before any building happens.

---

## The Process

### Step 1: Retrieve Requirements

From Fireflies transcript or other source, extract:
- Client information (name, company, email)
- Problem statement (their pain points)
- Solution requirements (what they need)
- Technical integrations (tools they use)
- Success criteria (how they'll measure success)
- Timeline expectations
- Budget hints (pricing tier)
- User count (critical for company vs solopreneur pricing)

### Step 2: Determine Business Context

**Demographic:**
- **Investor:** Fund manager, syndicator, $5M-$50M AUM
- **Solopreneur:** Consultant, coach, sales rep (1-5 users)
- **Startup:** Sales team wanting leverage (5+ users)

**Pricing Tier:**
- Base / Mid / Premium (see pricing tables)
- Custom ($5,000+ for complex requirements)

**Brain Configuration:**
- 1 Brain (single system)
- 2 Brains (two systems)
- 3 Brains (full platform)

### Step 3: Generate All 14 Files

**Specification Files (1-11):**
1. PROJECT_OVERVIEW.md - Client info, requirements, business model
2. TECHNICAL_ARCHITECTURE.md - Tech stack, API design, security
3. DATABASE_SCHEMA.md - Complete SQL for all tables
4. API_INTEGRATIONS.md - External services, authentication
5. UI_SPECIFICATIONS.md - Page layouts, component library
6. BUILD_PHASES.md - Task breakdown by phase
7. DEBUGGING_GUIDE.md - Non-technical troubleshooting
8. CLIENT_REQUIREMENTS.md - ICP, branding, business rules
9. PRODUCTIZATION_GUIDE.md - SaaS opportunity analysis
10. DEPLOYMENT_CHECKLIST.md - Production deployment steps
11. AI_WEST_DESIGN_SYSTEM.md - Copy master file exactly

**Execution Files (12-14):**
12. EXECUTION_PLAN.md - Task-by-task build commands (OVERSHARED)
13. PROJECT_INSTRUCTIONS.md - Paste into Claude Project
14. CODE_STARTER_PROMPT.md - Paste into Claude Code

### Step 4: Create GitHub Repository

```javascript
github:create_repository({
  name: "{project-name}",
  description: "{brief description}",
  private: false
})
```

### Step 5: Push All 14 Files

```javascript
github:push_files({
  owner: "joshmartin1186",
  repo: "{project-name}",
  branch: "main",
  files: [
    { path: "README.md", content: "..." },
    { path: "PROJECT_OVERVIEW.md", content: "..." },
    { path: "TECHNICAL_ARCHITECTURE.md", content: "..." },
    { path: "DATABASE_SCHEMA.md", content: "..." },
    { path: "API_INTEGRATIONS.md", content: "..." },
    { path: "UI_SPECIFICATIONS.md", content: "..." },
    { path: "BUILD_PHASES.md", content: "..." },
    { path: "DEBUGGING_GUIDE.md", content: "..." },
    { path: "CLIENT_REQUIREMENTS.md", content: "..." },
    { path: "PRODUCTIZATION_GUIDE.md", content: "..." },
    { path: "DEPLOYMENT_CHECKLIST.md", content: "..." },
    { path: "AI_WEST_DESIGN_SYSTEM.md", content: "..." },
    { path: "EXECUTION_PLAN.md", content: "..." },
    { path: "PROJECT_INSTRUCTIONS.md", content: "..." },
    { path: "CODE_STARTER_PROMPT.md", content: "..." }
  ],
  message: "Initial project documentation"
})
```

### Step 6: Report to Josh

```
Project Repository Created ✅

**Repository:** https://github.com/joshmartin1186/{project-name}

**What's Built:** [2-3 sentence summary]

**Business Model:**
- Demographic: [Investor/Solopreneur/Startup]
- Configuration: [X] Brain(s)
- Monthly: $[X]/month
- Productization: $[Y]/month potential from [N] similar customers

**Timeline:** 2-3 days with 5-7 deploy cycles

**Next Steps:**
1. Open repository
2. Create TWO separate Claude Desktop windows
3. Window 1 (Claude Project): Paste PROJECT_INSTRUCTIONS.md
4. Window 2 (Claude Code): Paste CODE_STARTER_PROMPT.md
5. Both begin automatically

Ready to build! 🚀
```

---

## The Oversharing Methodology

Files 12-14 must be **OVERSHARED** - extremely detailed instructions that leave nothing to interpretation.

### Bad Example (NOT overshared):
```markdown
Task 3: Set up Supabase connection
- Create client files
- Configure environment variables
```

### Good Example (OVERSHARED):
```markdown
Task 3: Set up Supabase connection

**Step 1: Create lib directory**
```bash
cd ~/projects/{project-name}/code
mkdir -p lib
```

**Step 2: Create browser client (lib/supabase.ts)**
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

**Step 3: Create server client (lib/supabase-server.ts)**
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
            // Handle read-only cookie store
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
NEXT_PUBLIC_SUPABASE_URL=[GET FROM JOSH]
NEXT_PUBLIC_SUPABASE_ANON_KEY=[GET FROM JOSH]
SUPABASE_SERVICE_ROLE_KEY=[GET FROM JOSH]
EOF
```

**Verification:**
```bash
ls lib/
# Expected: supabase.ts supabase-server.ts

npm run dev
# Expected: Server starts without import errors
```

**Troubleshooting:**
- If "Module not found: @supabase/ssr": Run `npm install @supabase/ssr`
- If TypeScript errors: Ensure tsconfig includes "lib" in paths
- If env errors: Restart dev server after creating .env.local

**Commit:**
```bash
cd ~/projects/{project-name}
git add .
git commit -m "Phase 1 Task 3: Supabase connection configured"
git push
```
```

---

## Why Oversharing Matters

1. **Claude Code gets exact commands** - No interpretation needed
2. **Troubleshooting built-in** - Common errors pre-answered
3. **Verification steps included** - Know when task is actually done
4. **Commit messages written** - Consistent progress tracking

---

## Quality Checklist

Before creating the GitHub repo, verify:

### All 14 Files Present
- [ ] PROJECT_OVERVIEW.md
- [ ] TECHNICAL_ARCHITECTURE.md
- [ ] DATABASE_SCHEMA.md (complete SQL)
- [ ] API_INTEGRATIONS.md
- [ ] UI_SPECIFICATIONS.md
- [ ] BUILD_PHASES.md
- [ ] DEBUGGING_GUIDE.md
- [ ] CLIENT_REQUIREMENTS.md
- [ ] PRODUCTIZATION_GUIDE.md
- [ ] DEPLOYMENT_CHECKLIST.md
- [ ] AI_WEST_DESIGN_SYSTEM.md (exact copy)
- [ ] EXECUTION_PLAN.md (OVERSHARED)
- [ ] PROJECT_INSTRUCTIONS.md (Claude Project)
- [ ] CODE_STARTER_PROMPT.md (Claude Code)

### Content Quality
- [ ] Database schema includes ALL tables with RLS policies
- [ ] EXECUTION_PLAN has exact commands for every task
- [ ] Every task has verification and troubleshooting steps
- [ ] Commit messages pre-written for each task
- [ ] PROJECT_INSTRUCTIONS tells Claude Project to use GitHub only
- [ ] CODE_STARTER_PROMPT starts with git clone command

---

## Cash's Job Ends Here

Once the GitHub repository is created with all 14 files:
1. Report the repo URL to Josh
2. Provide the "Next Steps" instructions
3. Cash's work is complete

The handoff to the parallel Claude instances happens when Josh:
1. Opens PROJECT_INSTRUCTIONS.md and pastes it into Claude Project
2. Opens CODE_STARTER_PROMPT.md and pastes it into Claude Code

From there, Claude Project orchestrates and Claude Code builds.