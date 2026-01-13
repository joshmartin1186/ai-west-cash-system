# Communication Templates

> **These templates are used in the .claude-comms folder for coordination between Project Claude and Claude Code**

---

## current-task.md Template

```markdown
# Current Task Assignment

**Phase:** [Phase Number] - [Phase Name]
**Tasks:** [Task Numbers]

## Instructions
Execute tasks [X-Y] from BUILD_PHASES.md in /docs/.

All code goes in the `/code/` subfolder.

Update `.claude-comms/outbox-code/progress.md` as you complete each task.

When phase complete, write final status and signal for review.

## Important Notes
- Use environment variables from .env.local
- Follow AI West Design System for styling
- Create system logs for major operations
- Test locally before marking complete
- Implement multi-tenant isolation (organization_id on all queries)

## Phase-Specific Guidance

### Phase 1: Foundation
- Initialize Next.js with TypeScript and Tailwind
- Set up Supabase connection and authentication
- Create complete database schema with RLS policies
- Verify localhost:3000 runs without errors

### Phase 2: Core UI
- Build all user-facing pages
- Connect to real Supabase data
- Implement settings and user management
- Test all pages load correctly

### Phase 3: Automation
- Integrate external APIs
- Build business logic and workflows
- Set up webhook handlers
- Test all integrations end-to-end

### Phase 4: Deploy & Handoff
- Push to GitHub
- Deploy to Vercel
- Configure production environment
- Test production thoroughly
```

---

## progress.md Template

```markdown
# Build Progress

**Phase:** [Phase Number] - [Phase Name]
**Status:** Not Started | In Progress | Complete - Ready for Review
**Last Updated:** [Timestamp]

## Completed Tasks
[None yet]

## Current Task
[Description of what you're working on right now]

## Recent Updates
[Latest changes and accomplishments]

## Blockers
None | [Description of any issues preventing progress]

## Next Steps
[What's coming next in the build sequence]

## Notes
[Any observations, decisions, or context for Project Claude]
```

---

## feedback.md Template

```markdown
# Review Feedback - Phase [X]

**Reviewed:** [Timestamp]
**Status:** In Review | Issues Found | Approved

---

## Visual Testing Results (Chrome MCP)

### ✅ Passed
- [List features that work correctly]

### ❌ Failed
- [List issues found with specific locations and expected vs actual behavior]

---

## Codebase Audit Results

### 🔴 CRITICAL Issues (MUST FIX BEFORE PROCEEDING)

#### Issue #1: [Title]
**Location:** [File path and line number]
**Problem:** [Detailed description]
**Fix:** [Specific code or approach to resolve]

### 🟡 HIGH Priority Issues (SHOULD FIX BEFORE PROCEEDING)

#### Issue #1: [Title]
**Location:** [File path]
**Problem:** [Description]
**Fix:** [Solution]

### 🟠 MEDIUM Priority Issues (CAN ADDRESS IN NEXT PHASE)

#### Issue #1: [Title]
**Location:** [File path]
**Problem:** [Description]
**Fix:** [Solution]

### 🔵 LOW Priority Issues (NICE TO HAVE)

#### Issue #1: [Title]
**Location:** [File path]
**Problem:** [Description]
**Fix:** [Solution]

---

## Required Actions

### Before Proceeding to Next Phase:
1. ✅ Fix CRITICAL Issue #1: [Brief description]
2. ✅ Fix CRITICAL Issue #2: [Brief description]
3. ✅ Fix HIGH Issue #1: [Brief description]

### Can Address Later:
- MEDIUM priority issues (next phase)
- LOW priority issues (Phase 3 or 4)

---

## Next Steps

1. Read this feedback completely
2. Fix all CRITICAL issues first
3. Fix all HIGH priority issues
4. Fix visual issues
5. Update progress.md when complete
6. Signal ready for re-review

---

**Re-review will check:**
- All CRITICAL and HIGH issues resolved
- Visual issues fixed
- No new issues introduced
- Local testing still passing
```

---

## blockers.md Template

```markdown
# Blockers

**Last Updated:** [Timestamp]

## Active Blockers

### Blocker #1: [Title]
**Severity:** Critical | High | Medium | Low
**Impact:** [What this prevents]
**Details:** [Full description of the problem]
**Attempted Solutions:** [What has been tried]
**Needs:** [What's required to unblock]

---

## Resolved Blockers

### Blocker #1: [Title] - RESOLVED
**Resolution:** [How it was solved]
**Resolved:** [Timestamp]
```

---

## Folder Structure

```
.claude-comms/
├── inbox-code/           # Project Claude → Claude Code
│   ├── current-task.md
│   └── feedback.md
├── outbox-code/          # Claude Code → Project Claude
│   ├── progress.md
│   └── blockers.md
└── status/               # Quick status checks
    ├── current-phase.txt
    └── last-action.txt
```

---

## File Usage Guidelines

### current-task.md (Project Claude writes)
- **When:** At start of each phase
- **Purpose:** Assign clear tasks to Claude Code
- **Updates:** Only when starting new phase or major pivot

### progress.md (Claude Code writes)
- **When:** After completing each task
- **Purpose:** Keep Project Claude informed of progress
- **Updates:** Frequently (after each significant accomplishment)

### feedback.md (Project Claude writes)
- **When:** After reviewing completed phase
- **Purpose:** Provide detailed, actionable feedback
- **Updates:** Once per phase review cycle

### blockers.md (Claude Code writes)
- **When:** Encountering issues that prevent progress
- **Purpose:** Alert Project Claude to problems needing attention
- **Updates:** Immediately when blocked, update when resolved

### status/current-phase.txt (Both write)
- **Format:** Simple one-line status
- **Example:** "Phase 2: Core UI - In Progress"
- **Purpose:** Quick status check without reading full files

### status/last-action.txt (Both write)
- **Format:** One-line description of most recent action
- **Example:** "Completed Task 15: User management page"
- **Purpose:** Quick check of latest activity

---

## Communication Best Practices

### For Claude Code:
- ✅ Update progress.md after each task completion
- ✅ Signal clearly when phase is complete and ready for review
- ✅ Create blockers.md entry immediately when stuck
- ✅ Include specific details (file paths, line numbers, error messages)
- ❌ Don't wait until end of phase to update progress
- ❌ Don't hide or minimize issues

### For Project Claude:
- ✅ Write clear, specific task assignments
- ✅ Categorize feedback by priority (CRITICAL, HIGH, MEDIUM, LOW)
- ✅ Provide exact code fixes, not just descriptions
- ✅ Acknowledge progress and accomplishments
- ❌ Don't give vague feedback ("make it better")
- ❌ Don't skip testing before providing feedback

---

## Example Communication Flow

**Phase Start:**
```
Project Claude → inbox-code/current-task.md
"Execute tasks 9-15 from BUILD_PHASES.md..."
```

**During Build:**
```
Claude Code → outbox-code/progress.md
"✅ Task 9: Dashboard page created
✅ Task 10: Connected to Supabase
🔨 Task 11: Building settings page..."
```

**If Blocked:**
```
Claude Code → outbox-code/blockers.md
"Cannot connect to external API - missing credentials"
```

**Phase Complete:**
```
Claude Code → outbox-code/progress.md
"Phase 2 complete. All tasks done. Ready for review.
Testing results: [summary]"
```

**Review:**
```
Project Claude → inbox-code/feedback.md
"Phase 2 Review: 2 CRITICAL issues, 3 HIGH issues.
Fix before Phase 3..."
```

**Fixes Complete:**
```
Claude Code → outbox-code/progress.md
"All CRITICAL and HIGH issues resolved.
Ready for re-review."
```

**Approval:**
```
Project Claude → inbox-code/current-task.md
"Phase 2 APPROVED. Starting Phase 3: Automation
Execute tasks 16-22..."
```

---

## Tips for Success

1. **Be Specific:** Include file paths, line numbers, exact error messages
2. **Update Often:** Don't let Project Claude wonder what's happening
3. **Categorize Clearly:** Use priority labels (CRITICAL, HIGH, etc.)
4. **Show Progress:** Celebrate small wins, build momentum
5. **Flag Early:** Report blockers immediately, don't struggle in silence
6. **Test Thoroughly:** Verify everything works before signaling complete
7. **Read Carefully:** Understand feedback before implementing fixes
8. **Verify Fixes:** Test that solutions actually resolve issues
9. **Keep Context:** Reference previous issues and decisions
10. **Stay Organized:** Update status files for quick reference

---

This file-based communication system enables two Claudes to work together efficiently without context switching or information loss.
