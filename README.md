# EasyNotes

> **SpecKit Demo Project** - A fully functional note-taking app built entirely by AI using natural language specifications.

## About This Project

This is a demonstration of [SpecKit](https://www.speckit.ai) workflow. The entire codebase was generated from natural language descriptions—**zero manual coding by humans**. The goal: prove AI can build production-ready applications from specs alone.

## What It Does

A modern note-taking app with:
- ✨ Rich text editing (Tiptap)
- 🏷️ Tag system with colors
- 📅 Calendar navigation
- 💾 Auto-save (2-second debounce)
- 🎨 Apple-inspired liquid glass UI

## Tech Stack

- **Next.js 15** (App Router, Server Actions)
- **TypeScript** + **Prisma** + **PostgreSQL**
- **Tailwind CSS v4** (glassmorphism design)
- **Tiptap Editor**

## Quick Start

```bash
# Install dependencies
npm install

# Setup PostgreSQL database
createdb easynotes

# Configure .env
echo 'DATABASE_URL="postgresql://user:password@localhost:5432/easynotes"' > .env

# Run migrations
npx prisma migrate dev

# Start dev server
npm run dev
```

Open http://localhost:3000

## The SpecKit Workflow

1. **📝 Specify** - Describe features in natural language
2. **📋 Plan** - AI generates implementation plan
3. **✅ Tasks** - Break down into actionable steps
4. **🚀 Implement** - AI writes all the code
5. **🎨 Polish** - Iterative refinements via conversation

**Result:** Production-ready app without writing code manually.

## License

MIT

---

**Built with:** [SpecKit](https://www.speckit.ai) | **Generated:** 100% by AI
