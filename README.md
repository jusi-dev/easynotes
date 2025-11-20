# EasyNotes

A modern, full-stack note-taking application built with Next.js 15, TypeScript, Prisma, and PostgreSQL.

## Features

✨ **Rich Text Editing** - Powered by Tiptap editor with formatting toolbar
📝 **Auto-Save** - Changes saved automatically every 2 seconds
🏷️ **Tag System** - Organize notes with colored tags and multi-tag filtering
📅 **Calendar Navigation** - Find notes by creation date
🎨 **Three-Panel Interface** - Calendar/filters, notes list, and editor
⚡ **Server Actions** - Type-safe mutations with Next.js 15
🔍 **Advanced Filtering** - Filter by tags (OR logic) and dates
💾 **PostgreSQL Storage** - Reliable data persistence with Prisma ORM

## Tech Stack

- **Framework**: Next.js 15 (App Router)
- **Language**: TypeScript 5.3+
- **Database**: PostgreSQL 15+ with Prisma ORM
- **Editor**: Tiptap (ProseMirror-based)
- **Styling**: Tailwind CSS
- **Calendar**: react-day-picker
- **Validation**: Zod

## Prerequisites

- Node.js 18.17+ (v20 recommended)
- PostgreSQL 15+
- npm or yarn

## Quick Start

### 1. Install Dependencies

```bash
npm install
```

### 2. Setup Database

Create a PostgreSQL database:

```sql
CREATE DATABASE easynotes;
```

Configure environment variables in `.env`:

```env
DATABASE_URL="postgresql://username:password@localhost:5432/easynotes?schema=public"
```

### 3. Run Migrations

```bash
# Generate Prisma Client
npx prisma generate

# Run migrations
npx prisma migrate dev --name init
```

### 4. Start Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Project Structure

```
easynotes/
├── actions/              # Server Actions (mutations)
│   ├── notes.ts         # Note CRUD operations
│   └── tags.ts          # Tag management
├── app/                 # Next.js App Router
│   ├── notes/           # Notes page
│   ├── layout.tsx       # Root layout
│   ├── page.tsx         # Home (redirects to /notes)
│   └── globals.css      # Global styles
├── components/          # React Components
│   ├── calendar/        # Calendar widget
│   ├── layout/          # Layout components
│   ├── notes/           # Note components
│   ├── tags/            # Tag components
│   ├── ui/              # Reusable UI components
│   └── NotesClient.tsx  # Main client component
├── lib/                 # Utilities
│   ├── prisma.ts        # Prisma client singleton
│   ├── utils.ts         # Helper functions
│   └── validations.ts   # Zod schemas
├── prisma/
│   └── schema.prisma    # Database schema
└── types/
    └── index.ts         # TypeScript types
```

## Database Schema

### Note
- `id` (String, PK) - Unique identifier
- `title` (String?, optional) - Note title
- `content` (String) - Note content (max 50,000 chars)
- `createdAt` (DateTime) - Creation timestamp
- `updatedAt` (DateTime) - Last modified timestamp

### Tag
- `id` (String, PK) - Unique identifier
- `name` (String, unique) - Tag name (max 50 chars)
- `color` (String) - HEX color code

### NoteTag (Join Table)
- `noteId` (String, FK) - Reference to Note
- `tagId` (String, FK) - Reference to Tag
- Composite PK: (noteId, tagId)

## Development

### Database Commands

```bash
# View database in Prisma Studio
npx prisma studio

# Create new migration
npx prisma migrate dev --name migration_name

# Reset database (⚠️ deletes all data)
npx prisma migrate reset

# Generate Prisma Client
npx prisma generate
```

### Type Checking

```bash
npm run type-check
```

### Linting

```bash
npm run lint
```

## Architecture

### Server Actions

All mutations use Next.js 15 Server Actions for type-safe, direct server-client communication:

- `createNote(data)` - Create new note
- `updateNote(id, data)` - Update note content/title
- `deleteNote(id)` - Delete note
- `getNotes(filters?)` - Fetch notes with optional filters
- `createTag(data)` - Create new tag
- `updateTag(id, data)` - Update tag
- `deleteTag(id)` - Delete tag
- `assignTagsToNote(data)` - Assign tags to note
- `replaceNoteTags(noteId, tagIds)` - Replace all tags on note

### Response Format

All server actions return a discriminated union:

```typescript
type ActionResponse<T> = 
  | { success: true; data: T }
  | { success: false; error: string }
```

### Auto-Save

Notes auto-save 2 seconds after typing stops using a debounced effect in the editor component.

### Filtering

- **Tags**: OR logic (note appears if it has ANY selected tag)
- **Date**: Notes created on selected calendar date
- **Combined**: Both filters applied together (AND logic)

## Features Implemented

### Phase 1: Setup ✅
- [x] Next.js 15 project with TypeScript
- [x] Tailwind CSS configuration
- [x] ESLint and Prettier setup
- [x] Core dependencies installed

### Phase 2: Foundation ✅
- [x] Prisma schema with Note, Tag, NoteTag models
- [x] Database indexes for performance
- [x] Validation schemas with Zod
- [x] TypeScript types
- [x] Utility functions
- [x] Root layout and routing

### Phase 3: User Story 1 - Notes ✅
- [x] Create, read, update, delete notes
- [x] Tiptap rich text editor
- [x] Auto-save with 2-second debounce
- [x] Three-panel layout
- [x] Notes list with preview
- [x] Character count (50,000 limit)

### Phase 4: User Story 5 - Delete ✅
- [x] Delete button with confirmation dialog
- [x] Cascade delete of tag associations

### Phase 5: User Story 2 - Tags ✅
- [x] Create, update, delete tags
- [x] Color picker with presets
- [x] Tag assignment to notes
- [x] Multi-select tag filtering (OR logic)
- [x] Tag display on notes

### Phase 7: User Story 4 - Calendar ✅
- [x] Calendar widget with date selection
- [x] Filter notes by creation date
- [x] Clear date filter

## Known Limitations

- **No Authentication**: Single-user or demo mode
- **No Mobile Support**: Desktop-optimized (min 1024px)
- **No Real-time Collaboration**: Single-user editing
- **No Undo/Redo**: Beyond browser defaults
- **No Export**: Notes stored in database only
- **Database Required**: Must have PostgreSQL connection to run

## Performance

- **Page Load**: <2 seconds (React Server Components)
- **Note Creation**: <1 second
- **Auto-Save**: <3 seconds after typing stops
- **Filter Response**: <1 second
- **Supports**: 1000+ notes, 100+ tags

## Troubleshooting

### Database Connection Error

```bash
# Check PostgreSQL is running
psql -U username -d easynotes

# Test Prisma connection
npx prisma db pull
```

### Prisma Client Not Found

```bash
npx prisma generate
```

### Type Errors

```bash
npx prisma generate
# Then restart TypeScript server in VS Code
# Cmd+Shift+P → "TypeScript: Restart TS Server"
```

## Deployment

### Vercel (Recommended)

1. Push code to GitHub
2. Import repository in Vercel
3. Set environment variables:
   - `DATABASE_URL` - PostgreSQL connection string
4. Deploy!

### Production Migration

```bash
npx prisma migrate deploy
```

## Contributing

This is a demonstration project for the EasyNotes specification. See `specs/001-note-taking-core/` for complete specification and planning documents.

## License

MIT

## Support

For issues or questions, please refer to the specification documents in `/specs/001-note-taking-core/`.
