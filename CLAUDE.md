# Prep — Meal Planning App

## Project Overview
Single-file web app (`index.html`) for personal meal prep. Runs entirely in the browser with `localStorage` as primary storage and Google Sheets as optional cross-device sync backend.

## Architecture Rules
- **Single file only** — everything lives in `index.html`. No build tools, no npm, no bundlers.
- **localStorage-first** — all writes go to localStorage immediately; Google Sheets sync is fire-and-forget background push.
- **No external dependencies** — Google Fonts CDN is the only external resource. No frameworks, no libraries.
- **Prefix** — all localStorage keys use the prefix `prep_`.

## Data Structures

### Ingredient
```js
{ id, name, category, isStaple, dateAdded }
// category: 'produce' | 'protein' | 'dairy' | 'grain' | 'pantry' | 'spice' | 'other'
// isStaple: true = always in stock; false = added today
```

### Recipe
```js
{ id, name, emoji, prepTime, cookTime, servings, ingredients, instructions, tags, isSaved, timesCooked, lastCooked }
// ingredients: [{ name, qty }]
// tags: string[]
```

## Design System
- **Colors:** `--green-900` (#0a2318) sidebar bg, `--cream` (#fdfbf5) body bg, `--orange` (#e8713a) accent
- **Fonts:** Playfair Display (headings) + Inter (body) via Google Fonts
- **Radius:** sm=8px, md=14px, lg=20px, xl=28px
- **All design tokens** are CSS custom properties on `:root`

## Google Sheets Sync
Uses the `google-sheets-backend` skill pattern. Two sheets: `Ingredients` and `Recipes`. Push overwrites, pull fully replaces. URL stored in `localStorage` as `prep_sheets_url`.

## Skills in this workspace
- `google-sheets-backend` — reference for the Sheets sync pattern
- `apps-script` — canonical Apps Script for this app; check this whenever the data schema changes to determine if redeployment is needed
- `add-recipe` — how to add a new recipe to the app's seed data or UI
- `add-ingredients` — ingredient data shape, categories, matching rules
- `deploy` — how to deploy this app to GitHub Pages
- `new-feature` — checklist for adding a new feature to this app

## Apps Script Rule
After ANY change to the data schema (new fields, renamed fields, new sync actions), Claude must:
1. Update the script inside `<pre id="apps-script-code">` in `index.html`
2. Update the canonical script in `.claude/skills/apps-script/SKILL.md` (bump the schema version date)
3. Tell the user a redeploy is required and what changed
