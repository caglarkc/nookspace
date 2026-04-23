# Configuration Guide

## App Configuration Overview

NookSpace stores persistent application settings using Electron store.
Main configuration logic is managed under `src/main/config/`.

## Required Runtime Settings

At minimum, configure:

- API key for your selected model provider
- Base URL (for OpenAI-compatible endpoints)
- Model identifier
- Workspace directory

## Supported Provider Styles

- Anthropic-native endpoints
- OpenAI-compatible endpoints (custom base URL)

## Workspace Configuration

- Select a workspace folder on first run or from settings
- File operations are constrained to allowed workspace mounts
- Avoid using system directories as workspace roots

## Skill and Tool Configuration

- Built-in skills are loaded from `.claude/skills/`
- Custom skills can be added through the app skill flow
- Tool execution is managed from main process executors

## Build-Time Configuration

Relevant files:

- `electron-builder.yml`: packaging, product metadata, publish settings
- `package.json`: scripts, dependencies, metadata
