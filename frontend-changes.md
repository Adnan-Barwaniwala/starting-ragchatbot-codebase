# Frontend Changes: Dark/Light Theme Toggle

## Feature
Added a toggle button that switches between dark (default) and light themes.

## Files Modified

### `frontend/index.html`
- Added a `<button id="themeToggle">` element with fixed positioning, placed directly inside `<body>` before `.container`
- Button contains two SVG icons: a **sun** (shown in dark mode) and a **moon** (shown in light mode)
- Includes `aria-label` and `title` attributes for accessibility

### `frontend/style.css`
- Added `[data-theme="light"]` CSS variable overrides:
  - `--background: #f8fafc`
  - `--surface: #ffffff`
  - `--surface-hover: #f1f5f9`
  - `--text-primary: #0f172a`
  - `--text-secondary: #64748b`
  - `--border-color: #e2e8f0`
  - `--assistant-message: #f1f5f9`
  - `--shadow` reduced opacity for light context
  - `--welcome-bg: #dbeafe`
- Added `transition: background-color 0.3s ease, color 0.3s ease, border-color 0.3s ease` to all major UI elements for smooth switching
- Added `.theme-toggle` button styles:
  - `position: fixed; top: 1rem; right: 1rem; z-index: 1000`
  - 40×40px circular button matching the design aesthetic
  - Hover: scales up, highlights in primary blue, shows focus ring
  - Focus outline replaced with `box-shadow` focus ring for consistency
- Icon visibility rules: sun visible in dark mode, moon visible in light mode (via `[data-theme="light"]` selector)

### `frontend/script.js`
- Added an IIFE at the top of the file that reads `localStorage` and applies the saved theme to `<html>` immediately, preventing a flash of wrong theme on page load
- Added `initTheme()` function (sets `data-theme` from `localStorage` on DOMContentLoaded)
- Added `toggleTheme()` function:
  - Reads current `data-theme` attribute on `<html>`
  - Toggles the attribute between `"light"` and absent (dark)
  - Persists choice to `localStorage` under key `"theme"`
- Registered click listener for `#themeToggle` in `setupEventListeners()`

## Design Decisions
- Theme attribute is set on `document.documentElement` (`<html>`) so CSS selectors like `[data-theme="light"] .theme-toggle` work globally
- Dark mode is the default (no attribute needed); light mode uses `data-theme="light"`
- Theme preference persists across page reloads via `localStorage`
- All color changes rely on the existing CSS variable system — no element-level style overrides needed
