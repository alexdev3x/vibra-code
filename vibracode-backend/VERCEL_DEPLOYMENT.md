# Vercel Deployment Guide

This guide explains how to deploy VibraCode's backend/website to Vercel.

## Prerequisites

1. A Vercel account (https://vercel.com)
2. Required service accounts and API keys:
   - Anthropic API key (for Claude AI)
   - E2B API key (for code sandboxes)
   - Convex deployment (Convex database)
   - Clerk account (for authentication)

## Deployment Steps

### 1. Connect Repository to Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy the project
cd vibracode-backend
vercel

# For production deployment to a specific domain:
vercel --prod
```

Or connect via Vercel Dashboard:
1. Go to https://vercel.com/dashboard
2. Click "Add New..." > "Project"
3. Select "Import Git Repository"
4. Select the `vibra-code` repository
5. Set the "Root Directory" to `vibracode-backend`

### 2. Set Environment Variables in Vercel

In Vercel Dashboard, go to Settings > Environment Variables and add:

#### Required Variables

**Anthropic (Claude AI)**
- `ANTHROPIC_API_KEY`: Your Anthropic API key
- `ANTHROPIC_SANDBOX_API_KEY`: Anthropic API key for sandboxes (can be same as above)
- `ANTHROPIC_BASE_URL`: `https://api.anthropic.com`
- `AGENT_TYPE`: `claude`

**E2B (Code Sandbox Provider)**
- `E2B_API_KEY`: Your E2B API key from https://e2b.dev

**Convex (Real-time Database)**
- `CONVEX_DEPLOYMENT`: Your Convex deployment ID
- `NEXT_PUBLIC_CONVEX_URL`: Your Convex deployment URL (e.g., `https://xxx.convex.cloud`)

**Clerk (Authentication)**
- `CLERK_SECRET_KEY`: Your Clerk secret key
- `CLERK_WEBHOOK_SECRET`: Clerk webhook secret (from Webhooks section)
- `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY`: Clerk publishable key

#### Optional Variables

**NextAuth (Web OAuth)**
- `NEXTAUTH_URL`: Your production domain (e.g., `https://vibracodeapp.com`)
- `NEXTAUTH_SECRET`: Generated secret (run `openssl rand -base64 32`)
- `AUTH_SECRET`: Same as NEXTAUTH_SECRET
- `AUTH_GITHUB_ID`: GitHub OAuth app ID
- `AUTH_GITHUB_SECRET`: GitHub OAuth app secret

**GitHub Integration**
- `GITHUB_CLIENT_ID`: For "Push to GitHub" feature
- `GITHUB_CLIENT_SECRET`: For "Push to GitHub" feature

**Stripe (Web Payments)**
- `STRIPE_SECRET_KEY`: Stripe secret API key
- `STRIPE_PUBLISHABLE_KEY`: Stripe publishable key
- `STRIPE_WEBHOOK_SECRET`: Webhook signing secret
- `STRIPE_PRICE_ID_WEEKLY_PLUS`: Price ID for weekly+ plan

**RevenueCat (Mobile Payments)**
- `REVENUECAT_API_KEY`: RevenueCat API key
- `REVENUECAT_WEBHOOK_SECRET`: RevenueCat webhook secret
- `REVENUECAT_OAUTH_CLIENT_ID`: OAuth client ID
- `REVENUECAT_OAUTH_CLIENT_SECRET`: OAuth client secret
- `REVENUECAT_OAUTH_REDIRECT_URI`: Redirect URI (e.g., `https://vibracodeapp.com/auth/revenuecat/callback`)

**Other Services**
- `OPENAI_API_KEY`: OpenAI key (optional, for image generation)
- `ELEVENLABS_API_KEY`: ElevenLabs key (optional, for audio generation)
- `FIRECRAWL_API_KEY`: Firecrawl key (optional, for web scraping)
- `CONTEXT7_API_KEY`: Context7 key (optional, for documentation lookup)
- `FEATUREBASE_SECRET_KEY`: Featurebase key (optional, for feature requests)

**Configuration**
- `NODE_ENV`: `production`
- `INNGEST_DEV`: `false`
- `NEXT_PUBLIC_APP_URL`: Your production domain
- `AUTO_PAUSE_TIMEOUT_MS`: `600000` (10 minutes)

### 3. Set Deployment Branch

In Vercel Dashboard Settings > Git:
- Set "Production Branch" to `main` (or your main branch)
- Set "Preview Deployments" to deploy all branches
- The `claude/website-vercel-deploy-xm2899` branch is configured for preview deployments

### 4. Configure Custom Domain

In Vercel Dashboard > Domains:
1. Add your custom domain (e.g., `vibracodeapp.com`)
2. Update DNS records according to Vercel's instructions
3. Enable "Enhanced Protection"

### 5. Deploy

**Automatic Deployment**
Push to your connected branch and Vercel will automatically deploy:
```bash
git push origin main  # Production deployment
git push origin claude/website-vercel-deploy-xm2899  # Preview deployment
```

**Manual Deployment via CLI**
```bash
vercel deploy              # Deploy preview
vercel deploy --prod       # Deploy to production
```

## Post-Deployment Setup

### Clerk Webhook
1. Go to Clerk Dashboard > Webhooks
2. Add endpoint: `https://your-domain.com/api/webhooks/clerk`
3. Select events: `user.created`, `user.updated`, `user.deleted`

### Inngest
1. Inngest functions run on Vercel's serverless platform
2. Manage background jobs in Inngest Dashboard
3. Background functions: `create-session`, `run-agent`, `push-to-github`

### Stripe Webhook (if using Stripe)
1. Go to Stripe Dashboard > Webhooks
2. Add endpoint: `https://your-domain.com/api/webhooks/stripe`
3. Select events: `payment_intent.succeeded`, `payment_intent.failed`, `customer.subscription.updated`

## Monitoring & Logs

**View Logs in Vercel**
1. Dashboard > Project > Deployments > [Latest] > Logs
2. Or use CLI: `vercel logs`

**Monitor Function Performance**
1. Dashboard > Analytics
2. Check Function Duration, Cold Starts, Memory Usage

## Troubleshooting

### Build Failures
1. Check Vercel Build Logs for errors
2. Ensure all required environment variables are set
3. Verify `vercel.json` configuration

### Runtime Errors
1. Check application logs: `vercel logs [--prod]`
2. Verify API key permissions and validity
3. Check Convex deployment status

### Environment Variables Not Applied
1. Environment variables must be set BEFORE deployment
2. Restart deployment after changing variables
3. Use `vercel env pull` to test locally

## Local Testing Before Deployment

```bash
# Build locally to test for errors
npm run build

# Start production server locally
npm start

# Use .env.local with real credentials for testing
cp .env.example .env.local
# Fill in real credentials from your services
```

## Project Structure for Deployment

```
vibracode-backend/
├── app/                    # Next.js App Router
├── components/             # React components
├── convex/                 # Convex database schema
├── lib/
│   ├── inngest/           # Background job functions
│   ├── e2b/               # E2B sandbox configuration
│   ├── prompts.ts         # AI system prompts
│   └── ...
├── public/                # Static files
├── vercel.json           # Vercel configuration
├── next.config.ts        # Next.js configuration
└── package.json
```

## Key Configuration Files

- **vercel.json**: Vercel deployment settings, API route timeouts, memory limits
- **next.config.ts**: Next.js build configuration, body size limits, allowed dev origins
- **.vercelignore**: Files to exclude from deployment
- **package.json**: Dependencies and build scripts

## Cost Considerations

- **Vercel**: Free tier included, Pro plan ($20/month) for production
- **Convex**: Free tier (1M free transactions/month)
- **E2B**: Pay-as-you-go for sandbox usage
- **Anthropic**: Usage-based billing for Claude API
- **Other services**: See respective pricing pages

## Support

For issues with:
- **Deployment**: Check Vercel docs (https://vercel.com/docs)
- **Next.js**: Next.js documentation (https://nextjs.org/docs)
- **Convex**: Convex support (https://convex.dev/support)
- **Clerk**: Clerk documentation (https://clerk.com/docs)
- **Inngest**: Inngest docs (https://www.inngest.com/docs)
