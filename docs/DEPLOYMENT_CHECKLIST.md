# DEPLOYMENT_CHECKLIST.md - Template

> **Copy this template into every Claude Project. Fill in project-specific details during build.**

---

# {Project Name} - Deployment Checklist

> **Use this checklist when deploying from local development to Vercel production**

---

## Pre-Deployment Verification (Local)

Before pushing to GitHub, verify these work locally:

- [ ] App runs without errors at `npm run dev`
- [ ] All pages load and display correctly
- [ ] Authentication flow works (signup, login, logout)
- [ ] Database queries return correct data
- [ ] API routes respond correctly
- [ ] External integrations work (if applicable)
- [ ] No console errors in browser
- [ ] No TypeScript errors in terminal

---

## Step 1: Push to GitHub (First Time Only)

```bash
# Initialize git (if not already done)
git init
git add .
git commit -m "Initial working application"

# Create GitHub repo and push
git remote add origin https://github.com/username/{project-name}.git
git push -u origin main
```

---

## Step 2: Connect Vercel

1. Go to [vercel.com/new](https://vercel.com/new)
2. Import from GitHub → Select {project-name}
3. **DO NOT deploy yet** - need to set environment variables first
4. Framework Preset: Next.js (auto-detected)
5. Root Directory: `./` (default)

---

## Step 3: Set Environment Variables in Vercel

Go to Project Settings → Environment Variables and add:

### Supabase (Required)
| Variable | Value | Where to Get It |
|----------|-------|-----------------|
| `NEXT_PUBLIC_SUPABASE_URL` | `https://xxxxx.supabase.co` | Supabase Dashboard → Settings → API |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | `eyJhbGc...` | Supabase Dashboard → Settings → API |
| `SUPABASE_SERVICE_ROLE_KEY` | `eyJhbGc...` | Supabase Dashboard → Settings → API (keep secret!) |

### Stripe (LIVE MODE - Required)
| Variable | Value | Where to Get It |
|----------|-------|-----------------|
| `STRIPE_SECRET_KEY` | `sk_live_...` | Stripe Dashboard → Developers → API Keys (LIVE MODE) |
| `STRIPE_PUBLISHABLE_KEY` | `pk_live_...` | Stripe Dashboard → Developers → API Keys (LIVE MODE) |
| `STRIPE_WEBHOOK_SECRET` | *Set after Step 6* | Create webhook in production first |

### External APIs (Project-Specific)
| Variable | Value | Where to Get It |
|----------|-------|-----------------|
| `GEMINI_API_KEY` | `AIza...` | Google AI Studio → Get API Key |
| `UNIPILE_API_KEY` | `pk_...` | Unipile Dashboard → API Keys (if using LinkedIn) |
| `PHANTOMBUSTER_API_KEY` | `...` | PhantomBuster Dashboard (if using) |
| {Add others} | | |

### App Config
| Variable | Value | Notes |
|----------|-------|-------|
| `NEXT_PUBLIC_APP_URL` | *Set after first deploy* | Will be `https://yourapp.vercel.app` |

---

## Step 4: Initial Deploy to Vercel

1. Click "Deploy" button in Vercel
2. Wait for build to complete (usually 2-3 minutes)
3. Note your production URL: `https://yourapp-xxxxx.vercel.app`
4. **DO NOT test yet** - need to complete remaining steps

---

## Step 5: Update OAuth Redirect URLs

### Supabase Auth (Required)
1. Go to Supabase Dashboard → Authentication → URL Configuration
2. Add to **Site URL**: `https://yourapp.vercel.app`
3. Add to **Redirect URLs**:
   - `https://yourapp.vercel.app/auth/callback`
   - `https://yourapp.vercel.app/**` (wildcard for all routes)
4. Save changes

### Google OAuth (If Using)
1. Go to Google Cloud Console → APIs & Services → Credentials
2. Edit OAuth 2.0 Client ID
3. Add **Authorized redirect URIs**:
   - `https://xxxxx.supabase.co/auth/v1/callback`
   - `https://yourapp.vercel.app/auth/callback`
4. Save

### {Other OAuth Providers - Add as Needed}

---

## Step 6: Set Up Stripe Webhook (Production)

1. Go to Stripe Dashboard → Developers → Webhooks
2. Click "Add endpoint"
3. Endpoint URL: `https://yourapp.vercel.app/api/webhooks/stripe`
4. Select events to listen for:
   - `checkout.session.completed` (Required)
   - `customer.subscription.updated` (Required)
   - `customer.subscription.deleted` (Required)
   - `invoice.payment_succeeded`
   - `invoice.payment_failed`
5. Add endpoint
6. Copy **Signing secret** (starts with `whsec_`)
7. Go to Vercel → Project Settings → Environment Variables
8. Add `STRIPE_WEBHOOK_SECRET` = `whsec_...`
9. Redeploy app (Vercel → Deployments → Redeploy)

---

## Step 7: Update NEXT_PUBLIC_APP_URL

1. Go to Vercel → Project Settings → Environment Variables
2. Find or add `NEXT_PUBLIC_APP_URL`
3. Update value to: `https://yourapp.vercel.app` (your actual Vercel URL)
4. Redeploy app

---

## Step 8: Test Production App End-to-End

Run through this complete checklist on the live site:

### Basic Functionality
- [ ] Homepage loads correctly
- [ ] All navigation links work
- [ ] Images and assets load
- [ ] Styling appears correct (check AI West design)

### Authentication Flow
- [ ] Sign up form works
- [ ] Email verification (if enabled)
- [ ] Login form works
- [ ] Dashboard appears after login
- [ ] User data is scoped to their organization
- [ ] Logout works

### Core Features
- [ ] {List project-specific features to test}
- [ ] {Feature 2}
- [ ] {Feature 3}

### API Routes
- [ ] API routes respond correctly (check browser Network tab)
- [ ] Database queries return data
- [ ] Error handling works (test with invalid input)

### Stripe Integration
- [ ] Checkout button works
- [ ] Stripe Checkout page loads
- [ ] Test payment succeeds (use card: 4242 4242 4242 4242)
- [ ] Webhook receives event (check Stripe Dashboard → Webhooks → Events)
- [ ] Organization created in database after payment
- [ ] User redirected to dashboard
- [ ] Subscription status shows "active"

### User Management
- [ ] Owner can invite users
- [ ] Invite email sends
- [ ] Invited user can sign up via link
- [ ] Role system works (owner, admin, developer, viewer)

### Error Checking
- [ ] No errors in Vercel function logs
- [ ] No errors in browser console
- [ ] No errors in Supabase logs
- [ ] System logs table capturing events

---

## Step 9: Set Up Cron Jobs (If Applicable)

If the app has scheduled background jobs:

1. Create `vercel.json` in project root:
```json
{
  "crons": [
    {
      "path": "/api/cron/daily-job",
      "schedule": "0 0 * * *"
    },
    {
      "path": "/api/cron/hourly-sync",
      "schedule": "0 * * * *"
    }
  ]
}
```

2. Cron endpoints should verify requests:
```typescript
// app/api/cron/daily-job/route.ts
export async function GET(request: Request) {
  // Verify request is from Vercel Cron
  const authHeader = request.headers.get('authorization')
  if (authHeader !== `Bearer ${process.env.CRON_SECRET}`) {
    return Response.json({ error: 'Unauthorized' }, { status: 401 })
  }
  
  // Your cron logic here
  return Response.json({ success: true })
}
```

3. Add `CRON_SECRET` environment variable to Vercel
4. Push changes to GitHub
5. Vercel auto-deploys and enables cron
6. Test by visiting cron endpoint manually (with auth header)

---

## Code Changes Required (If Any)

### ✅ No Changes Needed For:
- API routes (work identically local/production)
- Database queries (same Supabase instance)
- Authentication (same Supabase Auth)
- External API calls (same keys, same code)
- File uploads (same Supabase Storage)
- Server components and client components

### ⚠️ Check These Potential Issues:

**1. Hardcoded Localhost URLs**
Search entire codebase for `localhost:3000` and replace with environment variable:

```typescript
// ❌ BAD - Will break in production
const apiUrl = 'http://localhost:3000/api/data'
const redirectUrl = 'http://localhost:3000/dashboard'

// ✅ GOOD - Works everywhere
const apiUrl = `${process.env.NEXT_PUBLIC_APP_URL}/api/data`
const redirectUrl = `${process.env.NEXT_PUBLIC_APP_URL}/dashboard`
```

**2. Absolute URLs in Fetch Calls**
Use relative URLs when calling your own API routes:

```typescript
// ❌ BAD - Hardcoded
fetch('http://localhost:3000/api/users')

// ✅ GOOD - Relative URL works in both environments
fetch('/api/users')

// ✅ ALSO GOOD - With environment variable
fetch(`${process.env.NEXT_PUBLIC_APP_URL}/api/users`)
```

**3. Console.log Statements (Cleanup)**
Remove or reduce excessive debug logging:

```typescript
// ✅ Keep important logs for production debugging
console.error('Payment failed:', error)
console.warn('Rate limit approaching:', remaining)

// ❌ Remove verbose debug logs
// console.log('User clicked button')
// console.log('State:', state)
// console.log('Rendering component...')
```

**4. CORS Settings (If Using External Frontend)**
If you have separate frontend calling the API:

```typescript
// app/api/route.ts
export async function POST(request: Request) {
  // Add CORS headers if needed
  const headers = {
    'Access-Control-Allow-Origin': process.env.NEXT_PUBLIC_APP_URL!,
    'Access-Control-Allow-Methods': 'POST, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type, Authorization',
  }
  
  // Handle preflight
  if (request.method === 'OPTIONS') {
    return new Response(null, { headers })
  }
  
  // Your logic...
  return Response.json({ data }, { headers })
}
```

---

## Project-Specific Notes

### {Feature or Integration Name}
- **Local testing:** {How it was tested locally}
- **Production setup:** {Any specific configuration needed}
- **Known issues:** {Document any quirks or limitations}

### {External API Integration}
- **Rate limits:** {Document limits and how app handles them}
- **Error handling:** {How failures are handled}
- **Fallback behavior:** {What happens if service is down}

### {Database Operations}
- **Migration status:** {Any pending migrations or schema changes}
- **Data seeding:** {How to populate initial data}
- **Backup strategy:** {Supabase has automatic backups, note any special needs}

---

## Troubleshooting Common Issues

### Issue: "Invalid redirect URL" error on login
**Symptoms:** User redirected to error page after OAuth login
**Fix:** 
1. Check Supabase → Authentication → URL Configuration
2. Ensure production URL is in Redirect URLs list with wildcard
3. Verify `NEXT_PUBLIC_APP_URL` matches actual domain

### Issue: Stripe webhook not receiving events
**Symptoms:** Payments succeed but organization not created
**Fix:** 
1. Check webhook URL: `https://yourapp.vercel.app/api/webhooks/stripe`
2. Verify `STRIPE_WEBHOOK_SECRET` is set in Vercel environment variables
3. Check Stripe Dashboard → Webhooks → Your endpoint → Recent events for errors
4. Check Vercel function logs for webhook handler errors
5. Verify events are selected (checkout.session.completed required)

### Issue: Environment variables not working
**Symptoms:** API calls fail, config values are undefined
**Fix:**
1. Verify variables are set in Vercel → Settings → Environment Variables
2. Remember: Variables starting with `NEXT_PUBLIC_` are exposed to browser
3. Other variables are server-side only
4. **Must redeploy after adding/changing variables**
5. Clear browser cache if `NEXT_PUBLIC_` variables not updating

### Issue: API route returns 404 in production
**Symptoms:** API calls work locally but 404 in production
**Fix:**
1. Check file structure: Must be `app/api/[route]/route.ts`
2. Verify exports: `export async function GET()` or `POST()`
3. Check Vercel deployment logs for build errors
4. Ensure no uppercase letters in route folder names

### Issue: Database connection fails
**Symptoms:** All database queries fail with auth errors
**Fix:**
1. Verify Supabase environment variables are correct
2. Check Supabase project is active (not paused)
3. Verify RLS policies allow the operation
4. Check service role key if bypassing RLS

### Issue: Images or assets not loading
**Symptoms:** Images show broken, CSS not applied
**Fix:**
1. Check `next.config.js` has correct image domains
2. Verify assets are in `public/` folder
3. Use `Image` component from `next/image` for optimization
4. Check Vercel deployment logs for build warnings

### Issue: Slow API responses
**Symptoms:** Requests take >3 seconds
**Fix:**
1. Check Supabase query performance (use EXPLAIN ANALYZE)
2. Add database indexes on frequently queried columns
3. Check for N+1 query problems
4. Consider caching for read-heavy operations
5. Review Vercel function logs for timeout warnings

---

## Post-Deployment Checklist

- [ ] All environment variables set in Vercel
- [ ] OAuth redirects updated (Supabase, Google, etc.)
- [ ] Stripe webhook configured and receiving events successfully
- [ ] Production app tested end-to-end (all features work)
- [ ] No errors in Vercel function logs
- [ ] No errors in browser console
- [ ] System logs capturing events correctly
- [ ] Client organization pre-seeded in database
- [ ] Josh (Developer role) pre-seeded in database
- [ ] Custom domain connected (if applicable)
- [ ] Client notified that app is live
- [ ] Magic link sent to client for first login
- [ ] Walkthrough call scheduled

---

## Support & Maintenance

### For Josh (Developer Role):
- **Access:** Login with josh@aiwest.co
- **View Logs:** Vercel → Deployments → View Function Logs
- **Database:** Supabase Dashboard → Table Editor
- **System Logs:** Access via Debug Console in app
- **No Access To:** Billing settings (Owner only)

### For Client (Owner Role):
- **Full Access:** All features including billing
- **User Management:** Settings → Users to invite team
- **Billing:** Settings → Billing for subscription
- **Debug Console:** Available if technical issues arise
- **Support:** Contact josh@aiwest.co for technical help

---

## Performance Monitoring

### Key Metrics to Watch:
- **Vercel Analytics:** Response times, error rates
- **Supabase:** Database query performance
- **Stripe:** Payment success rates, failed charges
- **System Logs:** Error patterns, unusual activity

### Warning Signs:
- API response times >3 seconds consistently
- Database connection errors
- Failed webhook deliveries
- High error rate in logs (>5% of requests)

---

## Deployment Complete! 🎉

**Production URL:** `https://yourapp.vercel.app`

**Next Steps:**
1. Client walkthrough call
2. Monitor for 24-48 hours
3. Phase 5: Productization (if applicable)
4. Document any issues or improvements needed

---

## Deployment History

| Date | Version | Changes | Deployed By |
|------|---------|---------|-------------|
| YYYY-MM-DD | v1.0 | Initial production deployment | Josh |
| | | | |

---

## Notes

{Add any additional notes, quirks, or important context here}
