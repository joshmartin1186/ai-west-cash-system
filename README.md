# AI West - Cash v3 System

**Multi-Agent Development System for Complex Projects**

Cash v3 is AI West's proprietary system for coordinating multiple Claude agents to build production applications in 2-3 days. This repository contains all operating instructions, workflow documentation, and templates.

---

## System Overview

**Three Coordinated Agents:**
1. **Cash (Discovery)** - Analyzes requirements, generates comprehensive project documentation
2. **Project Claude (Architect)** - Sets up infrastructure, coordinates building, tests systematically
3. **Claude Code (Builder)** - Executes development tasks, implements fixes, updates progress

**Communication:** File-based protocol through shared workspace folder

**Timeline:** 2-3 days from requirements to production-ready deployment

**Result:** Professional applications with systematic testing, security verification, and complete documentation

---

## Documentation Structure

### Core Documentation (`/docs/`)

📋 **[CASH_OPERATING_INSTRUCTIONS.md](docs/CASH_OPERATING_INSTRUCTIONS.md)**
- Complete operating instructions for Cash
- Multiple input source handling (Fireflies, RFPs, documents, conversation)
- Dual pricing models (solopreneur vs company)
- AI West Platform Brain configuration
- 12-file generation process

📖 **[COMPLETE_WORKFLOW.md](docs/COMPLETE_WORKFLOW.md)**
- End-to-end workflow from requirements to production
- Three-agent coordination protocol
- File-based communication system
- Dual testing methodology (visual + codebase audit)
- Complete build cycle with examples

📝 **[12_FILE_TEMPLATE.md](docs/12_FILE_TEMPLATE.md)**
- Structure and templates for all 12 project files
- PROJECT_OVERVIEW, TECHNICAL_ARCHITECTURE, DATABASE_SCHEMA
- API_INTEGRATIONS, UI_SPECIFICATIONS, BUILD_PHASES
- DEBUGGING_GUIDE, CLIENT_REQUIREMENTS, PRODUCTIZATION_GUIDE
- DEPLOYMENT_CHECKLIST, DESIGN_SYSTEM, PROJECT_INSTRUCTIONS

🎯 **[PROJECT_CREATION_INSTRUCTIONS.md](docs/PROJECT_CREATION_INSTRUCTIONS.md)**
- How Cash analyzes requirements from any source
- Requirements extraction methodology
- File generation process
- Quality standards and checklists

🚀 **[DEPLOYMENT_CHECKLIST.md](docs/DEPLOYMENT_CHECKLIST.md)**
- Production deployment step-by-step guide
- Environment configuration
- Testing and verification procedures
- Troubleshooting common issues

### Templates (`/templates/`)

📄 **[PROJECT_INSTRUCTIONS_TEMPLATE.md](templates/PROJECT_INSTRUCTIONS_TEMPLATE.md)**
- Template for project-specific instructions
- GitHub repository links structure
- Setup steps and communication protocol

💬 **[COMMUNICATION_TEMPLATES.md](templates/COMMUNICATION_TEMPLATES.md)**
- current-task.md - Architect → Code task assignments
- progress.md - Code → Architect progress updates
- feedback.md - Architect → Code review results

### YouTube Assets (`/youtube/`)

🎬 **[VIDEO_DESCRIPTION.md](youtube/VIDEO_DESCRIPTION.md)**
- Complete YouTube video description
- Timestamps and structure
- Download links and CTAs

🖼️ **[THUMBNAIL_PROMPT.md](youtube/THUMBNAIL_PROMPT.md)**
- Detailed AI image generation prompts
- Thumbnail psychology and design principles
- Post-production editing checklist

---

## Quick Start

### For Cash (Discovery Agent)

1. **Read operating instructions:**
   ```
   Read docs/CASH_OPERATING_INSTRUCTIONS.md from GitHub
   ```

2. **When Josh triggers project creation:**
   - Analyze requirements from source (Fireflies/RFP/Doc/Conversation)
   - Generate 12 comprehensive project files
   - Create new GitHub repository for client project
   - Push all files to repo
   - Generate PROJECT_INSTRUCTIONS.md with GitHub links
   - Present single file to Josh

3. **Reference templates:**
   - Use templates/PROJECT_INSTRUCTIONS_TEMPLATE.md for structure
   - Follow docs/12_FILE_TEMPLATE.md for content

### For Project Claude (Architect)

1. **Receive PROJECT_INSTRUCTIONS.md from Josh**
2. **Read all project files from client's GitHub repo**
3. **Follow workflow in docs/COMPLETE_WORKFLOW.md**
4. **Use templates for communication:**
   - Write tasks to inbox-code/current-task.md
   - Read progress from outbox-code/progress.md
   - Provide feedback via inbox-code/feedback.md

### For Claude Code (Builder)

1. **Pointed to workspace folder by Josh**
2. **Read task assignment from .claude-comms/inbox-code/current-task.md**
3. **Reference project files from GitHub as needed**
4. **Build, test, update progress in outbox-code/progress.md**

---

## Key Features

### GitHub-First Approach
- ✅ All project documentation version controlled
- ✅ Single source of truth
- ✅ No file upload friction
- ✅ Always accessible reference
- ✅ Can update files mid-project

### Multi-Agent Coordination
- ✅ Context optimization (each agent focused)
- ✅ File-based communication (explicit, traceable)
- ✅ Systematic testing before deployment
- ✅ No manual coordination required

### Comprehensive Documentation
- ✅ 12 files cover every aspect of project
- ✅ Technical architecture fully specified
- ✅ Database schema with security policies
- ✅ Complete deployment checklist
- ✅ Productization strategy included

### Professional Output
- ✅ Production-ready from day one
- ✅ Security verified (RLS, input validation)
- ✅ Performance optimized (indexes, queries)
- ✅ Responsive design
- ✅ Error handling throughout

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

### Custom Builds
- $5,000/month minimum
- Bespoke features beyond standard Brains

---

## AI West Platform Brains

**Outreach Brain** - LinkedIn outbound, email sequences, multi-channel prospecting
**Content Brain** - Content creation, scheduling, multi-platform publishing
**Business Brain** - CRM intelligence, knowledge hub, deal tracking

Each project enables specific Brains based on client needs.

---

## System Requirements

- Claude Pro or Team subscription (multiple projects)
- Claude Desktop (file system access)
- GitHub account (repository management)
- Basic command line familiarity
- Understanding of: Git, databases, APIs

---

## Success Metrics

**Speed:** 2-3 days from requirements to production
**Quality:** Zero security vulnerabilities, optimized performance
**Testing:** Visual + codebase audit before deployment
**Documentation:** Complete handoff materials
**Result:** Professional applications ready for immediate use

---

## Repository Maintenance

**Main Branch:** Stable, production-ready documentation
**Updates:** Version controlled with clear commit messages
**Access:** Private repository, AI West internal use only

---

## Questions or Issues?

Contact: josh@aiwest.co
Website: aiwest.co

---

**Last Updated:** January 2026
**Version:** 3.0 (GitHub-First Architecture)

© 2025 AI West LLC. All rights reserved.