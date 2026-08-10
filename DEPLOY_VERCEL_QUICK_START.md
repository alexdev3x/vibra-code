# Quick Start: Deploy Website to Vercel

This branch (`claude/website-vercel-deploy-xm2899`) is ready for automatic deployment to Vercel.

## What's Been Done

✅ **Vercel Configuration Created**
- `vercel.json`: Production settings, API timeouts, memory allocation
- `.vercelignore`: Optimized build by excluding unnecessary files
- `next.config.ts`: Fixed for Next.js 15 compatibility

✅ **Comprehensive Deployment Guide**
- Full guide: `vibracode-backend/VERCEL_DEPLOYMENT.md`
- All required environment variables documented
- Webhook setup instructions
- Troubleshooting guide

## Next Steps: Deploy to Vercel

### Option 1: Deploy via Vercel Dashboard (Recommended)

1. **Connect Repository**
   - Go to https://vercel.com/dashboard
   - Click "Add New..." > "Project"
   - Select "Import Git Repository"
   - Find and select `vibra-code` repository

2. **Configure Project**
   - Set "Root Directory" to `vibracode-backend`
   - Set "Production Branch" to `main`
   - Skip build configuration (uses `vercel.json`)

3. **Add Environment Variables**
   - Go to Settings > Environment Variables
   - Add all variables from `vibracode-backend/VERCEL_DEPLOYMENT.md`
   - Required: Anthropic, E2B, Convex, Clerk API keys

4. **Deploy**
   - Click "Deploy"
   - Vercel will automatically build and deploy

### Option 2: Deploy via Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Navigate to backend directory
cd vibracode-backend

# Deploy (interactive setup)
vercel

# For production
vercel --prod
```

### Option 3: Automatic GitHub Integration

Once repository is connected, commits to branches will trigger:
- **Preview deployments** for `claude/website-vercel-deploy-xm2899` and other branches
- **Production deployments** for `main` branch (when ready)

## Required Services Setup

Before deployment, ensure you have accounts for:

1. **Anthropic** (Claude AI)
   - Get API key: https://console.anthropic.com
   - Free credits for testing

2. **E2B** (Code Sandboxes)
   - Get API key: https://e2b.dev
   - Free tier available

3. **Convex** (Database)
   - Get deployment info: https://convex.dev
   - Free tier: 1M transactions/month

4. **Clerk** (Authentication)
   - Get keys: https://clerk.com
   - Free tier available

See `vibracode-backend/VERCEL_DEPLOYMENT.md` for complete details on each service.

## Environment Variable Reference

### Must-Have Variables
```
ANTHROPIC_API_KEY=sk-...
ANTHROPIC_SANDBOX_API_KEY=sk-...
E2B_API_KEY=...
CONVEX_DEPLOYMENT=...
NEXT_PUBLIC_CONVEX_URL=https://....convex.cloud
CLERK_SECRET_KEY=sk_...
CLERK_WEBHOOK_SECRET=whsec_...
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_...
```

### Optional but Recommended
```
STRIPE_SECRET_KEY=sk_...          # For web payments
REVENUECAT_API_KEY=...            # For mobile payments
AUTH_GITHUB_ID=...                # For GitHub OAuth
OPENAI_API_KEY=sk-...             # For image generation
```

## Verification

After deployment:

1. **Check Build**
   - Go to Vercel Dashboard > Deployments
   - Verify latest deployment is "Ready"

2. **Test Application**
   - Visit your deployment URL
   - Check production domain works

3. **Configure Webhooks**
   - Clerk: Add webhook for `https://your-domain.com/api/webhooks/clerk`
   - Stripe (if enabled): Add webhook for payments
   - See `VERCEL_DEPLOYMENT.md` for details

4. **Monitor Performance**
   - Vercel Dashboard > Analytics
   - Check function performance and errors

## Rollback

If needed, rollback to previous deployment:
1. Vercel Dashboard > Deployments
2. Click previous successful deployment
3. Click "Redeploy"

## Need Help?

- **Vercel Docs**: https://vercel.com/docs
- **Next.js Docs**: https://nextjs.org/docs
- **Deployment Guide**: See `vibracode-backend/VERCEL_DEPLOYMENT.md`

## Branch Info

- **Branch**: `claude/website-vercel-deploy-xm2899`
- **Status**: Ready for deployment
- **Target**: Production website deployment to Vercel
- **Automatic**: Configured for auto-deployment on push
