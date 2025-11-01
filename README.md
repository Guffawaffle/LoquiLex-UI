# LoquiLex UI

Web-based user interface and API wrapper for [LoquiLex](https://github.com/Guffawaffle/LoquiLex) — local-first live captioning and EN↔ZH translation.

## Overview

This project contains the web UI and API bridge for LoquiLex. It provides:

- **Web UI** (React/Vite) — Interactive interface for configuring and controlling LoquiLex sessions
- **Two UI versions**:
  - `app/` — Legacy React/Vite interface (`@loquilex/app`)
  - `web/` — Newer React/Vite interface with enhanced features (`@loquilex/web`)

This is a **monorepo** using npm workspaces for independent package management.

## Architecture

```
LoquiLex-UI
├── package.json           Root workspace configuration
├── app/                   React/Vite web app (legacy) - @loquilex/app
│   └── package.json       Independent package with own dependencies
└── web/                   React/Vite web app (current) - @loquilex/web
    ├── package.json       Independent package with own dependencies
    └── src/
        ├── components/      React components (UI, settings, etc.)
        ├── orchestration/   Client-side utilities and patterns
        └── App.tsx          Main application
```

## Setup

### Prerequisites

- Node.js 18+
- npm 9+
- [LoquiLex](https://github.com/Guffawaffle/LoquiLex) running locally

### Installation

Install all workspace dependencies from the root:

```bash
npm install
```

This will install dependencies for both `app/` and `web/` workspaces automatically.

You can also install dependencies for individual workspaces:

```bash
cd app && npm install   # Install app dependencies only
cd web && npm install   # Install web dependencies only
```

### Development

Start development servers for both apps:

```bash
npm run dev:app   # Run legacy app on http://localhost:5173
npm run dev:web   # Run newer app on http://localhost:5174
```

### Building

Build for production:

```bash
npm run build:app       # Build app workspace only
npm run build:web       # Build web workspace only
npm run build           # Build all workspaces
```

## NPM Workspace Management

This project uses npm workspaces to manage multiple packages independently.

### Available Commands

#### Development
```bash
npm run dev:app         # Start app development server
npm run dev:web         # Start web development server
```

#### Building
```bash
npm run build:app       # Build app workspace
npm run build:web       # Build web workspace
npm run build           # Build all workspaces
```

#### Testing
```bash
npm run test            # Run tests in all workspaces
npm run test:app        # Run tests in app workspace only
npm run test:web        # Run tests in web workspace only
```

#### Type Checking
```bash
npm run typecheck       # Type check all workspaces
npm run typecheck:app   # Type check app workspace only
npm run typecheck:web   # Type check web workspace only
```

#### Cleanup
```bash
npm run clean:app       # Remove app node_modules and dist
npm run clean:web       # Remove web node_modules and dist
npm run clean           # Clean all workspaces and root
```

### Working with Individual Workspaces

You can run commands in specific workspaces using npm's workspace flag:

```bash
# Install a package to a specific workspace
npm install react-query --workspace=@loquilex/app
npm install zustand --workspace=@loquilex/web

# Run a script in a specific workspace
npm run dev --workspace=@loquilex/app
npm run test --workspace=@loquilex/web
```

### Workspace Structure

Each workspace (`app/` and `web/`) has:
- Independent `package.json` with its own dependencies
- Separate build outputs (in respective `dist/` directories)
- Can be published independently to npm (when ready)
- Shares dependencies where possible (deduplication)

### Dependency Management

- **Shared dependencies** (like React, Vite, TypeScript) are automatically deduplicated
- **Version conflicts** are avoided through proper dependency specification
- Run `npm ls --all` to view the complete dependency tree
- Run `npm ls <package-name> --all` to check for version conflicts

## Integration with LoquiLex

The UI communicates with LoquiLex via:

- **WebSocket API** — Real-time event streaming at `ws://localhost:8000/ws/events/{sid}`
- **REST API** — Model discovery, session management, profile CRUD
- **Profiles** — Stored in `~/.loquilex/profiles/`

## Project Structure

### `app/` (Legacy)

- Vite + React 18
- TypeScript
- Tailwind CSS for styling
- Playwright E2E tests
- Components:
  - `LaunchWizard` — Initial setup flow
  - `SettingsView` — Configuration interface
  - `SessionTab` — Active session display
  - `ModelSelect` — Model picker
  - `VuMeter` — Audio level visualization

### `web/` (Current)

- Vite + React 18
- TypeScript
- Tailwind CSS
- Zustand state management
- Orchestration layer for client-side utilities:
  - `cancellation.ts` — Cancellation token pattern
  - `concurrency.ts` — Promise utilities
  - `retry.ts` — Retry logic with backoff
  - `bounded-queue.ts` — Memory-bounded queue
  - `throttle.ts` — Rate limiting
  - `ws-client.ts` — WebSocket client
- Components:
  - `LaunchWizard` — Session initialization
  - `SettingsDialog` — Configuration modal
  - `SessionTab` — Active session management
  - `HardwareInfo` — System information display
  - `DownloadsPage` — Model download tracking
  - `VuMeter` — Audio level visualization

## Dependencies

Both `app/` and `web/` include:

- React 18+
- Vite (build tool)
- TypeScript
- Tailwind CSS
- Vitest/Playwright (testing)

See `app/package.json` and `web/package.json` for complete dependency lists.

## Testing

Run all tests:

```bash
npm run test
```

### App tests
```bash
cd app && npm run test
cd app && npm run e2e
```

### Web tests
```bash
cd web && npm run test
```

## License

Same as LoquiLex. See parent repository for details.

## Contributing

This project is maintained alongside [LoquiLex](https://github.com/Guffawaffle/LoquiLex).

For issues, feature requests, or contributions, please open a PR/issue in the parent repository.

---

**Note**: This project depends on LoquiLex running locally. Ensure the LoquiLex API server is started before running the UI.
