# LoquiLex UI

Web-based user interface and API wrapper for [LoquiLex](https://github.com/Guffawaffle/LoquiLex) — local-first live captioning and EN↔ZH translation.

## Overview

This project contains the web UI and API bridge for LoquiLex. It provides:

- **Web UI** (React/Vite) — Interactive interface for configuring and controlling LoquiLex sessions
- **Two UI versions**:
  - `app/` — Legacy React/Vite interface
  - `web/` — Newer React/Vite interface with enhanced features

## Architecture

```
LoquiLex-UI
├── app/              React/Vite web app (legacy)
└── web/              React/Vite web app (current)
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

```bash
npm install
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
npm run build:app
npm run build:web
```

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
