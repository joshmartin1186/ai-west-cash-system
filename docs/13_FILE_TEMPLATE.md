# 13-File Generation Template (v6)

## Overview

Cash generates 13 files for every project:
- **Files 1-11:** Specification files
- **Files 12-13:** Execution files

**v6 Key Change:** Files are generated AFTER Supabase is created, so credentials can be embedded.

---

## Quick Checklist

- [ ] File 1: PROJECT_OVERVIEW.md
- [ ] File 2: TECHNICAL_ARCHITECTURE.md
- [ ] File 3: DATABASE_SCHEMA.md (SQL that was applied to Supabase)
- [ ] File 4: API_INTEGRATIONS.md (includes Supabase URL)
- [ ] File 5: UI_SPECIFICATIONS.md
- [ ] File 6: BUILD_PHASES.md
- [ ] File 7: DEBUGGING_GUIDE.md
- [ ] File 8: CLIENT_REQUIREMENTS.md
- [ ] File 9: PRODUCTIZATION_GUIDE.md
- [ ] File 10: DEPLOYMENT_CHECKLIST.md
- [ ] File 11: AI_WEST_DESIGN_SYSTEM.md
- [ ] File 12: EXECUTION_PLAN.md
- [ ] File 13: CODE_STARTER_PROMPT.md (includes all credentials)

---

## FILE 1: PROJECT_OVERVIEW.md

```markdown
# [Project Name] - Project Overview

## Client Information
- **Client Name:** [Full Name]
- **Company:** [Company Name]
- **Email:** [Contact Email]
- **Team Size:** [Number of users]

## Problem Statement
[2-3 sentences describing their pain]

## Solution Overview
[What we're building - 3-4 sentences]

## Infrastructure (Created by Cash)

**Supabase Project:**
- URL: https://[ref].supabase.co
- Dashboard: https://supabase.com/dashboard/project/[ref]
- Status: ✅ Schema applied

**GitHub Repository:**
- URL: https://github.com/joshmartin1186/[project-name]

## AI West Platform Configuration

**Brains Included:**
- [ ] Outreach Brain
- [ ] Content Brain
- [ ] Business Brain

## Business Model

**Pricing Model:** [Solopreneur / Company / Investor]
**Configuration:** [X] Brain(s)
**Monthly Cost:** $[X]/month
**Annual Value:** $[Y]/year

## Timeline
- **Total:** 2-3 days to production
```

---

## FILE 3: DATABASE_SCHEMA.md

**Important:** This contains the exact SQL that was applied to Supabase via migration.

```markdown
# Database Schema

## Migration Status

✅ Applied to Supabase project: [ref]
✅ Migration name: initial_schema

## Complete Schema

```sql
-- This SQL was applied via supabase:apply_migration

-- Organizations
CREATE TABLE organizations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  slug TEXT UNIQUE,
  stripe_customer_id TEXT,
  subscription_status TEXT DEFAULT 'trialing',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Users
CREATE TABLE users (
  id UUID PRIMARY KEY REFERENCES auth.users(id),
  organization_id UUID REFERENCES organizations(id) NOT NULL,
  email TEXT NOT NULL,
  role TEXT DEFAULT 'viewer',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- RLS Policies
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can view own org" ON users
  FOR SELECT USING (organization_id = (
    SELECT organization_id FROM users WHERE id = auth.uid()
  ));

-- [Additional tables...]
```
```

---

## FILE 4: API_INTEGRATIONS.md

**Now includes Supabase credentials:**

```markdown
# External API Integrations

## Supabase (Already Configured)

**Project URL:** https://[ref].supabase.co
**Dashboard:** https://supabase.com/dashboard/project/[ref]
**Status:** ✅ Schema applied, ready to use

### Credentials
```
NEXT_PUBLIC_SUPABASE_URL=https://[ref].supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=[anon-key]
SUPABASE_SERVICE_ROLE_KEY=[ask Josh for this]
```

## Stripe (Configure During Build)

[Standard Stripe setup...]

## Environment Variables Summary

```bash
# Supabase (Ready)
NEXT_PUBLIC_SUPABASE_URL=https://[ref].supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=[anon-key]
SUPABASE_SERVICE_ROLE_KEY=[service-role-key]

# Stripe (Get from Josh)
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
```
```

---

## FILE 12: EXECUTION_PLAN.md

**Updated to reflect database is ready:**

```markdown
# Execution Plan

## Pre-Build Status

✅ Supabase project created: https://[ref].supabase.co
✅ Database schema applied
✅ All tables ready
✅ RLS policies configured

**You do NOT need to create the database - Cash already did it.**

## Repository Information

**GitHub Repo:** https://github.com/joshmartin1186/[project-name]
**Local Clone:** ~/projects/[project-name]/

## Phase 1: Foundation

### Task 1: Initialize Next.js Project

```bash
cd ~/projects/[project-name]
mkdir -p code
cd code
npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir=false --import-alias="@/*"
```

### Task 2: Install Dependencies

```bash
npm install @supabase/supabase-js @supabase/ssr stripe @stripe/stripe-js
npx shadcn-ui@latest init
npx shadcn-ui@latest add button card input label
```

### Task 3: Create Environment File

```bash
cat > .env.local << 'EOF'
# Supabase (Database ready - schema already applied by Cash)
NEXT_PUBLIC_SUPABASE_URL=https://[ref].supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=[anon-key]
SUPABASE_SERVICE_ROLE_KEY=[service-role-key]

# Stripe
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
EOF
```

**Note:** Skip any database creation tasks - tables already exist!

[Continue with remaining tasks...]
```

---

## FILE 13: CODE_STARTER_PROMPT.md

**Complete prompt with all credentials:**

```markdown
# [Project Name] - Claude Code Build Instructions

## Your Role

You are Claude Code, the sole builder. Database is already ready.

## Project Locations

**Git Remote:** https://github.com/joshmartin1186/[project-name]
**Local Folder:** ~/projects/[project-name]/
**Supabase Dashboard:** https://supabase.com/dashboard/project/[ref]

## Database Status

✅ Supabase project created by Cash
✅ Schema applied via migration
✅ All tables ready to use
✅ RLS policies configured

**You do NOT need to create any database tables.**

## Environment Variables

Create `~/projects/[project-name]/code/.env.local`:

```
NEXT_PUBLIC_SUPABASE_URL=https://[ref].supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=[anon-key]
SUPABASE_SERVICE_ROLE_KEY=[service-role-key]
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## Tables Available

[List all tables from schema]

## First Steps

```bash
cd ~/projects/[project-name]
cat EXECUTION_PLAN.md
# Begin Task 1!
```

**START NOW!**
```

---

## Key v6 Differences

| Aspect | v5 | v6 |
|--------|----|----||
| Supabase | Created by Claude Code | Created by Cash |
| Schema | Applied during build | Applied before GitHub |
| Credentials | Manual entry | Embedded in files |
| Database tasks | In EXECUTION_PLAN | Skipped - already done |
| Starter prompt | Basic | Includes all credentials |