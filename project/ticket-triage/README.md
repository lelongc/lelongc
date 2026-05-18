# AI Support Ticket Triage System

> Classify customer support emails instantly — priority, category, and summary in one click.

**Live Demo:** [ticket-triage.vercel.app](#)

## Who Is This For?

Tech startups and small support teams who are overwhelmed by customer emails and need an AI-powered tool to automatically classify, prioritize, and summarize incoming support requests before a human even reads them.

## What It Does

1. A support agent pastes a customer email or message into the system
2. Click "Analyze with AI"
3. The AI instantly returns:
   - **Priority:** URGENT or NORMAL (color-coded badge)
   - **Category:** Technical Bug, Billing, Refund Request, Feature Request, or General Inquiry
   - **Summary:** One sentence describing what the customer wants
   - **Suggested Reply:** A professional opening line for the response
4. Save the ticket to track its status (Open → In Progress → Resolved)
5. Browse, filter, and manage all tickets in a dashboard view

## Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Frontend | Next.js 14 (App Router) | Fast, serverless, great DX |
| AI Engine | Google Gemini API | Free tier, accurate classification |
| Database | Supabase (PostgreSQL) | Free tier, real-time, strong typing |
| Hosting | Vercel | Zero-config deployment |

## Features

- AI-powered ticket classification (priority + category + summary)
- Suggested reply generation for faster response times
- Persistent ticket storage with status tracking
- Filter by priority (Urgent/Normal) and category
- Status workflow: Open → In Progress → Resolved
- Fully responsive dark mode UI
- Real-time ticket count and stats

## Getting Started

### Prerequisites
- Node.js 18+
- Supabase account (free tier)
- Google Gemini API key

### Setup
```bash
# Clone and install
git clone https://github.com/lelongc/ticket-triage.git
cd ticket-triage
npm install

# Environment variables
cp .env.example .env.local
# Fill in your keys:
#   NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
#   NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJ...
#   GEMINI_API_KEY=...

# Run locally
npm run dev
```

### Supabase Setup
Run this SQL in your Supabase SQL Editor:
```sql
CREATE TABLE tickets (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  original_message TEXT NOT NULL,
  priority TEXT CHECK (priority IN ('URGENT', 'NORMAL')),
  category TEXT,
  summary TEXT,
  suggested_reply TEXT,
  status TEXT DEFAULT 'open' CHECK (status IN ('open', 'in_progress', 'resolved')),
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Environment Variables
| Variable | Description |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anonymous/public key |
| `GEMINI_API_KEY` | Google Gemini API key |

## Project Structure
```
ticket-triage/
├── app/
│   ├── page.tsx                # Main page — paste email + analyze
│   ├── tickets/page.tsx        # All tickets list with filters
│   ├── layout.tsx              # Root layout
│   └── api/
│       ├── triage/route.ts     # POST — calls Gemini, returns analysis
│       └── tickets/route.ts    # GET/POST/PATCH — Supabase CRUD
├── components/
│   ├── TicketInput.tsx         # Textarea + analyze button
│   ├── AnalysisResult.tsx      # Priority + Category + Summary display
│   ├── TicketList.tsx          # Filterable data table
│   └── StatusBadge.tsx         # Color-coded status/priority badges
├── lib/
│   ├── supabase.ts             # Supabase client
│   └── gemini.ts               # Gemini API wrapper
└── public/
```

## License
MIT
