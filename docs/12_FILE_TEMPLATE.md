# 12-File Generation Template

## Quick Checklist

When generating project files, use this as your template to ensure completeness.

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
- **Pricing Tier:** [Base/Mid/Premium]
- **Monthly Cost:** $[X]/month
- **Annual Value:** $[Y]/year
- **Systems Included:** [List systems]

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
- **Phase 1 (Foundation):** [Dates] - 2-3 hours
- **Phase 2 (Core UI):** [Dates] - 3-4 hours
- **Phase 3 (Automation):** [Dates] - 4-6 hours
- **Phase 4 (Deploy & Handoff):** [Dates] - 1-2 hours
- **Phase 5 (Productization):** [Date] - 30-60 minutes
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

## External Integrations

### [Integration 1 Name]
- **Purpose:** [What it does]
- **API:** [API name/version]
- **Authentication:** [API key, OAuth, etc.]
- **Rate Limits:** [Limits]
- **Cost:** [Pricing tier]

### [Integration 2 Name]
[Repeat for each integration from transcript]

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

### Organization Model
- One organization per client/customer
- Users belong to one organization
- All data scoped to organization_id
- Subscription tied to organization

### Role-Based Access Control
- **Owner:** Full access including billing
- **Admin:** All features except billing
- **Developer:** Debug console + logs access
- **Viewer:** Read-only access

## Data Flows

### User Authentication Flow
```
User → Login → Supabase Auth → JWT Token → App
                ↓
        Check organization_id
                ↓
        Load organization data
                ↓
        Render dashboard
```

### [Feature] Data Flow
[Diagram specific feature flows]

## API Design

### Public Routes (No Auth)
- `POST /api/auth/signup` - Create account
- `POST /api/auth/login` - Authenticate
- `POST /api/webhooks/stripe` - Stripe webhooks

### Protected Routes (Auth Required)
- `GET /api/users` - List organization users
- `POST /api/users/invite` - Invite user
- `GET /api/[resource]` - List resources
- `POST /api/[resource]` - Create resource
- `PUT /api/[resource]/[id]` - Update resource
- `DELETE /api/[resource]/[id]` - Delete resource

[List all API routes needed]

## Security Considerations
- All queries use organization_id filter
- RLS policies on every table
- API routes verify authentication
- Rate limiting on public endpoints
- Input validation on all forms
- SQL injection prevention (parameterized queries)
- XSS prevention (sanitize user input)
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

CREATE POLICY "Admins can manage users" 
ON users FOR ALL 
USING (
  organization_id = (
    SELECT organization_id FROM users WHERE id = auth.uid()
  ) AND (
    SELECT role FROM users WHERE id = auth.uid()
  ) IN ('owner', 'admin')
);
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
CREATE INDEX idx_system_logs_status ON system_logs(status);

ALTER TABLE system_logs ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Org isolation" ON system_logs
  FOR ALL USING (organization_id = (
    SELECT organization_id FROM users WHERE id = auth.uid()
  ));
```

## Domain-Specific Tables

### [Table 1 Name]
```sql
CREATE TABLE [table_name] (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  organization_id UUID REFERENCES organizations(id) NOT NULL,
  
  -- Table-specific fields
  [field_name] [data_type] [constraints],
  
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_[table]_organization ON [table_name](organization_id);
CREATE INDEX idx_[table]_[field] ON [table_name]([field]);

-- RLS
ALTER TABLE [table_name] ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Org isolation" ON [table_name]
  FOR ALL USING (organization_id = (
    SELECT organization_id FROM users WHERE id = auth.uid()
  ));
```

[Repeat for each domain table needed]

## Relationships

```
organizations
    ↓ (1:many)
users, [domain_tables]
    ↓ (1:many)
[related_tables]
```

## Sample Data

### Organizations
```json
{
  "id": "uuid",
  "name": "Acme Fund",
  "slug": "acme-fund",
  "stripe_customer_id": "cus_xxx",
  "subscription_status": "active",
  "subscription_plan": "premium"
}
```

### Users
```json
{
  "id": "uuid",
  "organization_id": "uuid",
  "email": "john@acme.com",
  "name": "John Smith",
  "role": "owner"
}
```

[Include sample data for domain tables]

## Performance Considerations

### Indexes
All foreign keys indexed
All frequently queried fields indexed
Composite indexes for common query patterns

### Query Optimization
Use joins instead of multiple queries (avoid N+1)
Limit result sets with pagination
Use database-level filtering (not application)

---

[GENERATE COMPLETE SQL FOR ALL TABLES NEEDED]
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

#### Product 1: [Subscription Name]
- **Name:** [Product Name]
- **Price:** $[X]/month
- **Billing:** Monthly recurring
- **Trial:** 14 days
- **Features:** [List features]

[Repeat for each tier]

### Webhook Events
Configure webhook at: `https://[app-url]/api/webhooks/stripe`

**Required Events:**
- `checkout.session.completed` - Create organization after payment
- `customer.subscription.updated` - Update subscription status
- `customer.subscription.deleted` - Handle cancellation
- `invoice.payment_succeeded` - Confirm payment
- `invoice.payment_failed` - Handle failure

### API Keys
```bash
STRIPE_SECRET_KEY=sk_live_xxx (production) / sk_test_xxx (development)
STRIPE_PUBLISHABLE_KEY=pk_live_xxx / pk_test_xxx
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
**Credentials:**
```bash
[INTEGRATION]_API_KEY=xxx
[INTEGRATION]_SECRET=xxx
```

### API Endpoints Used
- `GET /api/endpoint1` - [Purpose]
- `POST /api/endpoint2` - [Purpose]

### Rate Limits
- [X] requests per [timeframe]
- [How to handle rate limiting]

### Error Handling
- **Rate Limit:** Wait and retry with exponential backoff
- **Auth Error:** Check API key, refresh token if needed
- **Server Error:** Log error, retry up to 3 times

### Cost
- **Tier:** [Free / Paid tier]
- **Pricing:** $[X]/month or [X] requests for $[Y]

---

[Repeat for each integration from transcript]

## Environment Variables Summary

### Required in All Environments
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

# [Other integrations]
[INTEGRATION]_API_KEY=

# App Config
NEXT_PUBLIC_APP_URL=
```

### Development vs Production
- Stripe: Use test keys in dev, live keys in prod
- URLs: localhost:3000 in dev, vercel.app in prod
- Most other APIs: Same keys work in both

## Testing Integrations Locally

### Stripe
```bash
# Install Stripe CLI
brew install stripe/stripe-cli/stripe

# Login
stripe login

# Forward webhooks to local
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

### [Other Integrations]
[Testing instructions]
```

---

## FILE 5: UI_SPECIFICATIONS.md

```markdown
# UI Specifications

## Design System
**Follow:** AI_WEST_DESIGN_SYSTEM.md (included in project files)

**Key Colors:**
- Primary: #DC6B4A (Terracotta)
- Background: #F5F3EF (Off-white)
- Text: #1a1a1a (Near-black)
- Secondary: Warm grays

**Typography:**
- Font: IBM Plex Sans
- Headings: 600 weight
- Body: 400 weight

## Component Library
**Using:** shadcn/ui components

**Required Components:**
- Button
- Card
- Form (Input, Label, Select, Textarea)
- Dialog/Modal
- Dropdown Menu
- Table
- Tabs
- Toast notifications

## Page Structure

### Public Pages

#### Landing Page (`/`)
**Purpose:** Marketing homepage for productized version
**Sections:**
- Hero with value proposition
- Key features (3-4)
- Pricing
- CTA to signup

#### Login (`/login`)
**Elements:**
- Email input
- Password input
- "Forgot password" link
- "Sign up" link
- Social auth buttons (if applicable)

#### Signup (`/signup`)
**Flow:**
1. Email + password form
2. Redirect to Stripe Checkout (starts trial)
3. Webhook creates organization
4. Redirect to dashboard

### Protected Pages (Auth Required)

#### Dashboard (`/app/dashboard`)
**Purpose:** Main interface after login
**Sections:**
- Welcome header with user name
- Key metrics (4-6 cards)
- Recent activity table
- Quick actions

**Layout:**
```
┌─────────────────────────────────┐
│ Header (nav + user menu)        │
├─────────────────────────────────┤
│ Welcome, [Name]!                │
├─────────┬─────────┬─────────┬──┤
│Metric 1│Metric 2│Metric 3│M│
├─────────────────────────────────┤
│ Recent Activity Table           │
│                                 │
│                                 │
└─────────────────────────────────┘
```

#### [Feature Page 1] (`/app/[feature]`)
[Describe layout and components]

#### Settings (`/app/settings`)
**Tabs:**
- Profile (name, email, avatar)
- Organization (name, plan)
- Team (user management, invites)
- Billing (subscription, payment method)
- API Keys (if applicable)

#### Debug Console (`/app/debug`) [Developer role only]
**Features:**
- System logs table
- Filters (date, severity, action type)
- Search
- Export logs

## User Flows

### New User Onboarding
```
1. Land on signup page
2. Enter email + password
3. Redirect to Stripe Checkout
4. Enter payment info (starts trial)
5. Stripe processes → Webhook creates org
6. Redirect to dashboard
7. See onboarding checklist:
   [ ] Complete profile
   [ ] Invite team member
   [ ] [Feature-specific onboarding step]
```

### [Key Feature Flow]
```
[Describe step-by-step user flow]
```

## Responsive Design
- **Mobile:** < 768px (stack layout)
- **Tablet:** 768px - 1024px (adapted layout)
- **Desktop:** > 1024px (full layout)

**Priority:** Desktop-first (target users primarily on desktop)
**Mobile:** Functional but can be simplified

## Loading States
- Skeleton loaders for initial data fetch
- Spinners for button actions
- Progress bars for long operations
- Optimistic UI updates where appropriate

## Error States
- Inline form errors (red text below field)
- Toast notifications for API errors
- Full-page error boundary for catastrophic failures
- Empty states for zero data

## Accessibility
- Semantic HTML
- ARIA labels where needed
- Keyboard navigation support
- Focus indicators
- Alt text on images
```

---

## FILE 6: BUILD_PHASES.md

[Continue with remaining files...]

```markdown
# Build Phases

## Phase 1: Foundation
**Duration:** 2-3 hours
**Goal:** Working Next.js app with auth and database

### Tasks
1. [ ] Initialize Next.js project with TypeScript
2. [ ] Install dependencies (Supabase, shadcn, Stripe)
3. [ ] Set up Supabase connection
4. [ ] Create complete database schema
5. [ ] Implement authentication flow
6. [ ] Create base layout with AI West styling
7. [ ] Configure environment variables
8. [ ] Test: App runs at localhost:3000 without errors

### Success Criteria
- `npm run dev` runs without errors
- Can access localhost:3000
- Database tables created in Supabase
- Can create account and log in
- Dashboard loads after login
- No TypeScript compilation errors
- No console errors in browser

---

## Phase 2: Core UI
**Duration:** 3-4 hours
**Goal:** All user-facing pages built and connected to real data

### Tasks
9. [ ] Build dashboard with metrics
10. [ ] Create [feature] pages
11. [ ] Implement settings pages (profile, org, team, billing)
12. [ ] Build user management interface
13. [ ] Create invite system with email
14. [ ] Add loading and error states
15. [ ] Test all pages load correctly

### Success Criteria
- All pages render without errors
- Data displays from Supabase
- Settings save and persist
- Can invite users via email
- Loading states show during data fetch
- Error messages display appropriately
- Mobile-responsive (basic)

---

## Phase 3: Automation
**Duration:** 4-6 hours  
**Goal:** External integrations working and business logic implemented

### Tasks
16. [ ] Integrate [External API 1]
17. [ ] Integrate [External API 2]
18. [ ] Build [core feature] functionality
19. [ ] Implement background jobs (if needed)
20. [ ] Set up webhook handlers
21. [ ] Create system logging throughout app
22. [ ] Test all integrations end-to-end

### Success Criteria
- All external APIs connect successfully
- [Feature] works end-to-end
- Webhooks receive and process events
- Background jobs execute (if applicable)
- System logs capture all actions
- Error handling works for API failures

---

## Phase 4: Deploy & Handoff
**Duration:** 1-2 hours
**Goal:** Production deployment and client handoff

### Tasks
23. [ ] Push code to GitHub
24. [ ] Deploy to Vercel
25. [ ] Configure production environment variables
26. [ ] Update OAuth redirect URLs
27. [ ] Set up production Stripe webhook
28. [ ] Test production app end-to-end
29. [ ] Pre-seed client as Owner
30. [ ] Pre-seed Josh as Developer
31. [ ] Send handoff email with credentials
32. [ ] Schedule walkthrough call

### Success Criteria
- Production app accessible at vercel.app URL
- All features work in production
- Stripe payments process successfully
- Webhooks fire correctly
- Client can log in as Owner
- Josh can log in as Developer
- No errors in production logs

---

## Phase 5: Productization (Optional)
**Duration:** 30-60 minutes
**Goal:** SaaS version ready to launch

### Tasks
33. [ ] Create product landing page
34. [ ] Configure self-serve Stripe checkout
35. [ ] Test signup → payment → dashboard flow
36. [ ] Submit to Product Hunt
37. [ ] Submit to BetaList
38. [ ] Post on Reddit (r/SaaS, r/microsaas)
39. [ ] Post on Indie Hackers
40. [ ] Optional: Set up $20/day Meta ads

### Success Criteria
- Landing page live and styled correctly
- Self-serve signup works end-to-end
- New customers auto-create organizations
- Listed on launch channels
- Optional paid ads running
```

---

## FILES 7-12: Quick Templates

### FILE 7: DEBUGGING_GUIDE.md
```markdown
# Non-technical troubleshooting
# System logs access instructions
# Common error states and fixes
```

### FILE 8: CLIENT_REQUIREMENTS.md
```markdown
# ICP definition from transcript
# Custom branding requirements
# Specific business rules
# Pre-built templates needed
```

### FILE 9: PRODUCTIZATION_GUIDE.md
```markdown
# Market opportunity (who else needs this)
# Recommended SaaS pricing tiers
# Landing page copy structure
# Launch channel strategy
```

### FILE 10: DEPLOYMENT_CHECKLIST.md
```markdown
# Pre-deployment verification checklist
# Step-by-step Vercel deployment
# Environment variables table with sources
# OAuth redirect configuration steps
# Stripe webhook setup steps
# Production testing checklist
# Post-deployment verification
```

### FILE 11: AI_WEST_DESIGN_SYSTEM.md
```markdown
[COPY MASTER FILE EXACTLY - NEVER MODIFY]
```

### FILE 12: PROJECT_INSTRUCTIONS.md
```markdown
# How Project Claude should use these files
# Communication protocol with Claude Code
# Review standards (visual + codebase audit)
# Quality criteria for each phase
```

---

## Remember

**Make every file:**
- Comprehensive (no gaps)
- Actionable (clear next steps)
- Specific (no vague descriptions)
- Complete (includes all SQL, all configurations)

**Project Claude will rely on these files to:**
- Set up local structure
- Create Supabase database
- Guide Claude Code
- Test systematically
- Deploy to production

**Your files = Their blueprint. Make them perfect.**
