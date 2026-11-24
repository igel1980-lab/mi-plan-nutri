# Mi plan

A personal nutrition-plan app (Spanish UI), built as a single-page installable PWA. It covers menus, supplements, recipes, shopping list and progress tracking for the plan running from **16 November to 10 January**.

## Features

- **Hoy** – today's menu and supplement checklist
- **Calendario** – browse any day of the plan
- **Suplementos** – daily supplement schedule with configurable times (breakfast 09:00, lunch 14:00, dinner 19:30 by default)
- **Recetas** – recipes that are gluten-free, dairy-free and sugar-free
- **Compra** – weekly shopping list, with copy-to-clipboard
- **Progreso** – weight and sport log
- Light/dark theme switch in the header (sun/moon button). Follows the system setting until you pick one; your choice is remembered
- Works offline and can be installed to the home screen
- Backup/restore of data as a JSON file

## Tech

No build step and no dependencies: plain HTML, CSS and JavaScript.

| File | Purpose |
|------|---------|
| [index.html](index.html) | The whole app (markup, styles, logic) |
| [sw.js](sw.js) | Service worker: network-first for the app, cache fallback offline; caches Google Fonts |
| [manifest.webmanifest](manifest.webmanifest) | PWA manifest (name, icons, theme) |
| [vercel.json](vercel.json) | Headers: `sw.js` is never cached; correct manifest content type |
| [Avisos_mi_plan.ics](Avisos_mi_plan.ics) | Calendar file with daily supplement reminders (Europe/Madrid) |
| [LEEME.txt](LEEME.txt) | Step-by-step deployment guide in Spanish |
| `icon-*.png`, `icon.svg`, `apple-touch-icon.png` | App icons |

### Data storage

State (checked items, shopping list, custom entries, times, weights, sport) is saved in the browser's `localStorage` under the key `kineva-plan-raquel-v1`. Old data is pruned automatically (checks after 75 days, shopping lists after about 4 weeks). The theme choice is stored separately under `kineva-theme` (`light` or `dark`). If the page runs inside a Claude artifact host that provides a `db` capability, state is also synced there, and the most recently updated copy wins.

## Run locally

Service workers need HTTP, so serve the folder rather than opening the file directly:

```sh
npx serve .
# or
python -m http.server 8000
```

Then open the printed address.

## Deploy

Hosted as a static site on [Vercel](https://vercel.com). Leave the framework preset as **Other**.

```sh
npx vercel --prod
```

Or import the GitHub repository from the Vercel dashboard. See [LEEME.txt](LEEME.txt) for the full walkthrough (in Spanish).

### Releasing an update

Bump the `CACHE` version in [sw.js](sw.js) (currently `mi-plan-v5`) when you change cached files, so installed copies refresh.

## Install on a phone

- **Android (Chrome):** open the site, then menu ⋮ → *Install app*.
- **iOS (Safari):** Share → *Add to Home Screen*.

## Reminders

Import [Avisos_mi_plan.ics](Avisos_mi_plan.ics) into Google, Apple or Outlook Calendar to get daily alerts for the supplement times.
