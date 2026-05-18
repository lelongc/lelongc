# Startup Waitlist & Secure Admin Dashboard

> A production-ready landing page with email collection and a secure admin dashboard — deploy in one day.

**Live Demo:** [waitlist-demo.vercel.app](#)

## Who Is This For?

Solo Founders and early-stage startups who need a beautiful waitlist page to collect emails before their product launch, plus a secure admin panel to manage and export that list.

## What It Does

### Public-Facing (Landing Page)
1. Visitors land on a stunning, high-converting landing page
2. They enter their email and click "Join the Waitlist"
3. They see a confirmation: "You're on the list!"
4. Their email is securely stored in Supabase

### Admin Dashboard (Protected)
1. Only the founder can log in at `/login`
2. Dashboard shows total signups, recent entries, and a data table
3. One-click "Export CSV" to download the full email list
4. Used for importing into Mailchimp, ConvertKit, or ad platforms

## Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Frontend | Next.js 14 (App Router) | Fast, SEO-optimized landing page |
| Database | Supabase (PostgreSQL) | Free tier, real-time, built-in RLS |
| Authentication | Supabase Auth | Email/password, no third-party needed |
| Security | Cloudflare | DNS, CDN, DDoS protection, SSL |
| Hosting | Vercel | Instant deployment from GitHub |

## Features

- High-converting landing page with email capture form
- Supabase Auth — secure login for admin only
- Row Level Security (RLS) — public can submit, only admin can read
- Admin dashboard with signup stats and data table
- CSV export for email marketing tools
- Cloudflare DNS + CDN integration
- Fully responsive dark mode UI
- Duplicate email prevention

## Getting Started

### Prerequisites
- Node.js 18+
- Supabase account (free tier)
- Cloudflare account (optional, for custom domain)

### Setup
```bash
# Clone and install
git clone https://github.com/lelongc/waitlist-kit.git
cd waitlist-kit
npm install

# Environment variables
cp .env.example .env.local
# Fill in your keys:
#   NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
#   NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJ...
#   SUPABASE_SERVICE_ROLE_KEY=eyJ...

# Run locally
npm run dev
```

### Supabase Setup
Run this SQL in your Supabase SQL Editor:
```sql
CREATE TABLE waitlist (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  source TEXT DEFAULT 'website',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

ALTER TABLE waitlist ENABLE ROW LEVEL SECURITY;

-- Anyone can submit their email
CREATE POLICY "Public insert" ON waitlist FOR INSERT
  WITH CHECK (true);

-- Only logged-in admin can view data
CREATE POLICY "Admin read" ON waitlist FOR SELECT
  USING (auth.role() = 'authenticated');
```

### Environment Variables
| Variable | Description |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anonymous/public key |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase service role key (server-side only) |

## Project Structure
```
waitlist-kit/
├── app/
│   ├── page.tsx                # Public landing page
│   ├── login/page.tsx          # Admin login
│   ├── admin/page.tsx          # Protected admin dashboard
│   ├── layout.tsx              # Root layout
│   └── api/
│       ├── subscribe/route.ts  # POST — add email to waitlist
│       └── export/route.ts     # GET — export CSV (auth required)
├── components/
│   ├── HeroSection.tsx         # Landing page hero
│   ├── EmailForm.tsx           # Email input + submit
│   ├── AdminTable.tsx          # Data table for admin
│   └── StatsCards.tsx          # Total signups, today's count
├── lib/
│   └── supabase.ts             # Supabase client helpers
└── public/
```

## License
MIT
