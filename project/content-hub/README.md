# AI Content Repurposing Hub

> Turn one long article into multiple social media posts in 5 seconds — powered by AI.

**Live Demo:** [content-hub.vercel.app](#)

## Who Is This For?

Marketing Agencies and content teams who need to repurpose blog posts, articles, or any long-form content into platform-specific social media posts quickly and consistently.

## What It Does

1. Paste any long-form text (article, blog post, newsletter)
2. Click "Repurpose with AI"
3. Get back instantly:
   - **1 LinkedIn post** — professional tone, max 1300 characters
   - **3 Twitter/X posts** — casual tone, max 280 characters each
   - **1 Short Video Script** — 60-90 second hook-value-CTA structure
4. Save results to your history for later reference
5. One-click copy for each output

## Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Frontend | Next.js 14 (App Router) | Fast, SEO-friendly, serverless API routes |
| AI Engine | Google Gemini API | Free tier, high-quality text generation |
| Database | MongoDB Atlas | Flexible JSON storage, free 512MB tier |
| Hosting | Vercel | Zero-config deployment, edge network |

## Features

- AI-powered content repurposing (article → LinkedIn + Twitter + Video Script)
- Persistent history — browse and search all past conversions
- One-click copy to clipboard
- Fully responsive dark mode UI
- Fast serverless API routes (no backend server needed)

## Getting Started

### Prerequisites
- Node.js 18+
- MongoDB Atlas account (free tier)
- Google Gemini API key

### Setup
```bash
# Clone and install
git clone https://github.com/lelongc/content-hub.git
cd content-hub
npm install

# Environment variables
cp .env.example .env.local
# Fill in your keys:
#   MONGODB_URI=mongodb+srv://...
#   GEMINI_API_KEY=...

# Run locally
npm run dev
```

### Environment Variables
| Variable | Description |
|---|---|
| `MONGODB_URI` | MongoDB Atlas connection string |
| `GEMINI_API_KEY` | Google Gemini API key |

## Project Structure
```
content-hub/
├── app/
│   ├── page.tsx                # Main page — input + AI output
│   ├── history/page.tsx        # Browse saved repurposed content
│   ├── layout.tsx              # Root layout with dark theme
│   └── api/
│       ├── repurpose/route.ts  # POST — calls Gemini, returns results
│       └── history/route.ts    # GET/POST — MongoDB CRUD
├── components/
│   ├── InputForm.tsx           # Textarea + submit button
│   ├── OutputCard.tsx          # Tabbed display for each format
│   └── HistoryTable.tsx        # Table of past conversions
├── lib/
│   ├── mongodb.ts              # MongoDB client singleton
│   └── gemini.ts               # Gemini API wrapper
└── public/
```

## License
MIT
