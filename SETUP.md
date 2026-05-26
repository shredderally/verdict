# VERDICT SaaS Setup Guide

## Updated Pricing (May 2026)

### Plans
- **Free**: $0/month - 5 free tools, 3 runs per tool per day
- **Pro**: $89/month - All 14 tools, unlimited runs, priority processing
- **Ultra**: $299/month - Everything in Pro + API access, dedicated support, team collaboration

### Annual Billing
- **Pro Annual**: $642.24/year (40% off) - Save $267.76
- **Ultra Annual**: $2,149.20/year (40% off) - Save $1,070.80

## Authentication Setup

### 1. Set Up Supabase
1. Go to [supabase.com](https://supabase.com)
2. Create a new project
3. Get your API URL and Anon Key from Settings > API
4. Copy `.env.local.example` to `.env.local`
5. Add your Supabase credentials:
   ```
   NEXT_PUBLIC_SUPABASE_URL=your_url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your_key
   ```

### 2. Supabase Database Setup
Run these SQL queries in your Supabase dashboard:

```sql
-- Create user_subscriptions table
CREATE TABLE user_subscriptions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  plan TEXT NOT NULL DEFAULT 'free',
  billing_cycle TEXT DEFAULT 'monthly',
  status TEXT DEFAULT 'active',
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Create usage_logs table
CREATE TABLE usage_logs (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  tool_id TEXT NOT NULL,
  runs_count INT DEFAULT 1,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 3. Authentication Pages
The following auth pages are now available:
- `/auth/signup` - Sign up page
- `/auth/signin` - Sign in page
- `/auth/forgot-password` - Password reset
- `/auth/confirm-email` - Email confirmation

### 4. Pricing Page
Access pricing at `/pricing` to see all plans with monthly/annual toggle.

## Features Updated

### Free Tier
- 5 free tools
- 3 runs per tool per day
- Basic analysis
- No credit card required

### Pro Tier
- All 14 tools unlocked
- Unlimited runs
- Deep competitor analysis
- Priority AI processing
- Analysis history
- Export results
- Early access to new tools

### Ultra Tier
- All Pro features +
- API access
- Dedicated support
- Custom analysis requests
- Team collaboration (up to 5 users)
- Advanced reporting
- Webhook integrations

## Deployment to Vercel

1. Push to GitHub:
   ```bash
   git add .
   git commit -m "Update auth and pricing system"
   git push origin main
   ```

2. Go to [vercel.com](https://vercel.com)
3. Connect your GitHub repo
4. Add environment variables in Vercel Settings
5. Deploy!

## File Structure

```
verdict/
├── app/
│   ├── auth/
│   │   ├── signin/page.tsx
│   │   ├── signup/page.tsx
│   │   └── forgot-password/page.tsx
│   └── pricing/page.tsx
├── components/
│   ├── auth/
│   │   ├── SignUp.tsx
│   │   └── SignIn.tsx
│   └── pricing/
│       └── PricingCards.tsx
├── lib/
│   ├── auth.ts
│   └── pricing.ts
└── .env.local.example
```

## Support

If you have issues:
1. Check Supabase dashboard for database errors
2. Check browser console for client-side errors
3. Check Vercel logs for server errors
