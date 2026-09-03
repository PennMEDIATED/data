# Penn MEDIATED — Data

The Data Infrastructure page for the [Center on Media, Technology and Democracy](https://infodem.upenn.edu). Static HTML/CSS, no build step. Content mirrors https://infodem.upenn.edu/data/.

Same conventions as the [`home`](https://github.com/PennMEDIATED/home), [`about`](https://github.com/PennMEDIATED/about), and [`grants`](https://github.com/PennMEDIATED/grants) repos — shared spacing tokens, brand colors, and fonts. Unlike those three, this page does **not** carry the "Subscribe Here" newsletter block or supporters row — both were removed (2026-08-31) as footer content that didn't belong on this page; don't re-add them without being asked.

- `index.html` — page markup
- `styles.css` — all styling (design tokens live at the top in `:root`)
- `assets/` — images, GIFs, and shared funder logos

## Updating content

- **Intro copy** (`.intro`): the page title, PennMAP description, and body paragraphs. Edit `index.html` directly — this content changes rarely, so there's no templated update flow like `home`'s What's New grid.
- **Research Projects** (`.data-projects`): a 3-up grid of external tools/dashboards built on PennMAP data, on a full-bleed `--c-accent` (brand purple) backdrop — matches `home`'s `.research-cta` treatment, white card/text floating on solid purple, rather than a contained pale-orange callout. To add one, drop its preview image/GIF into `assets/data/`, then add a new `.data-project` link following the existing pattern (title + image, wrapped in a link to the external tool). Unlike `home`'s `news-card` images, these are **not** cropped to a fixed aspect ratio — they're screenshots of live dashboards, and cropping risks cutting off the UI being shown. Let each image keep its native proportions.

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
| `--c-pale-orange` | `#fce4dc` | `--c-red` tinted ~12% onto white — matches `/grants`' `--c-pale-orange` exactly. Reserved on this page (no longer used — `.data-projects__box` moved to a full-bleed `--c-accent` background, see below) |
| `--c-white` | `#ffffff` | — |

**Brand gradient** — used on every purple-to-red surface (this page's newsletter block, `about`'s orbital section, `home`'s hero): `linear-gradient(150deg, #5533ee 0%, #df3611 81%)` via `--c-gradient`. Never write this gradient out by hand or approximate it with different stops — reference the variable so a future palette tweak only has to happen in one place per repo.

**Type:**
- `--f-serif`: `'EB Garamond', Georgia, 'Times New Roman', serif` — headlines, quotes, the "MEDIATED" wordmark
- `--f-sans`: `'DM Sans', system-ui, -apple-system, sans-serif` — everything else

**Layout:** `--max-w: 1440px` page cap, `--pad-x: var(--space-1000)` (80px) side padding on the shared `*__inner` containers, scaling down responsively (32px under 900px, 20px under 480px) — matches `home`'s responsive `--pad-x`; backport to `about` if you touch its layout.

### Layout conventions

- Every section's content wrapper is named `.<section>__inner` and shares one rule (`width:100%; max-width:var(--max-w); margin-inline:auto; padding-inline:var(--pad-x);`). Add new sections to that shared selector list instead of writing a one-off inner container.
- Section-to-section vertical rhythm uses `--space-1000` (80px) for generous breaks and `--space-300` (24px) between a section heading and the content below it. When two full-bleed color *sections* sit back to back (rare on this page, but see `home`'s purple-then-orange CTA blocks), give the seam an explicit `margin` gap rather than letting the colors butt directly together — see `home`'s `.faculty-cta` for the pattern (`margin-top`/`margin-bottom: var(--space-800)`). This doesn't apply to callout *boxes* like `.data-projects__box` — those live inside the normal white page background and just need their own `padding`.
- BEM-ish naming: `.block__element`, modifiers as `.block--variant` or `.block__element--variant`.
- **Grid items with large images**: a grid item's default `min-width: auto` lets an image's native intrinsic width blow out the track even though the `<img>` itself is capped to `max-width:100%`. Set `min-width: 0` on the grid item (see `.data-project`) whenever a grid cell holds an image that might be wider than its column.
- **No eyebrow/kicker label above hero or section headings** — sitewide convention as of 2026-08-28 (see `about`/`team-leadership` READMEs, which had one removed).

### Shared components

- **Page intro / chapter heading** (`.intro` here, `.mission` in `about`): the page-top h1 is serif (`--f-serif`, `--fs-h1`, weight 600, `--c-accent`); an in-page h2 subheading (e.g. "Penn Media Accountability Project (PennMAP)") drops to sans-serif (`--f-sans`, `--fs-h2`, weight 600, `--c-dark`) so it reads as a section break rather than competing with the page title. Body paragraphs are DM Sans Light at `--fs-lede`. Reuse this pattern for any future long-form text page rather than inventing a new type scale. (This page's lead paragraphs were bolded at `font-weight:600` until 2026-08-31 — removed as unintentional-looking emphasis with no clear purpose; don't reintroduce it without a reason.)
- **Purple full-bleed callout** (`.data-projects`): a solid `--c-accent` section running edge to edge (background on the outer, unconstrained section — not on the width-capped `*__inner`), with white text and white-bordered image cards floating on it with a strong shadow (`0 24px 64px rgba(0,0,0,.25)`, `0 32px 72px rgba(0,0,0,.32)` on hover). Matches `home`'s `.research-cta` exactly. This is the pattern to reach for anywhere a purple "featured content" band is needed — not a contained pale-orange callout.
- **Cards/tiles in a grid**: never let a multi-column grid just shrink its columns as the viewport narrows — text becomes unreadably vertical. Reflow to fewer columns at defined breakpoints instead (see this page's `.data-projects__grid`, or `home`'s `.whats-new__grid`, for the pattern).

## Hyperlinks

One taxonomy, five categories, shared by every page repo. Pick the category by what the link *is*, not by which repo you happen to be editing.

**1. In-text links** — embedded mid-sentence in flowing prose.

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-dark` | `border-bottom: 1px solid rgba(13, 13, 12, 0.35)` | text and underline both turn `--c-red` |
| colour / gradient | `--c-white` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` — no colour swap |

The underline is a `border-bottom`, not `text-decoration`, so its colour can be transitioned independently of the text on hover. Pair it with `transition: color 0.15s, border-color 0.15s` on light grounds and `transition: opacity 0.15s` on coloured ones.

White-to-anything reads poorly on a saturated ground, which is why the coloured case fades instead of changing hue.

**2. Independent links** — a standalone text link that isn't inside a sentence ("Learn More About the Center", "Download the Full Schedule"). Same colours, decoration and hover as category 1, **plus a thin arrow** `⟶` after the text. Use `⟶` (`&#10230;`), not the `↗` badge from category 4.

**3. Document buttons** — an independent link that opens a document (a PDF, a report). A filled button box, not text:

| ground | box | text |
| --- | --- | --- |
| white / light | `--c-red` | `--c-white` |
| colour / gradient | `--c-white` | `--c-dark` |

Hover is **movement, not colour** — a lift or nudge. Do not darken or recolour the box.

**4. Links to another web page** — this site or an external one. The containing box carries the shared `.card-arrow`: a 26px dark circle with a white `↗`, in the box's top corner. On hover the arrow scales slightly and its background becomes a sliding purple-to-orange gradient (`@keyframes card-arrow-slide`), and the box itself animates. No separate text button — the whole box is the link.

**Exception:** a link to a research paper is category 2, not this — thin arrow, no badge.

**5. Hyperlinked headings** — a heading that is itself a link (a post title, a card title). Colour shift on hover per the ground rules above, and **no arrow and no underline**.

### Dropdowns and disclosures

A dropdown, `<details>` block or expand/collapse control uses one affordance sitewide: a **chevron SVG** (`M2 5l5 5 5-5`, 13×13, `--c-red` stroke, `stroke-width: 1.8`) beside a `--c-red` label at `--fs-small`, rotating `180deg` on open with `transition: transform 0.25s`. See `llm-civic-discourse`'s "Full summary & details" toggle for the reference implementation.

Never leave the marker to the browser — style `<select>` with `appearance: none` and supply the chevron, and hide the native `<summary>` marker. The `↗` circle badge is category 4's language and does not belong on a disclosure control.

### Keeping the repos in sync

`home`, `about`, `grants`, and `data` are separate repos with duplicated CSS, not a shared stylesheet — so consistency is a discipline, not something enforced automatically. When you change a shared token or component in one repo, check whether the same change belongs in the others before considering the task done — and when adding something that might already exist elsewhere (a color, a component pattern), check the sibling repos' actual `styles.css` first rather than guessing a new value, the way `--c-pale-orange` almost got invented twice with two different hex values.

## Typography

Sitewide convention. The `--fs-*`/`--lh-*` block at the top of `styles.css` is canonical and identical in every page repo.

**Two families, no third.** `--f-serif` (EB Garamond) for page and section titles and pull-quote copy; `--f-sans` (DM Sans) for everything else. There is no monospace face — uppercase micro-labels are DM Sans 700 uppercase with `letter-spacing: 0.08em`.

**Sizes come from tokens, never raw px.**

| Token | Mobile (=<480px) | Desktop (>=1440px) | Used for |
| --- | --- | --- | --- |
| `--fs-display` | 36px | 76px | full-bleed hero |
| `--fs-h1` | 36px | 56px | page title |
| `--fs-h2` | 26px | 40px | section titles |
| `--fs-h3` | 20px | 24px | card and third-level titles |
| `--fs-lede` | 18px | 20px | intro paragraphs |
| `--fs-body` | 16px | 16px | body copy |
| `--fs-small` | 14px | 14px | captions, meta, form controls |
| `--fs-small-serif` | 15px | 15px | EB Garamond at small sizes |
| `--fs-micro` | 12px | 12px | uppercase labels, tags, counts |

The top five are `clamp()` values that interpolate across the viewport, so tablet widths need no separate `@media` override. Only add a breakpoint font-size when a specific layout actually demands it.

**12px is the floor.** Nothing ships smaller. EB Garamond and uppercase-with-letter-spacing both read smaller than their nominal size, which is what `--fs-small-serif` and the 12px floor exist to absorb.

**Line heights are tokens too** — `--lh-display` 1.05, `--lh-heading` 1.15, `--lh-lede` 1.26, `--lh-title` 1.3, `--lh-body` 1.55. Never set a line-height in px; it breaks the fluid sizes.

**Heading gaps.** Section title to first content is `var(--space-300)` (24px); page or hero title to content is `var(--space-250)` (20px).

**Section rhythm.** A full-width colored section carries `var(--space-1000)` (80px) top and bottom padding, so its heading never sits flush against the band's edge. The page hero's bottom padding is `var(--space-600)` (48px) — shorter than 80px because the section below supplies its own.

**Nav height.** These pages don't render the site nav — WordPress does. If a page ever needs to know the nav's height (for example `scroll-margin-top` on an in-page anchor target so a sticky nav doesn't cover the heading you jumped to), the header build is the one that sets `--nav-h`; page CSS reads `var(--nav-h, 0px)` so the page still lays out correctly standalone, where there is no nav. Don't hardcode a nav height in a page repo — it will drift from whatever the header actually renders.
