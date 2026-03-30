# Frontend Code Quality Changes

## Summary

Added code quality tooling for the frontend (vanilla JS/CSS/HTML) and applied formatting consistency fixes to the existing source files.

---

## New Files

### `package.json`
Defines the project's frontend dev dependencies and npm scripts:
- `npm run format` — formats all frontend files with Prettier (modifies in place)
- `npm run format:check` — checks formatting without modifying files
- `npm run lint` — lints `frontend/script.js` with ESLint
- `npm run lint:fix` — auto-fixes ESLint issues in `frontend/script.js`

**Dev dependencies added:** `prettier@^3.3.0`, `eslint@^8.57.0`

---

### `.prettierrc`
Prettier configuration matching the existing code style:
- `tabWidth: 4` — 4-space indentation (consistent with current files)
- `singleQuote: true` — single quotes for JS strings
- `trailingComma: "es5"` — trailing commas in arrays/objects
- `printWidth: 88` — max line length
- `endOfLine: "lf"` — Unix line endings

---

### `.eslintrc.js`
ESLint configuration for browser-side JavaScript:
- Extends `eslint:recommended` ruleset
- Declares `marked` as a known global (loaded via CDN)
- `no-unused-vars`: warn
- `no-console`: warn (flags debug `console.log` calls left in production code)

---

### `scripts/frontend-format.sh`
Shell script to format all frontend files in one command:
```bash
./scripts/frontend-format.sh
```
Runs `prettier --write` over `frontend/**/*.{js,css,html}`.

---

### `scripts/frontend-lint.sh`
Shell script to check frontend quality without modifying files (suitable for CI):
```bash
./scripts/frontend-lint.sh
```
Runs `prettier --check` then `eslint` and exits non-zero if any check fails.

---

## Modified Files

### `frontend/script.js`
- Removed two double blank lines (between event listeners and between top-level functions) to match Prettier's single-blank-line-between-blocks style.

### `frontend/index.html`
- Removed an extra blank line between the closing `</div>` and the `<script>` tags at the bottom of `<body>`.

---

## Usage

Install dependencies once:
```bash
npm install
```

Then use the scripts directly:
```bash
# Format (modifies files)
./scripts/frontend-format.sh

# Lint only (no modifications — good for CI)
./scripts/frontend-lint.sh
```

Or use npm scripts:
```bash
npm run format        # format and save
npm run format:check  # check formatting only
npm run lint          # ESLint check
npm run lint:fix      # ESLint auto-fix
```
