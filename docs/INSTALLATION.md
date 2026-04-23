# Installation Guide

## Requirements

- Node.js 18+ (recommended: latest LTS)
- npm 9+
- OS: Linux, macOS, or Windows

## Local Development Setup

1. Clone the repository:

```bash
git clone https://github.com/caglarkc/nookspace.git
cd nookspace
```

2. Install dependencies:

```bash
npm install
```

3. Rebuild native modules for your Electron version:

```bash
npm run rebuild
```

4. Start development:

```bash
npm run dev
```

## Production Build

Create a production desktop package:

```bash
npm run build
```

Build output and installers are produced according to `electron-builder.yml`.

## Common Issues

- If native module errors appear, run `npm run rebuild`.
- If a clean install is needed:
  1. remove `node_modules`
  2. remove `package-lock.json` only if absolutely required
  3. run `npm install` again
