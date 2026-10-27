# PressFlow: Headless CMS & Portfolio Builder

PressFlow is a single-file demo of a headless CMS dashboard. You edit your portfolio content (title, bio, skills), watch a live preview update as you type, and "deploy" the changes. It also includes a simulated WordPress sync, a JSON API preview, and a few personalisation settings.

Everything lives in one file, `index.html`. There is no build step, no dependencies, and no backend.

## Quick start

1. Put `index.html` anywhere.
2. Open it in a modern browser (double-click, or serve it with any static server).

```bash
# optional: serve locally
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Features

### Dashboard
- Edit **portfolio title** (max 60 chars), **bio** (max 300 chars, with a live counter) and **skills** (comma-separated, duplicates removed, up to 12).
- **Live preview** in a browser-style frame, including a generated URL slug (e.g. `pressflow.app/aymen-nawaz`).
- **Deploy updates** saves the current content. A title is required to deploy.
- **Reset** reverts the form to the last deployed version.
- The sidebar shows the draft status ("Unsaved draft" / "All changes deployed") and the last deploy time.

### WordPress Sync (simulated)
- Validates an `http(s)` site URL, then runs a fake five-step sync with a progress bar and log.
- **No real requests are made.** The log says so explicitly. Replace the `steps`/`tick` logic with calls to the WordPress REST API (`/wp-json/wp/v2/posts`, `/wp-json/wp/v2/pages`) to make it real.

### API Endpoints (preview)
Shows the JSON a front end would consume, generated from the current form content:

| Endpoint | Returns |
|---|---|
| `GET /api/v1/profile` | `name`, `bio`, `skills` |
| `GET /api/v1/skills` | `count`, `items` |
| `GET /api/v1/meta` | `slug`, `initials`, `bioLength`, `deployedAt` |

You can copy the JSON to the clipboard or download it as `pressflow-<endpoint>.json`. These are in-page previews, not live URLs.

### Settings
- Choose from five accent colours.
- **Reduce motion** toggle (also respects the OS `prefers-reduced-motion` setting).
- **Erase saved data** clears everything stored in the browser.

## Data storage

Data is stored in the browser's `localStorage` under the key `pressflow.v1`. It never leaves the device. If storage is blocked (for example in some private modes), the app still works for the session and tells you that saving was blocked.

Stored fields: `name`, `bio`, `skills`, `accent`, `reduceMotion`, `deployedAt`.

## Customising

- **Default content:** edit the `DEFAULTS` object at the top of the script.
- **Accent colours:** edit the `ACCENTS` array.
- **Theme:** colours, radius and fonts are CSS variables in `:root`.

## Browser support

Any modern browser. The glows and gradients use `color-mix()`, which needs a 2023+ release of Chrome, Edge, Safari or Firefox. Older browsers will still work but lose some of the visual effects.

## Known limitations

- No real backend: "deploy" only saves to local browser storage.
- WordPress sync is a simulation.
- API endpoints are not real URLs.
- Only one profile is supported; there is no multi-entry content model yet.

## Roadmap ideas

- Real WordPress REST API integration
- A backend or static JSON export so the API URLs resolve
- Multiple content types (projects, posts, pages)
- Accessibility polish: full tab-pattern ARIA with arrow-key navigation, radio-input swatches, and an unsaved-changes warning on page close
