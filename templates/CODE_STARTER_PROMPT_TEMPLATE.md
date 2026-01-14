# CODE_STARTER_PROMPT.md - Template (v6)

> **This is pasted into Claude Code to start building.**
> **Database is already ready - Cash created Supabase first.**

---

# {Project Name} - Claude Code Build Instructions

## Your Role: SOLE BUILDER

You are Claude Code, the **ONLY** builder for this project.

**Key Point:** The database is already created and schema is applied. You do NOT need to create any Supabase resources.

---

## Project Locations

**GitHub:** https://github.com/joshmartin1186/{project-name}
**Local:** ~/projects/{project-name}/
**Supabase:** https://supabase.com/dashboard/project/{ref}

---

## Database Status

✅ Supabase project created by Cash
✅ Schema applied via migration
✅ All tables ready
✅ RLS policies configured

**Available Tables:**
- organizations
- users
- [other tables...]

**You do NOT need to:**
- Create Supabase project
- Run migrations
- Create tables

---

## Environment Variables

Create `~/projects/{project-name}/code/.env.local`:

```
# Supabase (Database ready)
NEXT_PUBLIC_SUPABASE_URL=https://{ref}.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY={anon-key}
SUPABASE_SERVICE_ROLE_KEY={service-role-key}

# Stripe (Get from Josh)
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

---

## First Steps

```bash
# Navigate to project
cd ~/projects/{project-name}

# Verify files
ls -la

# Read execution plan
cat EXECUTION_PLAN.md

# Start with Task 1!
```

---

## Build Protocol

- Follow EXECUTION_PLAN.md exactly
- **Skip any database creation tasks** (already done)
- Commit every 2-3 tasks
- Push immediately after every commit
- Test before committing
- Deploy to Vercel when ready
- Complete client handoff

---

## Commit Messages

```
Phase 1 Task 1: Initialize Next.js project
Phase 1 Task 2-3: Dependencies and Supabase connection
Phase 2 Task 9-10: Dashboard and metrics
```

---

## Reference Files

All in ~/projects/{project-name}/:
- EXECUTION_PLAN.md - Task-by-task guide
- TECHNICAL_ARCHITECTURE.md - Tech decisions
- DATABASE_SCHEMA.md - SQL (already in Supabase)
- UI_SPECIFICATIONS.md - Page layouts
- AI_WEST_DESIGN_SYSTEM.md - Styling rules
- DEPLOYMENT_CHECKLIST.md - Deploy & handoff

---

**START NOW: Read EXECUTION_PLAN.md and begin Task 1!**

**Remember: Database is ready. Focus on code!**