# Architecture Overview

## High-Level Layers

NookSpace follows a typical Electron split architecture:

1. **Main process (`src/main`)**
   - Session lifecycle
   - Tool execution
   - Sandbox and security checks
   - Skills loading and management
2. **Preload bridge (`src/preload`)**
   - Exposes controlled IPC APIs to the renderer
3. **Renderer (`src/renderer`)**
   - React UI
   - i18n
   - state and interaction logic

## Main Process Responsibilities

Key areas under `src/main`:

- `config/`: persistent settings and config stores
- `session/`: session orchestration and state
- `tools/`: local tool executors and validation
- `mcp/`: connector and integration management
- `skills/`: skill discovery and validation
- `sandbox/`: adapter and sandbox integration

## Renderer Responsibilities

Key areas under `src/renderer`:

- `components/`: major UI surfaces and reusable components
- `store/`: app-level state
- `i18n/`: localization config and locale files
- `types/`: shared UI type definitions

## Data and Execution Flow

1. User sends a prompt from UI
2. Renderer sends request through preload IPC bridge
3. Main process routes request to session and tool subsystems
4. Tool results and traces are persisted and streamed back to UI

## Build and Packaging

- Vite builds renderer assets
- TypeScript compiles main/preload logic
- Electron Builder packages desktop artifacts
