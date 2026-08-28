# Penn MEDIATED — Data

The Data Infrastructure page for the [Center on Media, Technology and Democracy](https://infodem.upenn.edu). Static HTML/CSS, no build step. Content mirrors https://infodem.upenn.edu/data/.

Same conventions as the [`home`](https://github.com/PennMEDIATED/home), [`about`](https://github.com/PennMEDIATED/about), and [`grants`](https://github.com/PennMEDIATED/grants) repos — shared spacing tokens, brand colors, fonts, and the "Subscribe Here" newsletter treatment.

- `index.html` — page markup
- `styles.css` — all styling (design tokens live at the top in `:root`)
- `assets/` — images, GIFs, and shared funder logos

## Updating content

- **Intro copy** (`.intro`): the page title, PennMAP description, and body paragraphs. Edit `index.html` directly — this content changes rarely, so there's no templated update flow like `home`'s What's New grid.
- **Research Projects** (`.data-projects`): a 3-up grid of external tools/dashboards built on PennMAP data, inside a `.data-projects__box` pale-orange callout (see below). To add one, drop its preview image/GIF into `assets/data/`, then add a new `.data-project` link following the existing pattern (title + image, wrapped in a link to the external tool). Unlike `home`'s `news-card` images, these are **not** cropped to a fixed aspect ratio — they're screenshots of live dashboards, and cropping risks cutting off the UI being shown. Let each image keep its native proportions.

## Style guide (shared across `home`, `about`, `grants`, and `data`)

All four repos are static HTML/CSS built off the same design system. If you're adding or editing anything — especially a new color or component — check the sibling repos' `styles.css` for an existing token before inventing one; this file documents what's confirmed to exist, but a repo can drift ahead of its own README, so a live check beats trusting this list blindly.

### Design tokens (`:root` in `styles.css`)

**Spacing** — Atlassian's 8px scale. Always use the variable, never a raw pixel value:

```
--space-025: 2px   --space-100: 8px   --space-300: 24px  --space-600: 48px
--space-050: 4px   --space-150: 12px  --space-400: 32px  --space-800: 64px
--space-075: 6px   --space-200: 16px  --space-500: 40px  --space-1000: 80px
--space-250: 20px
```

**Color:**

| Token | Hex | Use |
|---|---|---|
| `--c-dark` | `#0d0d0c` | Primary text, dark backgrounds |
| `--c-accent` | `#5533ee` | Brand purple |
| `--c-red` | `#f03d1f` | Brand red/orange, links, tags |
| `--c-gray` | `#888680` | Secondary/muted text |
| `--c-gray-dark` | `#54534f` | Body copy needing real contrast (~8:1 on white) — prefer this over `--c-gray` for paragraph text |
| `--c-light-bg` | `#f8f7f4` | Placeholder/image background, section backgrounds that need to read as neutral "off-white" |
| `--c-pale-orange` | `#fce4dc` | `--c-red` tinted ~12% onto white — matches `/grants`' `--c-pale-orange` exactly. Always a padded callout box sitting inside the normal content column (see `.data-projects__box`) — never a full-bleed section background |
| `--c-white` | `#ffffff` | — |

**Brand gradient** — used on every purple-to-red surface (this page's newsletter block, `about`'s orbital section, `home`'s hero): `linear-gradient(150deg, #5533ee 0%, #df3611 81%)` via `--c-gradient`. Never write this gradient out by hand or approximate it with different stops — reference the variable so a future palette tweak only has to happen in one place per repo.

**Type:**
- `--f-serif`: `'EB Garamond', Georgia, 'Times New Roman', serif` — headlines, quotes, the "MEDIATED" wordmark
- `--f-sans`: `'DM Sans', system-ui, -apple-system, sans-serif` — everything else
- `--f-mono`: `'Courier New', Courier, monospace` — small meta labels only

**Layout:** `--max-w: 1440px` page cap, `--pad-x: var(--space-1000)` (80px) side padding on the shared `*__inner` containers, scaling down responsively (32px under 900px, 20px under 480px) — matches `home`'s responsive `--pad-x`; backport to `about` if you touch its layout.

### Layout conventions

- Every section's content wrapper is named `.<section>__inner` and shares one rule (`width:100%; max-width:var(--max-w); margin-inline:auto; padding-inline:var(--pad-x);`). Add new sections to that shared selector list instead of writing a one-off inner container.
- Section-to-section vertical rhythm uses `--space-1000` (80px) for generous breaks and `--space-600` (48px) between a heading row and the content below it. When two full-bleed color *sections* sit back to back (rare on this page, but see `home`'s purple-then-orange CTA blocks), give the seam an explicit `margin` gap rather than letting the colors butt directly together — see `home`'s `.faculty-cta` for the pattern (`margin-top`/`margin-bottom: var(--space-800)`). This doesn't apply to callout *boxes* like `.data-projects__box` — those live inside the normal white page background and just need their own `padding`.
- BEM-ish naming: `.block__element`, modifiers as `.block--variant` or `.block__element--variant`.
- **Grid items with large images**: a grid item's default `min-width: auto` lets an image's native intrinsic width blow out the track even though the `<img>` itself is capped to `max-width:100%`. Set `min-width: 0` on the grid item (see `.data-project`) whenever a grid cell holds an image that might be wider than its column.
- **No eyebrow/kicker label above hero or section headings** — sitewide convention as of 2026-08-28 (see `about`/`team-leadership` READMEs, which had one removed).

### Shared components

- **Page intro / chapter heading** (`.intro` here, `.mission` in `about`): the page-top h1 is serif (`--f-serif`, 48px, weight 600, `--c-accent`); an in-page h2 subheading (e.g. "Penn Media Accountability Project (PennMAP)") drops to sans-serif (`--f-sans`, 32px, weight 600, `--c-dark`) so it reads as a section break rather than competing with the page title. Body paragraphs are DM Sans Light 20px with the lead paragraph of each block bolded (`font-weight:600`). Reuse this pattern for any future long-form text page rather than inventing a new type scale.
- **Newsletter CTA ("Subscribe Here")**: a white rectangle button with an animated conic-gradient border trace on hover (`@property --cta-angle` + the `cta-trace` keyframes), and the label knocked out via `background-clip:text` filled with `--c-gradient` so the brand gradient shows through the letterforms on hover. This exact effect lives in `home`, `about`, `grants`, and here — if you touch one, touch all four. This page's signup form ID differs from `home`/`about`'s (confirmed against the live site: `2104021` here vs `2017639` there) — don't copy the URL wholesale without checking which form the page is supposed to point to.
- **Supporters row**: `.supporters__label` 24px/weight 700/uppercase, Knight Foundation logo at `height:65px`, Penn logo at `height:120px`, both `width:auto`. Logos use `assets/knight-foundation-logo.png` and `assets/upenn-logo-full.png` — same files, copied across the other repos; don't re-crop or re-export one without the other.
- **`--c-pale-orange` callouts**: `grants` uses this token for small callout boxes (background fill + `border-left: 3px solid var(--c-red)`) and tag pills (`color: var(--c-red)` + `background: var(--c-pale-orange)`), not just plain section backgrounds. If you add a callout or tag anywhere, match that treatment rather than a flat fill only.
- **Cards/tiles in a grid**: never let a multi-column grid just shrink its columns as the viewport narrows — text becomes unreadably vertical. Reflow to fewer columns at defined breakpoints instead (see this page's `.data-projects__grid`, or `home`'s `.whats-new__grid`, for the pattern).

### Keeping the repos in sync

`home`, `about`, `grants`, and `data` are separate repos with duplicated CSS, not a shared stylesheet — so consistency is a discipline, not something enforced automatically. When you change a shared token or component in one repo, check whether the same change belongs in the others before considering the task done — and when adding something that might already exist elsewhere (a color, a component pattern), check the sibling repos' actual `styles.css` first rather than guessing a new value, the way `--c-pale-orange` almost got invented twice with two different hex values.
