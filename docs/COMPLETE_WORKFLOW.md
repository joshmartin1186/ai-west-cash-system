# AI-Assisted Development Workflow - Complete System

## System Overview

This workflow coordinates three AI assistants for rapid application development:
1. **Discovery Assistant** - Analyzes requirements, generates documentation
2. **Architect Assistant** - Sets up infrastructure, manages build, tests comprehensively
3. **Code Assistant** - Executes tasks, builds application, implements fixes

**Key Innovation:** Single local folder with file-based communication enables parallel work while maintaining optimal context size.

---

## The Complete Workflow

### Step 1: Discovery & Documentation Generation

**Trigger:** "Analyze [source] and create project documentation"

**Discovery Assistant Process:**

#### 1a. Retrieve Requirements
```javascript
// From transcription service API
const transcript = await transcriptionService.get({
  search: "[client/project name]",
  sort_by: "date",
  limit: 1
})

// Or from uploaded document
const requirements = readDocument("[filepath]")

// Or from direct conversation
const requirements = parseUserInput()
```

**Extract:**
- Client pain points and frustrations
- Technical requirements and preferences
- Business model and success criteria
- Timeline expectations
- Budget constraints
- User personas and workflows
- Integration needs
- Team size and user count

#### 1b. Analyze Business Context

**Determine demographic:**
- Individual/freelancer
- Small team (2-10 people)
- Mid-size company (10-50 people)
- Enterprise (50+ people)

**Determine pricing model:**
- Standard tier (single system)
- Professional tier (multiple systems)
- Enterprise tier (full platform)
- Custom development

**Identify productization potential:**
- Similar customer segments
- Market opportunity size
- SaaS positioning strategy

#### 1c. Generate All 12 Project Files

**Complete documentation package includes:**

1. PROJECT_OVERVIEW.md - Business context and goals
2. TECHNICAL_ARCHITECTURE.md - Complete tech stack
3. DATABASE_SCHEMA.md - SQL schema with RLS
4. API_INTEGRATIONS.md - External services
5. UI_SPECIFICATIONS.md - All pages and flows
6. BUILD_PHASES.md - Task breakdown by phase
7. DEBUGGING_GUIDE.md - Troubleshooting
8. CLIENT_REQUIREMENTS.md - Specific needs
9. PRODUCTIZATION_GUIDE.md - SaaS strategy
10. DEPLOYMENT_CHECKLIST.md - Deploy steps
11. DESIGN_SYSTEM.md - Styling guidelines
12. PROJECT_INSTRUCTIONS.md - How to use docs

#### 1d. Create Communication Templates

**current-task.md template:**
```markdown
# Current Task Assignment

**Phase:** [X] - [Phase Name]
**Tasks:** [Task Numbers]

## Instructions
Execute tasks [X-Y] from BUILD_PHASES.md.
All code goes in /code/ subfolder.
Update progress.md as you complete tasks.

## Important Notes
- Use environment variables from .env.local
- Follow design system guidelines
- Test locally before marking complete
```

**progress.md template:**
```markdown
# Build Progress

**Phase:** [X]
**Status:** Not Started
**Last Updated:** [Timestamp]

## Completed Tasks
[None yet]

## Current Task
[Next task to start]

## Blockers
None

## Next Steps
[What's coming next]
```

**feedback.md template:**
```markdown
# Review Feedback - Phase [X]

**Reviewed:** [Timestamp]
**Status:** [In Review / Issues Found / Approved]

## Visual Testing Results
[UI testing findings]

## Codebase Audit Results
[Code quality findings]

## Required Fixes

### CRITICAL
[Must fix before proceeding]

### HIGH
[Should fix before proceeding]

### MEDIUM
[Can fix in next phase]

### LOW
[Nice to have]
```

#### 1e. Package & Deliver

```bash
# Create output structure
/project-name-docs/
├── [12 project files]
├── /comms-templates/
│   ├── current-task.md
│   ├── progress.md
│   └── feedback.md
└── README.md
```

**Present to user:**
- Project summary (2-3 sentences)
- Technical approach
- Business model
- Timeline estimate
- Next steps

---

### Step 2: Project Setup (Architect Assistant)

**User Action:** Creates AI assistant project, uploads all files, types "begin"

**Architect Assistant Response:**

#### 2a. Create Local Folder Structure

```bash
# Create root project folder
mkdir -p /workspace/[project-name]
cd /workspace/[project-name]

# Create complete structure
mkdir -p code
mkdir -p .claude-comms/{inbox-code,outbox-code,status}
mkdir -p docs

# Copy project files to docs
cp [uploaded files] ./docs/

# Create initial communication files
cat > .claude-comms/inbox-code/current-task.md << EOF
# Current Task Assignment

**Phase:** 1 - Foundation
**Tasks:** 1-8

## Instructions
Execute tasks 1-8 from BUILD_PHASES.md in /docs/.
All code goes in the /code/ subfolder.
Update .claude-comms/outbox-code/progress.md as you complete each task.
EOF

cat > .claude-comms/outbox-code/progress.md << EOF
# Build Progress

**Phase:** 1 - Foundation
**Status:** Not Started
**Last Updated:** $(date)

## Completed Tasks
[None yet]

## Current Task
Waiting for Code Assistant to begin
EOF
```

#### 2b. Create Database Instance

```bash
# Using database service API
createDatabaseProject({
  name: "[Project Name]",
  region: "optimal-region",
  plan: "production"
})

# Apply complete schema from docs/DATABASE_SCHEMA.md
applyMigrations(extractSQL(schema))

# Verify tables created
listTables()
```

#### 2c. Create Environment Variables

```bash
# Create .env.local in code folder
cat > code/.env.local << EOF
# Database
DATABASE_URL=[connection string]
DATABASE_KEY=[service key]

# Authentication
AUTH_SECRET=[generated secret]

# External APIs
API_KEY_1=[key]
API_KEY_2=[key]

# App Config
NEXT_PUBLIC_APP_URL=http://localhost:3000
EOF
```

#### 2d. Report Completion

```
Setup complete! ✅

Project Folder: /workspace/[project-name]

Database Instance:
- URL: [database url]
- Status: Active
- Tables: [list created tables]

Structure Created:
- /code/ - Ready for app initialization
- /docs/ - All 12 project files loaded
- /.claude-comms/ - Communication layer configured
- .env.local - Environment variables configured

Next Steps:
1. Open Code Assistant in same workspace
2. Select folder: /workspace/[project-name]
3. Type: "begin"
```

---

### Step 3: Build Phase (Code Assistant)

**User Action:** Opens Code Assistant, selects folder, types "begin"

**Code Assistant Response:**

#### 3a. Read Assignment

```bash
# Read what to do
cat docs/BUILD_PHASES.md
cat .claude-comms/inbox-code/current-task.md

# Understand structure
ls -la
cat README.md
```

#### 3b. Execute Phase 1 Tasks

**Task 1: Initialize Framework**
```bash
cd code
npx create-next-app@latest . --typescript --tailwind --app
```

**Task 2: Install Dependencies**
```bash
npm install [required packages]
```

**Task 3: Database Connection**
```typescript
// lib/database.ts
import { createClient } from '@database/client'

const dbUrl = process.env.DATABASE_URL!
const dbKey = process.env.DATABASE_KEY!

export const db = createClient(dbUrl, dbKey)
```

**Tasks 4-8: Continue building foundation...**

#### 3c. Update Progress After Each Task

```bash
# After completing Task 1
cat >> ../.claude-comms/outbox-code/progress.md << EOF

## Progress Update - $(date)
✅ Task 1: Framework initialized
- App router configured
- TypeScript enabled
- Styling installed
EOF

# Update status
echo "Task 1 complete: Framework initialized" > ../.claude-comms/status/last-action.txt
```

#### 3d. Test Locally Throughout

```bash
# After each major task
npm run dev

# Verify:
# - No compilation errors
# - Server starts successfully
# - Can access localhost:3000
# - No console errors
```

#### 3e. Signal Phase Complete

```bash
cat > ../.claude-comms/outbox-code/progress.md << EOF
# Build Progress

**Phase:** 1 - Foundation
**Status:** Complete - Ready for Review
**Last Updated:** $(date)

## Completed Tasks
✅ Task 1: Framework initialized
✅ Task 2: All dependencies installed
✅ Task 3: Database connection configured
✅ Task 4: Schema verified
✅ Task 5: Authentication implemented
✅ Task 6: Base layout created
✅ Task 7: Environment configured
✅ Task 8: Local testing successful

## Testing Results
- ✅ npm run dev runs without errors
- ✅ Accessible at localhost:3000
- ✅ Database connection verified
- ✅ No TypeScript errors
- ✅ No console errors

Phase 1 complete: $(date)
EOF
```

---

### Step 4: Testing Phase (Architect Assistant)

**User Action:** Switches to Architect Assistant, types "Review Phase 1"

**Architect Assistant Response:**

#### 4a. Check Status

```bash
# Read progress report
cat .claude-comms/outbox-code/progress.md

# Verify phase complete
cat .claude-comms/status/current-phase.txt
```

#### 4b. Visual Testing (UI Testing Tool)

```javascript
// Launch browser to local dev server
await browser.launch('http://localhost:3000')
await browser.screenshot()

// Test homepage
const homeLoads = await browser.evaluate(() => {
  return document.querySelector('h1')?.textContent
})

// Test signup flow
await browser.navigate('/signup')
await browser.fill('input[type="email"]', 'test@test.com')
await browser.fill('input[type="password"]', 'Test123!')
await browser.click('button[type="submit"]')
await browser.screenshot()

// Test login flow
await browser.navigate('/login')
await browser.fill('input[type="email"]', 'test@test.com')
await browser.fill('input[type="password"]', 'Test123!')
await browser.click('button[type="submit"]')
await browser.screenshot()

// Test dashboard
await browser.navigate('/dashboard')
await browser.screenshot()

// Check for console errors
const errors = await browser.getConsoleErrors()
```

**Visual Testing Report:**
```
✅ Homepage loads correctly
✅ Signup form displays
✅ Login form displays
✅ Can create account
❌ Login form colors incorrect
❌ Dashboard redirect broken - 404 error
✅ No console errors
❌ Mobile view: navbar overlaps content
```

#### 4c. Codebase Audit (File Reading)

```bash
# Read all code files
find code/app -type f -name "*.tsx" -o -name "*.ts"
find code/lib -type f -name "*.ts"
find code/app/api -type f -name "route.ts"
```

**Security Review:**
```bash
# Check for SQL injection
grep -r "SELECT.*\${" code/

# Check environment variables
grep -r "process.env" code/ | grep -v "NEXT_PUBLIC"

# Check for exposed secrets
grep -r "sk_live" code/
```

**Security Findings:**
```
❌ CRITICAL: SQL injection in code/app/api/search/route.ts line 15
   Raw string interpolation in query

❌ CRITICAL: Missing RLS policy on 'activities' table
   Table exists but has no security policy

✅ Environment variables used correctly
✅ No hardcoded secrets found
```

**Performance Review:**
```bash
# Check for N+1 queries
grep -r "for.*await" code/app

# Check React optimization
grep -r "React.memo" code/
```

**Performance Findings:**
```
❌ HIGH: N+1 query in dashboard
   code/app/dashboard/page.tsx line 42
   Fetching users in loop instead of join

✅ Database indexes on all foreign keys

❌ MEDIUM: Missing React.memo on ExpensiveChart
   Re-renders on every parent update
```

#### 4d. Write Comprehensive Feedback

```bash
cat > .claude-comms/inbox-code/feedback.md << EOF
# Phase 1 Review - Issues Found

**Reviewed:** $(date)
**Status:** Issues Found - Fixes Required

## Visual Testing Results

### ✅ Passed
- Homepage loads correctly
- Signup and login forms work
- Can create accounts and authenticate
- No console errors

### ❌ Failed
1. **Login form colors incorrect**
   Location: code/app/login/page.tsx
   Problem: Using default colors instead of brand colors
   Fix: Update Button component to use primary color

2. **Dashboard redirect broken**
   Location: code/app/auth/callback/route.ts
   Problem: Redirecting to /dashboard but route is /app/dashboard
   Fix: Change redirect URL to '/app/dashboard'

3. **Mobile navbar overlap**
   Location: code/app/layout.tsx
   Problem: Navbar doesn't collapse on mobile
   Fix: Add responsive breakpoints

## Codebase Audit Results

### 🔴 CRITICAL Issues

#### 1. SQL Injection Vulnerability
**Location:** code/app/api/search/route.ts line 15
**Problem:**
\`\`\`typescript
// UNSAFE
const results = await db
  .from('items')
  .select('*')
  .filter('name', 'ilike', \`%\${query}%\`)
\`\`\`
**Fix:**
\`\`\`typescript
// SAFE - Use parameterized query
const results = await db
  .from('items')
  .select('*')
  .ilike('name', \`%\${query}%\`)
\`\`\`

#### 2. Missing RLS Policy
**Location:** Database - 'activities' table
**Problem:** No Row Level Security policy
**Fix:**
\`\`\`sql
ALTER TABLE activities ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users see own org" 
ON activities FOR ALL 
USING (organization_id = (
  SELECT organization_id FROM users WHERE id = auth.uid()
));
\`\`\`

### 🟡 HIGH Priority Issues

#### 1. N+1 Query Problem
**Location:** code/app/dashboard/page.tsx line 42
**Problem:**
\`\`\`typescript
// Bad - 25 database calls
const users = await getOrganizationUsers()
for (const user of users) {
  const activity = await getUserActivity(user.id)
}
\`\`\`
**Fix:**
\`\`\`typescript
// Good - Single query with join
const usersWithActivity = await db
  .from('users')
  .select('*, activities(*)')
  .eq('organization_id', orgId)
\`\`\`

## Required Actions

**Before proceeding to Phase 2:**
1. ✅ Fix CRITICAL Issue #1: SQL injection
2. ✅ Fix CRITICAL Issue #2: RLS policy
3. ✅ Fix HIGH Issue #1: N+1 query
4. ✅ Fix visual issues (colors, redirect, mobile nav)

## Next Steps

Read this feedback completely, fix all issues, update progress.md when complete, signal ready for re-review.
EOF
```

---

### Step 5: Code Implements Feedback (Code Assistant)

**User Action:** Switches to Code Assistant, types "Read feedback and fix"

**Code Assistant implements all fixes, tests, updates progress.md**

---

### Step 6: Architect Assistant Re-Reviews

**User Action:** Switches to Architect, types "Re-review Phase 1"

**Architect verifies all fixes, approves phase, assigns Phase 2**

---

### Steps 7-10: Repeat Pattern

**The cycle continues through all phases:**
- Phase 2: Core UI (pages + data)
- Phase 3: Integration (APIs + automation)
- Phase 4: Deploy & Handoff

---

### Final Step: Deployment

**User Action:** "Deploy to production"

**Architect Assistant handles:**
1. GitHub repository creation
2. Production hosting setup
3. Environment variable configuration
4. OAuth redirect updates
5. Webhook configuration
6. Production testing
7. Client handoff documentation

---

## Key Benefits

### Context Optimization
- Discovery: One-time, focused on requirements
- Architect: Documentation + status (~35K tokens)
- Code: Task assignments + code (~25K tokens)
- No compression needed

### Single Folder = Single Source of Truth
- Both assistants work in same folder
- Communication via explicit files
- User just switches between assistants
- All state persists in files

### Comprehensive Testing
- Visual catches UX issues
- Audit catches security/performance/quality
- Issues fixed before production
- Professional output

### Speed
- 2-3 days from requirements to production
- Minimal iteration cycles
- Clear handoff points
- Systematic process

---

## Timeline

**Typical Project:**
- Discovery: 30-45 minutes
- Setup: 10-15 minutes
- Phase 1 (Foundation): 2-3 hours
- Phase 2 (Core UI): 3-4 hours
- Phase 3 (Integration): 4-6 hours
- Phase 4 (Deploy): 1-2 hours
- **Total: 2-3 days to production**

---

## Success Metrics

**Quality:**
- Zero security vulnerabilities
- No performance issues (N+1 queries, etc.)
- Comprehensive error handling
- Professional polish

**Speed:**
- Same-day setup to building
- 2-3 days to production
- Minimal back-and-forth

**Professional Delivery:**
- Clear handoff process
- Complete documentation
- Immediate support access
- Production-ready from day one

---

**This workflow transforms requirements into production-ready applications through systematic AI-assisted development.**
