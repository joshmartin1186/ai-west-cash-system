# Deployment Checklist

## Overview

With the new parallel Claude architecture, deployment is simpler because:
- Claude Code has been pushing to GitHub continuously
- All code is already on GitHub (source of truth)
- Vercel connects directly to GitHub

---

## Pre-Deployment Verification

### Check GitHub Repository

Claude Project should verify:

```javascript
// Check recent commits
github:list_commits({
  owner: "joshmartin1186",
  repo: "{project-name}",
  perPage: 10
})

// Verify all phases completed
// Commits should show Phase 1-3 tasks complete
```

### Check for Open Issues

```javascript
github:list_issues({
  owner: "joshmartin1186",
  repo: "{project-name}",
  state: "OPEN"
})

// Should be zero open issues
// If issues exist, they need to be resolved first
```

### Verify Code Structure

```javascript
github:get_file_contents({
  owner: "joshmartin1186",
  repo: "{project-name}",
  path: "code/"
})

// Should see:
// - app/
// - lib/
// - components/
// - package.json
// - etc.
```

---

## Vercel Deployment Steps

### Step 1: Connect Vercel to GitHub

1. Go to [vercel.com/new](https://vercel.com/new)
2. Click "Import Git Repository"
3. Select: `joshmartin1186/{project-name}`
4. **Root Directory:** `code` (important!)
5. **Framework Preset:** Next.js (auto-detected)

### Step 2: Configure Environment Variables

Add these in Vercel dashboard → Settings → Environment Variables:

```
# Supabase
NEXT_PUBLIC_SUPABASE_URL = [from Supabase dashboard → Settings → API]
NEXT_PUBLIC_SUPABASE_ANON_KEY = [from Supabase dashboard → Settings → API]
SUPABASE_SERVICE_ROLE_KEY = [from Supabase dashboard → Settings → API]

# Stripe (Production)
STRIPE_SECRET_KEY = sk_live_xxx [from Stripe dashboard]
STRIPE_PUBLISHABLE_KEY = pk_live_xxx [from Stripe dashboard]
STRIPE_WEBHOOK_SECRET = whsec_xxx [after webhook setup]

# AI (if applicable)
GEMINI_API_KEY = [from Google AI Studio]

# App Config
NEXT_PUBLIC_APP_URL = https://{project-name}.vercel.app
```

### Step 3: Deploy

1. Click "Deploy"
2. Wait for build to complete (2-3 minutes)
3. Note the production URL: `https://{project-name}.vercel.app`

### Step 4: Post-Deploy Configuration

#### Update Supabase Authentication URLs

1. Go to Supabase Dashboard → Authentication → URL Configuration
2. **Site URL:** `https://{project-name}.vercel.app`
3. **Redirect URLs:** Add:
   - `https://{project-name}.vercel.app/auth/callback`
   - `https://{project-name}.vercel.app/api/auth/callback`

#### Configure Stripe Production Webhook

1. Go to Stripe Dashboard → Developers → Webhooks
2. Click "Add endpoint"
3. **Endpoint URL:** `https://{project-name}.vercel.app/api/webhooks/stripe`
4. **Events to send:**
   - `checkout.session.completed`
   - `customer.subscription.created`
   - `customer.subscription.updated`
   - `customer.subscription.deleted`
   - `invoice.payment_succeeded`
   - `invoice.payment_failed`
5. Copy the webhook signing secret
6. Add to Vercel env vars as `STRIPE_WEBHOOK_SECRET`
7. **Redeploy** (Vercel → Deployments → Redeploy)

#### Update App URL

1. Verify `NEXT_PUBLIC_APP_URL` is set to production URL
2. If not, update and redeploy

---

## Post-Deployment Testing

### Critical Path Testing

Claude Project guides Josh through testing:

```
## Production Testing Checklist

### Homepage
- [ ] Visit https://{project-name}.vercel.app
- [ ] Page loads without errors
- [ ] Styling looks correct (AI West design)

### Authentication
- [ ] Click Sign Up
- [ ] Enter email and password
- [ ] Click Submit → Redirects to Stripe

### Stripe Checkout
- [ ] Use test card: 4242 4242 4242 4242
- [ ] Complete checkout
- [ ] Redirects back to app
- [ ] Dashboard loads

### Dashboard
- [ ] Data displays correctly
- [ ] No console errors (F12 → Console)
- [ ] Navigation works

### Settings
- [ ] Profile saves correctly
- [ ] Organization info displays
- [ ] Billing shows subscription

### [Feature-Specific Tests]
- [ ] [Test 1]
- [ ] [Test 2]
```

### Check Logs for Errors

1. Vercel Dashboard → Project → Logs
2. Filter by "Error" level
3. Should be no errors

---

## Client Handoff

### Pre-seed Client Account

1. Create user in Supabase Auth:
   - Email: [client email]
   - Send magic link

2. Create user record with Owner role:
```sql
INSERT INTO users (id, organization_id, email, name, role)
VALUES (
  '[auth-user-id]',
  '[organization-id]',
  '[client-email]',
  '[client-name]',
  'owner'
);
```

### Pre-seed Josh as Developer

```sql
INSERT INTO users (id, organization_id, email, name, role)
VALUES (
  '[josh-auth-id]',
  '[organization-id]',
  'josh@aiwest.co',
  'Josh Martin',
  'developer'
);
```

### Send Handoff Email

Template:
```
Subject: Your [Project Name] System is Live! 🚀

Hi [Client Name],

Great news - your system is ready!

**Access Your Dashboard:**
https://{project-name}.vercel.app

I've set up your account. Click "Sign In" and use:
- Email: [client email]
- Click "Magic Link" to receive login email

**What's Included:**
- [Feature 1]
- [Feature 2]
- [Feature 3]

**Quick Start:**
1. Log in with magic link
2. [First action to take]
3. [Second action to take]

**Your Subscription:**
- Plan: [Plan name]
- Trial: 14 days free
- Monthly: $[X]/month after trial

**Support:**
I'm here to help! Reply to this email or schedule a call:
https://calendar.app.google/oJ584xcgU3rD4akH6

Let's schedule a 30-minute walkthrough this week?

Best,
Josh
AI West
```

### Schedule Walkthrough Call

Booking link: `calendar.app.google/oJ584xcgU3rD4akH6`

---

## Troubleshooting

### Build Failed

**Check:** Vercel Deployments → Click on failed deployment → View build logs

**Common causes:**
- TypeScript errors (fix in code, push, auto-redeploys)
- Missing dependencies (check package.json)
- Environment variables not set

### Authentication Not Working

**Check:** Supabase URL Configuration matches production URL

**Fix:**
1. Add production URL to Supabase Redirect URLs
2. Ensure `NEXT_PUBLIC_APP_URL` is correct in Vercel

### Stripe Webhook Not Firing

**Check:** Stripe Dashboard → Webhooks → View events

**Fix:**
1. Verify endpoint URL is correct
2. Check webhook secret matches Vercel env var
3. Ensure events are selected

### Data Not Showing

**Check:** Supabase → Database → Tables → View data

**Possible causes:**
- RLS policies blocking access
- User not in correct organization
- Environment variables incorrect

---

## Deployment Complete

When everything is verified:

```
Deployment Complete! ✅

**Production URL:** https://{project-name}.vercel.app

**Status:**
- [ ] All tests passing
- [ ] Client account pre-seeded
- [ ] Josh access configured
- [ ] Handoff email sent
- [ ] Walkthrough scheduled

**Monitoring:**
- Vercel: https://vercel.com/{username}/{project-name}
- Supabase: https://app.supabase.com/project/{project-id}
- Stripe: https://dashboard.stripe.com

**Client Info:**
- Name: [Client name]
- Email: [Client email]
- Plan: [Plan name]
- Monthly: $[X]/month

Project handoff complete! 🎉
```