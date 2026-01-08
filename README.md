# Bazai Idea Inbox

AI-powered YouTube video idea capture, ranking, and outline generation system.

## 🎯 Overview

**Bazai Idea Inbox** transforms scattered YouTube ideas into ready-to-record video outlines. It solves the "what should I record today?" friction by:

- **Capturing ideas** from anywhere (web, mobile, browser tab)
- **Scoring & ranking** ideas by searchability, Bazai fit, and complexity
- **Generating outlines** with hook, chapters, demo plan, and CTAs
- **Creating short-form hooks** for YouTube Shorts and clips
- **Tracking status** (shipped, in progress, backlog)

## ✨ Core Features

### v1 MVP
- **Idea Capture**: Quick text input with tags (topic, difficulty, format)
- **AI Scoring**: Ranks ideas by potential (views, uniqueness, effort)
- **One-Click Outlines**: Generates structured video plan with sections
- **Shorts Hooks**: Generate viral hooks for short-form content
- **History & Search**: Full-text search over all ideas and outlines

### Future (v2+)
- Browser extension for quick capture
- Analytics integration to score by historical performance
- Collaboration & team ideas
- Video script generation
- Thumbnail concept suggestions

## 🛠️ Tech Stack

### Frontend
- **Next.js** 14+ (App Router, TypeScript)
- **React** with hooks
- **Tailwind CSS** for styling
- **Shadcn/ui** for components
- **TanStack Query** for data fetching

### Backend
- **Node.js** / **Express** (or Next.js API routes)
- **Supabase** (PostgreSQL + Auth + Real-time)
- **tRPC** for type-safe APIs

### AI Layer
- **OpenAI API** or **Llama API** for idea scoring & outline generation
- **Prompt engineering** for context-aware suggestions

## 📦 Project Structure

```
bazai-idea-inbox/
├── apps/
│   ├── web/                    # Next.js frontend app
│   │   ├── src/
│   │   │   ├── app/           # App Router pages & layouts
│   │   │   ├── components/    # React components
│   │   │   ├── hooks/         # Custom React hooks
│   │   │   ├── lib/           # Utilities, API clients
│   │   │   └── styles/        # Global styles
│   │   ├── public/
│   │   └── next.config.js
│   └── api/                    # Node/Express backend (optional)
│       ├── src/
│       │   ├── routes/        # API endpoints
│       │   ├── middleware/    # Auth, logging, etc.
│       │   ├── db/            # Database layer (Supabase)
│       │   ├── ai/            # LLM integration
│       │   └── config/        # Env, constants
│       └── server.js
├── packages/
│   ├── db/                     # Shared database layer
│   │   ├── schema/            # SQL schemas
│   │   └── migrations/
│   ├── api/                    # Shared API types (tRPC)
│   └── config/                 # Shared config
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
└── README.md
```

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- npm or pnpm
- Supabase account (free tier works)
- OpenAI API key or Llama API key

### Installation

```bash
# Clone repo
git clone https://github.com/yourusername/bazai-idea-inbox
cd bazai-idea-inbox

# Install dependencies
npm install

# Set up environment
cp .env.example .env.local
# Fill in:
# - NEXT_PUBLIC_SUPABASE_URL
# - NEXT_PUBLIC_SUPABASE_KEY
# - OPENAI_API_KEY (or LLAMA_API_KEY)

# Run migrations
npm run db:migrate

# Start development
npm run dev
```

Visit `http://localhost:3000`

## 📋 Database Schema

### Ideas Table
```sql
CREATE TABLE ideas (
  id UUID PRIMARY KEY,
  user_id UUID NOT NULL REFERENCES auth.users,
  title TEXT NOT NULL,
  description TEXT,
  tags TEXT[] (topic, difficulty, format),
  status VARCHAR (shipped | in_progress | backlog),
  ai_score NUMERIC (0-100),
  ai_ranking_reason TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
  INDEX (user_id, status),
  INDEX (created_at DESC)
);
```

### Outlines Table
```sql
CREATE TABLE outlines (
  id UUID PRIMARY KEY,
  idea_id UUID NOT NULL REFERENCES ideas(id),
  hook TEXT NOT NULL,
  chapters JSONB,  -- [{title, duration, notes}]
  demo_plan TEXT,
  cta TEXT,
  shorts_hooks TEXT[],
  created_at TIMESTAMP,
  UNIQUE (idea_id)
);
```

## 🤖 AI Integration

### Idea Scoring Prompt
```
Score this YouTube video idea on potential (0-100) for an audience interested in [topic].
Consider: searchability, Bazai fit, trending relevance, uniqueness, estimated production effort.
```

### Outline Generation Prompt
```
Create a YouTube video outline for: "[idea]"  
Include:
- Hook (first 15 seconds)
- 3-5 key chapters (with duration)
- Demo plan (if applicable)
- Call-to-action
- 2-3 YouTube Shorts hooks from this content
```

## 🔄 User Flow

1. **Capture** → User drops idea + tags
2. **Score** → AI ranks it (background job)
3. **View** → See ranked list of ideas
4. **Generate** → Click "Generate Outline"
5. **Edit** → Refine outline, copy to notes/docs
6. **Record** → Use outline to record
7. **Archive** → Mark as "shipped" after publish

## 📊 Success Metrics

- Ideas captured per week
- Time from idea to recording (should drop)
- Ideas marked "shipped" vs backlog ratio
- Video performance correlated to AI score

## 🧪 Testing

```bash
# Unit tests
npm run test

# E2E tests (Playwright)
npm run test:e2e

# Coverage
npm run test:coverage
```

## 🚢 Deployment

### Frontend (Vercel)
```bash
vercel deploy
```

### Backend (Railway, Render, or self-hosted)
```bash
npm run build
npm start
```

## 🎬 Demo & Content

Plan to create content series:
1. **Launch video**: Building a YouTube idea inbox in [48hrs]
2. **Tutorial**: How to use Bazai Idea Inbox for your channel
3. **Behind-the-scenes**: AI scoring logic & prompt engineering
4. **Growth update**: Monthly stats on ideas → shipped videos

## 🤝 Contributing

Open to contributors! Please:
1. Fork the repo
2. Create a feature branch (`git checkout -b feature/amazing-thing`)
3. Commit changes (`git commit -am 'Add amazing feature'`)
4. Push (`git push origin feature/amazing-thing`)
5. Open a Pull Request

## 📝 License

MIT License - see LICENSE file for details.

## 🙋 Questions?

Reach out via:
- GitHub Issues
- LinkedIn [@bazai](https://linkedin.com/in/bazai)
- YouTube comments on [Bazai channel](https://youtube.com/@bazai)

---

**Built with ❤️ for creators who think faster than they can record.**
