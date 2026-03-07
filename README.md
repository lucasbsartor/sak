# SAK — Swiss Army Knife

A opinionated, copy-paste-ready collection of configs and conventions for TypeScript projects. The goal is to not think about setup — clone, install, and start building.

## What's inside

- **Bun** as the runtime and package manager
- **oxlint** for linting and type checking (replaces both ESLint and tsc)
- **oxfmt** for formatting (replaces Prettier)
- **VSCode** settings and extension recommendations
- **AGENTS.md** with development guidelines and UI/UX design principles for AI-assisted development

## Setup

```bash
bun install
```

## Commands

| Command | Description |
| --- | --- |
| `bun run lint` | Lint and type check |
| `bun run lint:fix` | Lint with auto-fix |
| `bun run fmt` | Format all files |
| `bun run fmt:check` | Check formatting |

## Agent Setup Instructions

Follow these steps to set up linting, formatting, type checking, and editor config in a new TypeScript project. Fetch all config files from the SAK repo using the raw GitHub URLs below.

**Base URL:** `https://raw.githubusercontent.com/lucasbsartor/sak/refs/heads/main`

### 1. Install dependencies

```bash
bun add -d oxlint oxfmt oxlint-tsgolint @types/bun
bun add typescript
```

### 2. Fetch config files

Download and create these files in the project root:

| File | URL |
| --- | --- |
| `.oxlintrc.json` | `{BASE_URL}/.oxlintrc.json` |
| `.oxfmtrc.json` | `{BASE_URL}/.oxfmtrc.json` |
| `tsconfig.json` | `{BASE_URL}/tsconfig.json` |
| `.vscode/settings.json` | `{BASE_URL}/.vscode/settings.json` |
| `.vscode/extensions.json` | `{BASE_URL}/.vscode/extensions.json` |

### 3. Add scripts to package.json

Merge the following into the `scripts` field of `package.json`:

```json
{
    "lint": "oxlint",
    "lint:fix": "oxlint --fix",
    "fmt": "oxfmt --write",
    "fmt:check": "oxfmt --check"
}
```

### 4. Set package.json module type

Ensure `package.json` includes:

```json
{
    "type": "module"
}
```
