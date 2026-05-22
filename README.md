# NookSpace

NookSpace is a desktop AI agent workspace built with Electron, React, TypeScript, and Vite. It gives users a local desktop interface for running AI-assisted work sessions with file tools, command execution, model configuration, trace visibility, skills, MCP connectors, and optional remote control workflows.

The project is designed for agentic coding and productivity use cases where a web-only chat interface is not enough: users need a persistent workspace, controlled filesystem access, session history, tool visibility, and integrations that can run close to local files.

## Project Goals

- Provide a desktop-first AI agent workspace with chat, session management, trace panels, and settings.
- Keep file and command operations scoped to configured workspaces instead of granting unrestricted system access.
- Support multiple AI provider styles, including Anthropic, OpenAI, OpenRouter, and custom Anthropic/OpenAI-compatible endpoints.
- Make the app extensible through built-in skills and Model Context Protocol (MCP) connectors.
- Package the app as installable desktop builds for Windows, macOS, and Linux using Electron Builder.

## Key Features

### Agent Workspace

- Multi-session chat workspace with persistent sessions, messages, and trace steps stored through SQLite.
- Working directory selection for local projects and per-session mounted workspace paths.
- Tool permission model for read, write, edit, bash, glob, grep, web fetch, web search, and user-question workflows.
- Visible trace and tool execution state for better transparency during agent runs.
- File attachment and image paste/drop handling in the welcome prompt flow, including image resizing before submission.

### Model and API Configuration

- Provider presets for OpenRouter, Anthropic, OpenAI, and custom endpoints.
- Support for Anthropic-native and OpenAI-compatible execution paths.
- API connection testing from the configuration UI.
- Optional Claude Code CLI path override.
- Thinking mode and developer logging settings exposed in the app configuration.

### Skills and Connectors

- Built-in skill discovery from `.claude/skills/`, including document-oriented skills present in the repository for DOCX, PDF, PPTX, XLSX, and skill creation workflows.
- Project and global skill loading through the skills manager.
- MCP connector management for stdio and SSE servers.
- MCP server presets, live server status, tool listing, and bundled Node.js support for packaged builds.

### Sandboxed and Scoped Execution

- Workspace path resolution for local tool operations.
- Sandbox-aware execution paths for WSL on Windows and Lima on macOS, with a constrained local fallback described in the security documentation.
- Path guards for sandbox mode that block obvious system paths and dangerous command patterns.
- Session-specific sandbox synchronization support.

### Remote Control Workflows

- Remote gateway management with configurable port, default working directory, safe-tool auto-approval, pairing, and paired-user tracking.
- Feishu channel integration through the remote control subsystem.
- Optional tunnel configuration through ngrok as exposed in the remote settings UI.

### Desktop UX

- React-based renderer with a sidebar, chat view, context panel, settings panel, permission dialog, sandbox setup dialog, and custom title bar.
- Localization infrastructure using i18next with English and Chinese locale files.
- Tailwind CSS styling and Lucide icons.
- External URL handling that opens non-app URLs in the system browser.

## Architecture and Tech Stack

NookSpace follows the standard Electron split between a privileged main process, a preload bridge, and a renderer UI.

```text
Electron main process
  Owns sessions, database access, provider runners, tools, MCP, skills, sandbox, remote control, credentials, logs, and native dialogs.

Preload bridge
  Exposes a controlled `window.electronAPI` surface to the renderer through Electron IPC.

React renderer
  Provides the desktop interface, chat/session state, settings screens, connector management, permissions, and localization.
```

Core technologies used in this repository:

- **Desktop runtime:** Electron
- **Frontend:** React, TypeScript, Vite, Tailwind CSS
- **State management:** Zustand
- **Persistence:** better-sqlite3 and electron-store
- **AI providers:** Anthropic SDK, OpenAI SDK, Anthropic Claude Agent SDK / Claude Code packages
- **Integrations:** Model Context Protocol SDK, Feishu SDK, WebSocket support, ngrok package
- **Testing:** Vitest
- **Packaging:** Electron Builder

## Important Folders and Modules

```text
nookspace/
├── src/main/                  # Electron main process
│   ├── claude/                # Claude agent environment and runner logic
│   ├── config/                # Provider presets and persisted app configuration
│   ├── credentials/           # Stored user credentials
│   ├── db/                    # SQLite schema and database helpers
│   ├── mcp/                   # MCP server configuration and runtime manager
│   ├── openai/                # OpenAI Responses runner
│   ├── remote/                # Remote gateway, routing, tunnels, and Feishu channel
│   ├── sandbox/               # WSL/Lima/native sandbox adapters and path guards
│   ├── session/               # Session lifecycle, queueing, messages, traces
│   ├── skills/                # Built-in, global, and project skill loading
│   ├── tools/                 # Workspace-aware tool execution
│   └── utils/                 # Logging and artifact parsing helpers
├── src/preload/               # Electron context bridge and IPC API
├── src/renderer/              # React UI, store, hooks, types, i18n, styles
├── .claude/skills/            # Built-in skills bundled with the app
├── docs/                      # Installation, configuration, architecture, security, contribution docs
├── scripts/                   # Node/Python/GUI tool preparation and MCP bundling scripts
├── tests/                     # Vitest test suite
├── electron-builder.yml       # Desktop packaging configuration
├── vite.config.ts             # Vite and Electron plugin configuration
└── package.json               # Scripts, dependencies, metadata
```

## Setup and Installation

Requirements are documented in `docs/INSTALLATION.md`:

- Node.js 18+
- npm 9+
- Linux, macOS, or Windows

Install dependencies:

```bash
npm install
```

Rebuild the native SQLite dependency for the Electron runtime:

```bash
npm run rebuild
```

Start the development app:

```bash
npm run dev
```

The development script also runs the repository preparation steps required by the current project setup:

- downloads/prepares the bundled Node.js runtime
- prepares Python support used by the app scripts
- builds the WSL agent
- builds the Lima agent
- bundles MCP server code
- starts Vite/Electron development mode

## Configuration

The repository includes `.env.example` with the following discoverable environment variables:

```bash
# Required for Anthropic/OpenRouter-style auth in development
ANTHROPIC_AUTH_TOKEN=your_api_key_here

# Optional custom API endpoint
ANTHROPIC_BASE_URL=https://openrouter.ai/api

# Optional model selection
CLAUDE_MODEL=anthropic/claude-sonnet-4.5

# Optional custom Claude Code CLI path
CLAUDE_CODE_PATH=C:/Users/yourname/AppData/Roaming/npm/node_modules/@anthropic-ai/claude-code/cli.js
```

The app also persists runtime configuration through Electron Store in `src/main/config/config-store.ts`. Supported settings include provider, API key, base URL, model, custom protocol, OpenAI API mode, Claude Code path, default working directory, developer logs, sandbox mode, thinking mode, and first-run configuration status.

Configuration can be managed through the app UI. The configuration store maps selected providers into the appropriate runtime environment variables for Anthropic-compatible or OpenAI-compatible execution.

## Development Commands

| Command | Description |
| --- | --- |
| `npm run dev` | Prepares runtime assets and starts development mode. |
| `npm run build` | Prepares runtime assets, runs TypeScript/Vite builds, and packages the app with Electron Builder. |
| `npm run test` | Runs the Vitest test suite. |
| `npm run lint` | Runs ESLint against `src` TypeScript and TSX files. |
| `npm run format` | Formats source TypeScript, TSX, and CSS files under `src`. |
| `npm run preview` | Runs Vite preview. |
| `npm run rebuild` | Rebuilds `better-sqlite3` for the installed Electron version. |
| `npm run build:wsl-agent` | Builds the WSL sandbox agent TypeScript project. |
| `npm run build:lima-agent` | Builds the Lima sandbox agent TypeScript project. |
| `npm run build:mcp` | Bundles MCP server code through `scripts/bundle-mcp.js`. |
| `npm run download:node` | Downloads/prepares the bundled Node.js runtime. |
| `npm run prepare:gui-tools` | Prepares GUI automation helper tools. |
| `npm run prepare:python` | Prepares Python support for the current platform. |
| `npm run prepare:python:all` | Prepares Python support for all supported targets. |

## Testing

Tests are located in `tests/` and run with Vitest:

```bash
npm run test
```

The current test suite covers areas such as file links, remote control panel behavior, API tester logic, artifact parsing and paths, session title flows, tool output paths, MCP tool names, Claude environment handling, and thinking options.

## Build and Distribution

Create production desktop packages:

```bash
npm run build
```

Packaging is configured in `electron-builder.yml`.

Current package targets defined there:

- Windows: NSIS installer for x64
- macOS: DMG for arm64 and x64
- Linux: AppImage for x64

The Electron Builder configuration also includes bundled runtime resources for Node.js, WSL/Lima agents, MCP servers, Python/GUI helper assets where configured, and built-in `.claude/skills/`.

## Security Notes

Security-related behavior is implemented across the Electron main process, preload bridge, tool executor, sandbox modules, and documented in `docs/SECURITY.md`.

- The renderer uses `contextIsolation: true` and `nodeIntegration: false`.
- Renderer access to native capabilities is routed through the explicit preload API in `src/preload/index.ts`.
- External URLs are intercepted and opened in the system browser instead of navigating the app window.
- File tools resolve paths through workspace-aware path handling before reading or writing.
- Sandbox path guards block many system directories and dangerous command patterns when sandbox mode is active.
- App configuration is stored with Electron Store, including a basic encryption key for API-key storage.

Users should still review agent actions before approving risky operations, especially commands and write/edit tool calls.

## Performance and Reliability Notes

- SQLite runs with WAL mode enabled and indexes for session/message/trace lookup paths.
- Prepared database statements are used for common session and message operations.
- Vite handles renderer builds, while TypeScript compilation covers Electron-side code.
- The packaged app can use bundled Node.js resources for MCP server execution.
- Large pasted images are resized/compressed in the renderer before being sent to provider APIs.

## Documentation

Additional documentation:

- [`docs/INSTALLATION.md`](docs/INSTALLATION.md)
- [`docs/CONFIGURATION.md`](docs/CONFIGURATION.md)
- [`docs/SECURITY.md`](docs/SECURITY.md)
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)
- [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md)
- [`docs/TURKCE.md`](docs/TURKCE.md)
- [`README_TR.md`](README_TR.md)
- [`src/renderer/i18n/README.md`](src/renderer/i18n/README.md)

## Future Improvements

- Add product screenshots, short demo clips, and visual walkthroughs to support portfolio and release pages.
- Add CI configuration for lint, test, and build verification.
- Expand release documentation with platform-specific signing/notarization notes.
- Document remote control setup in more detail, especially Feishu and tunnel configuration.
- Add more end-to-end coverage around packaged app behavior, sandbox setup, and MCP connector flows.

## Project Status

- Current version: `3.0.0`
- Product name: `NookSpace`
- License: MIT
- Primary maintainer: `Ali Çağlar Koçer`
- Publish target in `electron-builder.yml`: `caglarkc/nookspace`

## License

MIT © Ali Çağlar Koçer
