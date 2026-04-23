# NookSpace

NookSpace is a desktop AI agent workspace built with Electron, React, and TypeScript.
It provides a controlled local environment where agents can use tools, manage files, run commands, and integrate with external services.

## Why NookSpace

- Safe-by-default workspace boundaries for file and command operations
- Desktop-first UX with chat, trace, tool visibility, and session management
- Built-in skill system for common document workflows (DOCX, PDF, PPTX, XLSX)
- Multi-provider model support (Anthropic and OpenAI-compatible APIs)
- Extensible integration layer with MCP-based connectors and tooling

## Project Status

- Current version: `3.0.0`
- License: MIT
- Primary maintainer: `Ali Çağlar Koçer`

## Quick Start

### 1) Install dependencies

```bash
npm install
npm run rebuild
```

### 2) Run in development mode

```bash
npm run dev
```

### 3) Build distributables

```bash
npm run build
```

## Core Scripts

| Script | Purpose |
| --- | --- |
| `npm run dev` | Starts development mode with required prebuild steps |
| `npm run build` | Produces production build and Electron packages |
| `npm run test` | Runs test suite with Vitest |
| `npm run lint` | Runs ESLint on source files |
| `npm run format` | Formats renderer code with Prettier |

## Documentation

Detailed docs are split into focused pages:

- [`docs/TURKCE.md`](docs/TURKCE.md)
- [`docs/INSTALLATION.md`](docs/INSTALLATION.md)
- [`docs/CONFIGURATION.md`](docs/CONFIGURATION.md)
- [`docs/SECURITY.md`](docs/SECURITY.md)
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)
- [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md)
- [`src/renderer/i18n/README.md`](src/renderer/i18n/README.md)

## Repository Layout

```text
nookspace/
├── src/
│   ├── main/        # Electron main process, tool execution, session management
│   ├── preload/     # Secure API bridge
│   └── renderer/    # React UI
├── scripts/         # Build and preparation scripts
├── tests/           # Unit and integration tests
├── resources/       # Static assets
└── docs/            # Project documentation
```

## Release and Distribution

- Build config: `electron-builder.yml`
- Product name: `NookSpace`
- Publish target (current): `caglarkc/nookspace`

## Contributing

Contributions are welcome. Please start with:

1. `docs/CONTRIBUTING.md`
2. Existing issues or a new proposal
3. A focused PR with test coverage where relevant

## License

MIT © Ali Çağlar Koçer
