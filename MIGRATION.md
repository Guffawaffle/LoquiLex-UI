# LoquiLex-UI Project Migration

**Date**: October 31, 2025
**Status**: ✅ Complete

## What Was Moved

All UI-related content has been extracted from the main LoquiLex repository into a standalone project.

### New Project Location
```
~/LoquiLex-UI/
```

### Content Structure

```
LoquiLex-UI/
├── .git/                       Git repository
├── .gitignore                  VCS exclusions
├── .gitattributes              Line ending rules
├── README.md                   Project documentation
├── package.json                Root workspace config (monorepo)
│
├── app/                        Legacy React/Vite UI
│   ├── package.json
│   ├── vite.config.ts
│   ├── tsconfig.json
│   ├── playwright.config.ts
│   ├── index.html
│   ├── src/
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   ├── index.css
│   │   ├── components/        (15+ React components)
│   │   ├── hooks/
│   │   ├── utils/
│   │   ├── types/
│   │   └── test/              (E2E tests)
│   └── public/                (Static assets)
│
└── web/                        Current React/Vite UI (enhanced)
    ├── package.json
    ├── vite.config.ts
    ├── vitest.config.ts
    ├── tsconfig.json
    ├── tailwind.config.js
    ├── postcss.config.js
    ├── index.html
    └── src/
        ├── App.tsx
        ├── main.tsx
        ├── api.ts              (API client)
        ├── store.ts            (Zustand state)
        ├── ws.ts               (WebSocket setup)
        ├── styles.css
        ├── components/        (7+ React components)
        ├── hooks/
        ├── types/
        └── orchestration/     (Client-side utilities)
            ├── client/        (WebSocket client)
            ├── utils/         (Concurrency, retry, throttle, etc.)
            ├── worker/        (Web Worker implementations)
            ├── logging/       (Structured logging)
            ├── store/         (Store helpers)
            └── __tests__/     (Tests)
```

### What Stays in LoquiLex

The main `LoquiLex` repository now contains:
- **CLI tools** (`loquilex/cli/`)
- **Core ML/ASR modules** (`loquilex/asr/`, `loquilex/mt/`, etc.)
- **Backend API server** (`loquilex/api/`)
- **Configuration & utilities** (remaining modules)

### Files Moved

From `/home/guff/LoquiLex/ui/app/` → `~/LoquiLex-UI/app/`:
- 50+ React/TypeScript files
- Build configuration (Vite, TypeScript, Playwright)
- Component library + tests

From `/home/guff/LoquiLex/loquilex/ui/web/` → `~/LoquiLex-UI/web/`:
- 60+ React/TypeScript files
- Advanced orchestration utilities
- Build configuration (Vite, TypeScript, Vitest)
- Component library + tests

## Architecture

### Before (Monolith)
```
LoquiLex/
├── loquilex/         (Python ML backend)
├── ui/               (React/Vite apps)
└── loquilex/ui/web/  (Newer React/Vite app)
```

### After (Separated)
```
LoquiLex/             (Python CLI + Backend API)
└── loquilex/
    ├── cli/          (Command-line tools)
    ├── api/          (FastAPI server)
    ├── asr/          (Speech recognition)
    ├── mt/           (Machine translation)
    └── ...

LoquiLex-UI/          (React UI wrapper)
├── app/              (Legacy UI)
└── web/              (Current UI with enhancements)
```

## Integration

Both UIs in `LoquiLex-UI` communicate with LoquiLex via:

1. **WebSocket API**
   - Endpoint: `ws://localhost:8000/ws/events/{session_id}`
   - Real-time event streaming

2. **REST API**
   - Model discovery: `GET /models/asr`, `GET /models/mt`
   - Session management: `/sessions/*`
   - Profile CRUD: `/profiles/*`
   - Hardware info: `/hardware`

3. **Profile Storage**
   - Backend stores profiles in `~/.loquilex/profiles/`
   - UI can read/write via API endpoints

## Git Repository

The LoquiLex-UI project is now a standalone git repo:

```bash
cd ~/LoquiLex-UI
git log                    # View commit history
git remote add origin ...  # Add remote when ready
git push -u origin main    # Push to GitHub
```

**Initial commit**: `72011ce`

## Next Steps

1. ✅ Extract UI to standalone project
2. ⬜ Set up CI/CD for LoquiLex-UI (optional)
3. ⬜ Configure npm/GitHub for dependency management
4. ⬜ Update LoquiLex documentation to point to LoquiLex-UI
5. ⬜ Remove UI build targets from main LoquiLex Makefile (already done)

## Testing

To verify the migration:

```bash
# Check both UIs are present
ls -R ~/LoquiLex-UI/app/src ~/LoquiLex-UI/web/src

# Check dependencies
cd ~/LoquiLex-UI/app && npm list
cd ~/LoquiLex-UI/web && npm list

# Install & build
cd ~/LoquiLex-UI
npm install
npm run build:app
npm run build:web
```

---

**Status**: Ready for development and deployment as independent UI project.
