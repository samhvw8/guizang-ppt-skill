---
name: guizang-ppt-skill
description: Generate horizontal-swipe web PPTs (single HTML file) with WebGL backgrounds, chapter dividers, data hero pages, image grids, and more. Two styles available: ① "Digital Magazine × E-Ink" (serif + fluid background + warm tones) ② "Swiss International Style" (sans-serif + dot grid + IKB / lemon yellow / lime green / safety orange highlights). Use when creating shareable / presentation / launch-style web decks, or when the user mentions "magazine-style PPT", "Swiss-style PPT", "Swiss Style", or "horizontal swipe deck".
---

# Magazine Web Ppt

## What This Skill Does

Generates a **single-file HTML** horizontal-swipe PPT with two visual styles to choose from:

### Style A · Digital Magazine × E-Ink (Default)

- **WebGL fluid / contour / dispersion background** (visible on hero pages)
- **Serif headings (Noto Serif SC + Playfair Display) + sans-serif body + monospace metadata**
- Best for: humanities talks, industry insights, business launches, presentations that need a "magazine feel"
- Templates: `assets/template.html` · Theme colors: `references/themes.md` · Layouts: `references/layouts.md`
- Aesthetic anchor: like *Monocle* magazine with code pasted on

### Style B · Swiss International Style (Swiss Style)

- **WebGL ultra-fine grid + dot matrix background** (information-driven design)
- **All sans-serif throughout (Inter + Helvetica + Noto Sans SC) + extreme type-size contrast**
- **High-contrast functional colors**: Klein Blue IKB / lemon yellow / lime green / safety orange (pick one)
- Best for: tech products, data reports, design/engineering talks, year-end reviews
- Templates: `assets/template-swiss.html` · Theme colors: `references/themes-swiss.md` · Layouts: `references/layouts-swiss.md`
- Aesthetic anchor: like Massimo Vignelli + Helvetica Forever

**Shared across both styles**: horizontal swiping (keyboard ← →, scroll wheel, touch, ESC index; `scroll-snap-stop: always` prevents fast-swipe skipping), Lucide icons, Motion entrance animations (local + CDN dual fallback). Both templates include `text-autospace: normal` for native CJK-Latin spacing (Chrome 139+, Safari 18.4+, Firefox 145+).

## When to Use

**Good fit**:
- Offline talks / internal industry presentations / private gatherings
- AI product launches / demo days
- Presentations with a strong personal style
- "Build once, no slideshow tool needed" web-based slides

**Not a good fit**:
- Heavy tabular data or stacked charts (use a conventional PPT tool)
- Training courseware (not enough information density)
- Multi-person collaborative editing (this is static HTML)

## Workflow

### Step 1 · Requirements Clarification (**mandatory before starting**)

**If the user already provided a complete outline + images**, skip ahead to Step 2.

**If the user only gave a topic or a vague idea**, align on these 6 questions before starting. Do not begin writing slides based on guesses — once the structure is wrong, rework costs are high:

#### Runtime Environment Adaptation

- **In Codex**: Ask the user directly in normal conversation. Do not invoke Claude Code's `ask question` / `ask_question` mechanism, and do not assume those tools are available. Ask at most 1-3 critical questions at a time; if information gaps don't block progress, make reasonable assumptions and state them in your reply.
- **In Claude Code**: Continue using the standard `ask question` interaction to clarify items one by one.

#### 7-Question Clarification Checklist

| # | Question | Why Ask |
|---|----------|---------|
| 1 | **Style A or B?** (Digital Magazine / Swiss International Style) | **Must ask first** — determines which template + layouts + themes files to use |
| 2 | **Who is the audience? What's the setting?** (internal industry / business launch / demo day / private gathering) | Determines tone and depth |
| 3 | **How long is the presentation?** | 15 min ≈ 10 pages, 30 min ≈ 20 pages, 45 min ≈ 25-30 pages |
| 4 | **Any source material?** (documents / data / old PPT / article links) | If yes, build from it; if not, help scaffold |
| 5 | **Any images? Where are they?** | See "Image Conventions" below |
| 6 | **Which theme color set?** | Magazine style has 5 sets (`themes.md`) / Swiss style has 4 sets (`themes-swiss.md`) — pick one |
| 7 | **Any hard constraints?** (must include XX data / must not mention YY) | Avoid rework |

#### Style Selection Guide (Question 1)

| If the user says... | Recommended Style |
|---|---|
| "magazine feel" / "humanities" / "Monocle style" / unspecified | **A · Digital Magazine** |
| "Swiss style" / "Swiss Style" / "Helvetica" / "minimal" / "grid" / "infographic" / "data-driven" | **B · Swiss International Style** |
| Content is AI products / tech / engineering / data reports | B is a better fit |
| Content is industry insights / humanities / stories / culture | A is a better fit |
| User provided lots of KPI numbers / roadmaps / workflows | B is a better fit (`Data Hero` layout is Swiss style's forte) |
| User provided lots of documentary photos / humanities imagery | A is a better fit (image grid, left-text-right-image are magazine style's forte) |
| User needs GPT Image 2 to generate screenshots for redesign / infographics / evidence walls | B also works well (P23/P24 are Swiss style's dedicated image layouts) |

#### Outline Assistance (if the user doesn't have an outline)

Use the "narrative arc" template to build the skeleton, then fill in content:

```
Hook              → 1 page  : Throw out a contrast / question / hard data to stop the audience
Context           → 1-2 pages: Explain background / who you are / why this topic
Core              → 3-5 pages: Core content, intersperse with Layout 4/5/6/9/10
Shift             → 1 page  : Break expectations / present a new perspective
Takeaway          → 1-2 pages: Quotable line / suspense question / call to action
```

Narrative arc + page count plan + theme rhythm table (see `layouts.md`) — **align all three tables** before moving to Step 2.

It's recommended to save the outline as `project-notes.md` or `outline-v1.md` for future iterations.

#### Image Conventions (inform the user)

Explain these to the user before starting:

- **Folder location**: Under `project/XXX/ppt/images/` (same level as `index.html`)
- **Naming convention**: `{page-number}-{semantic}.{ext}`, e.g., `01-cover.jpg` / `03-figma.jpg` / `05-dashboard.png`
  - Zero-pad page numbers for sorting
  - Use English for the semantic part — short, specific, matching the content
- **Specs**:
  - Single image ≥ 1600px wide (avoid blur on large screens)
  - JPG for photos/screenshots, PNG for transparent UI/charts
  - Keep total size under 10MB (affects swipe fluidity)
- **How to replace**: **Same-name overwrite** is the safest approach (no path changes needed in HTML); if the filename changes, do a global search for `images/old-name` and replace with the new name
- **No images?**: Align with the user — you can generate the structure with placeholder color blocks first and add images later; but inform them that image-text layouts like layout 4/5/10 can't be visually verified without images

#### Codex Image Generation (Optional)

If the current runtime is **Codex**, after completing the deck draft, proactively ask the user whether they want to use GPT Image 2 to generate images and insert them into the PPT. Do not generate by default.

Recommended phrasing:

> Want to generate some images for this PPT? Options include documentary-style photos, magazine-style infographics, process/comparison/system diagrams, or redesigning screenshots into a unified magazine visual style.

If the user confirms, ask which image type or style they want; if the user has no preference, recommend 1-3 images worth generating based on page content.

When generating images, follow these rules:

- Keep prompts short — only frame the subject, purpose, style, and aspect ratio. Do not write lengthy photography direction
- Image style must match the current deck style: Style A uses "Digital Magazine × E-Ink"; Style B uses "Swiss International Style / Swiss Style"
- Text in infographics, charts, and screenshot redesigns must match the user's language; Chinese deck uses Chinese, English deck uses English
- First check `references/image-prompts.md` for image types and base prompts
- Image aspect ratio must match the final placement: hero visual 16:9, left-text-right-image 16:10 / 4:3, infographic 16:9 / 16:10, screenshot redesign 16:10, mixed image-text small image 3:2 / 3:4, grid images with uniform height cropping
- Place generated images under `images/`, following the `{page-number}-{semantic}.{ext}` naming convention

### Step 2 · Copy the Template

**Based on the style chosen in Step 1, copy the corresponding template** to the target location (typically `project/XXX/ppt/index.html`), and create an `images/` folder at the same level to hold images.

```bash
mkdir -p "项目/XXX/ppt/images"

# Style A · Digital Magazine
cp "<SKILL_ROOT>/assets/template.html" "项目/XXX/ppt/index.html"

# Or Style B · Swiss International Style
cp "<SKILL_ROOT>/assets/template-swiss.html" "项目/XXX/ppt/index.html"
```

Both `template*.html` files are **fully runnable** — CSS, WebGL shader, swipe JS, font/icon CDN are all pre-configured. Only the `<!-- SLIDES_HERE -->` placeholder awaits your slide content.

**Note**: Styles A and B **cannot be mixed**. Classes in layouts.md (like `.h-hero` serif heading, `.display-zh`, etc.) are only defined in template.html; classes in layouts-swiss.md (like `.kpi-hero`, `.accent-block`, `.span-N`, `.dots`, etc.) are only defined in template-swiss.html. A single deck can only use one set.

#### 2.1 · Required Placeholder Replacements (**easy to miss**)

Replace the following placeholders immediately after copying, otherwise the browser tab will display embarrassing text like "[Required] Replace with PPT title":

| Location | Original | Replace With |
|----------|----------|--------------|
| `<title>` | `[必填] 替换为 PPT 标题 · Deck Title` | Actual deck title (e.g., `一种新的工作方式 · Luke Wroblewski`) |

After every template copy, the first thing to do: grep for "[必填]" to confirm everything has been replaced.

#### 2.2 · Select a Theme Color (5 presets · no custom colors allowed)

This skill **only allows choosing from 5 carefully curated presets** — custom hex values are not accepted. A bad color combination instantly ruins the visual; protecting aesthetics matters more than offering freedom.

| # | Theme | Best For |
|---|-------|----------|
| 1 | 🖋 Ink Classic | General purpose / business launches / default when unsure |
| 2 | 🌊 Indigo Porcelain | Tech / research / data / product launches |
| 3 | 🌿 Forest Ink | Nature / sustainability / culture / non-fiction |
| 4 | 🍂 Kraft Paper | Nostalgia / humanities / literature / indie magazine |
| 5 | 🌙 Dune | Art / design / creative / gallery |

**Steps**:
1. Recommend a set based on content theme, or ask the user to choose
2. Open `references/themes.md`, find the corresponding theme's `:root` block
3. **Replace in bulk** the lines marked with "theme color" comments in the `:root{` block at the top of `assets/template.html` (the copied version) (`--ink` / `--ink-rgb` / `--paper` / `--paper-rgb` / `--paper-tint` / `--ink-tint`)
4. All other CSS uses `var(--...)`, no other changes needed

**Hard rules**:
- One deck, one theme — do not switch colors midway
- Do not accept arbitrary hex values from the user — politely decline and show the 5 options
- Do not mix-and-match (e.g., ink from Ink Classic, paper from Dune) — it will look completely off

### Step 3 · Fill in Content

#### 3.0 · Pre-flight: Class names must be defined in the template's `<style>` (**most important**)

**This is the root cause of all generation issues**. Layout skeletons use many class names — if the template's `<style>` doesn't have matching definitions, the browser falls back to defaults: heading fonts wrong, cards crammed together, pipelines collapsed into a single line, images piled at the page bottom.

**Class names are NOT interchangeable between styles** (emphasis again):
- Style A template has `h-hero` (serif), `stat-card`, `grid-2-7-5`, `frame`, etc.
- Style B template has `h-hero` (sans-serif), `kpi-hero`, `accent-block`, `span-N`, `dots`, `grid-12`, etc.
- Same-name classes render **completely differently** across templates (e.g., Style A's `h-hero` is Noto Serif SC serif; Style B's `h-hero` is Inter sans-serif)

**Before writing any slide code:**

1. **Read the current template first** (at least through the end of the `<style>` block):
   - Style A → `assets/template.html`
   - Style B → `assets/template-swiss.html`
2. **Cross-reference the Pre-flight checklist in the corresponding layouts file** to confirm every class you plan to use exists in `<style>`
3. If a class is missing: **add it to the template's `<style>`** — do not inline-rewrite it in each slide
4. **The template is the single source of truth for class names** — do not invent new class names; if you need custom styling, use `style="..."` inline

**Style A commonly missed classes**:
`h-hero` / `h-xl` / `h-sub` / `h-md` / `lead` / `kicker` / `meta-row` / `stat-card` / `stat-label` / `stat-nb` / `stat-unit` / `stat-note` / `pipeline-section` / `pipeline-label` / `pipeline` / `step` / `step-nb` / `step-title` / `step-desc` / `grid-2-7-5` / `grid-2-6-6` / `grid-2-8-4` / `grid-3-3` / `grid-6` / `grid-3` / `grid-4` / `frame` / `frame-img` / `img-cap` / `callout` / `callout-src` / `chrome` / `foot`

**Style B commonly missed classes** (post-2026-05 refactor):
- Canvas: `canvas-card` / `chrome-min`
- Typography: `h-hero` (sans-serif 7.4vw weight 200) / `h-statement` (9.6vw) / `h-xl` / `h-md` / `t-cat` (SemiBold 600 category label) / `t-meta` (mono uppercase) / `lead` / `num-mega` / `mono`
- Cards (four mutually exclusive types): `card-ink` / `card-accent` / `card-fill` / `card-outlined`
- Grids: `grid-12` / `grid-2-9` / `grid-2-9-5` / `span-N`
- Timelines: `timeline-v` + `tl-node` + `tl-axis` + `dot` / `timeline-h` + `tl-h-node` + `tl-h-axis`
- Charts: `kpi-tower-row` + `bar-tower` / `h-bar-chart` + `bar-row` + `bar-fill` / `spec-bars` + `bar-vert`
- Decorations: `dot-mat` (SVG mask solid dots) / `ring-mat` (stroke circles) / `cross-mat` (× grid) / `hr-hairline`
- Layout-specific: `cover-split` / `closing-split` / `duo-compare` + `vrule` / `manifesto-top` + `ink-banner-full` / `three-forces` / `loop-diagram` / `matrix-fill` + `matrix-cell` / `brief-grid` + `brief-card` / `system-diagram` / `why-now-grid` / `four-cards` / `stacked-ledger` + `ledger-row` / `tech-spec` / `image-hero` + `hero-img-wrap` + `hero-overlay-block` + `hero-stats`
- Image mixing: `frame-img` / `fit-contain` / `r-21x9` / `r-16x9` / `r-16x10` / `h-22` / `h-26` / `swiss-img-split` / `swiss-img-grid` / `swiss-img-caption` / `swiss-keyline` / `swiss-lined`
- Spacing tokens: `--sp-3`...`--sp-13` (8/12/16/24/32/40/48/64/80/96/160 px)

#### 3.0.5 · Plan Theme Rhythm (**equally important as class pre-flight**)

**Before picking layouts**, you must list each page's theme class (`hero dark` / `hero light` / `light` / `dark`) and write them in a document or draft to align. Detailed rules are in the "Theme Rhythm Planning" section at the top of `references/layouts.md`.

**Mandatory rules**:

- Every page's section must include one of `light` / `dark` / `hero light` / `hero dark` — do not just write `hero`
- More than 3 consecutive pages with the same theme = visual fatigue — not allowed
- Decks with 8+ pages must have ≥1 `hero dark` + ≥1 `hero light`
- The entire deck cannot be only `light` body pages — there must be `dark` body pages to create breathing room
- Insert 1 hero page every 3-4 pages (cover / chapter divider / question / big quote)

**Post-generation self-check**: `grep 'class="slide' index.html` to list all themes, visually confirm the rhythm is reasonable before delivery.

#### 3.1 · Pick a Layout

**Do not write slides from scratch**. Open the corresponding layouts file — it contains 10 ready-made layout skeletons, each a complete paste-ready `<section>` code block.

**Style A** → `references/layouts.md`:

| Layout | Purpose |
|---|---|
| 1. Opening Cover | Page 1 |
| 2. Chapter Divider | Opening of each act |
| 3. Data Hero | Presenting hard data |
| 4. Left Text + Right Image (Quote + Image) | Identity contrast / storytelling |
| 5. Image Grid | Multi-image comparison / screenshot evidence |
| 6. Two-Column Pipeline | Workflow |
| 7. Suspense Closer / Question Page | End of act / closing |
| 8. Big Quote Page | Serif quotable line / takeaway |
| 9. Side-by-Side Comparison (Before / After) | Old model vs new model |
| 10. Image + Text (Lead Image + Side Text) | Information-dense image-text page |

**Style B** → First read `references/swiss-layout-lock.md`, then read `references/layouts-swiss.md`.

Swiss theme defaults to **Swiss locked mode**:

- Body pages can only use the 22 layouts registered in the original reference PPT: `S01-S22`; new cover/closing pages can only use the explicitly provided `SWISS-COVER-ASCII` / `SWISS-CLOSING-ASCII`.
- Every `<section class="slide">` must include `data-layout="Sxx"`. Missing `data-layout` is treated as an unregistered layout.
- Inventing ad-hoc layouts like `P23/P24`, `Swiss Image Split`, `Evidence Grid` beyond the original 22 pages is not allowed, unless the user explicitly requests experimental layouts.
- Top Chinese headings default to left-aligned on the upper-left content axis. Do not place subheadings in the left column and main headings in the right column, creating visual centering; only original statement/split layouts allow strong center-driven narrative.
- SVG handles geometry only. Do not put text labels in SVG — use HTML grid/card/caption for all labels instead.
- Geography/history/city routes/location relationship pages use `S08 + Swiss Map Component`: first read `references/swiss-map-component.md`, and keep `data-layout="S08"`.

The original 22 body layouts:

| Layout | Purpose |
|---|---|
| S01 Index Cover | Original index cover |
| S02 Vertical Timeline + KPI | Evolution comparison / era transitions |
| S03 Split Statement | Core thesis / left-right split screen |
| S04 Six Cells | 6 concept definitions |
| S05 Three Layers | Three-tier architecture |
| S06 KPI Tower | 4-item data visualization with height contrast |
| S07 H-Bar Chart | 5-10 item ranking comparison |
| S08 Duo Compare | Before/After comparison |
| S09 Dot Matrix Statement | Big quote / statement |
| S10 Split Closing | Closing page |
| S11 Horizontal Timeline | 4-7 step process |
| S12 Manifesto + Ink Banner | Interim conclusion |
| S13 Three Forces | 3 equal-weight concepts deep-dive |
| S14 Loop Form | Self-learning loop / automation |
| S15 Matrix + Hero Stat | 8-12 item matrix + headline stat |
| S16 Multi-card Brief | 6-item news brief cards |
| S17 System Diagram | Three-tier architecture / ecosystem map |
| S18 Why Now | Three arguments + data support |
| S19 Four Cards | 4 equal-weight features |
| S20 Stacked KPI Ledger | Vertical ledger-style data |
| S21 Tech Spec Sheet | Product specs / benchmarks |
| S22 Image Hero | 21:9 top image + title block + three-column KPI |

**Registered extension**: `S08 + Swiss Map Component` is used for locations, residences, routes, and city relationships. It is not a new layout but a MapLibre map component for the right slot of S08; must be implemented following `references/swiss-map-component.md` for markers, connections, cards, and top-right zoom/drag controls.

Pick the matching layout, paste it in, and edit the copy and image paths. **Make sure to complete the 3.0 pre-flight first**.

**Style B layout diversity hard rules**:
- 7-8 page decks must use at least **6 different S-number layouts**; 10+ page decks must use at least 8 different layouts.
- If the user says "test the template / see how it looks / more layout variety", you must cover: one cover, one closing, at least 1 comparison or timeline (S08/S11/S02), at least 1 structural diagram (S14/S17/S15), and at least 1 image layout (S22 or S15/S16 grid adaptation).
- No more than 3 consecutive pages with the same main structure (e.g., three straight pages of `head + grid + card`).
- Image pages must not use improvised new structures. For 2-3 images, adapt the original grid skeleton of S15/S16 into image slots; for a single large image, use S22.
- Before writing HTML, draft a table of `page number → data-layout → selection rationale → image slots`; before delivery, run `node <SKILL_ROOT>/scripts/validate-swiss-deck.mjs index.html`.

#### 3.2 · Image Aspect Ratio Standards

Always use **standard aspect ratios** — never use the source image's odd ratios (like `2592/1798`):

| Scenario | Recommended Ratio |
|----------|-------------------|
| S22 top hero image | **21:9**; keep photo's key subject in the center safe zone |
| S15/S16 multi-image grid | Uniform 21:9 or uniform 16:10 — do not mix |
| Left-text-right-image main image (Style A) | 16:10 or 4:3 + `max-height:56vh` |
| Image grid (Style A) | **Fixed `height:26vh`** — do not use aspect-ratio |
| Left small image + right text | 1:1 or 3:2 |
| Full-screen hero visual | 16:9 + `max-height:64vh` |
| Mixed image-text small illustration | 3:2 or 3:4 |

**Do not let images `align-self:end` by default** — they will slide to the page bottom and easily overlap the pagination component. Use grid container + `align-items:start` (pre-set in the template) to keep images top-aligned; only Style B's P23 can use `.swiss-img-split.align-image-bottom`, because the template has a built-in `--nav-safe-bottom` safe zone for it.

**Style B Swiss style additional rules**:
- Single large image uses S22; multi-image testing uses the original card grid of S15/S16 — do not use unregistered P23/P24
- Before generating images, write `data-image-slot`: e.g., `s22-hero-21x9` / `s15-grid-21x9` / `s16-brief-21x9`
- S22 images default to 21:9; the prompt must include `subject centered in the safe middle area`; photo container uses `object-position:center 35%`, not `top center`
- Image containers must be sharp-cornered, no shadows, no rounded corners; default background uses white `var(--paper)` — do not use gray background with white-background infographics
- White-background GPT infographics/flowcharts/UI images should not have border strokes by default; do not casually apply `.swiss-keyline`; when emphasis is needed, only use `.swiss-lined` top accent line
- For UI/infographic images that are user-submitted raw screenshots or text-dense, use `.fit-contain`; if regenerated to fit S15/S16 slots, must use `.frame-img.r-21x9` / `.frame-img.r-16x10` to fill the container — do not fix `height:18vh` and shrink the image
- Multiple images in the same group must share uniform slot, ratio, and height — no mixing
- GPT Image 2 generated images follow the "Style B: Swiss International Style Image Rules" in `image-prompts.md`
- The bottom edge of any image, caption, timeline label, or footnote must not enter the bottom pagination zone; when content needs to be near the bottom, use `.nav-safe-bottom` / `.nav-safe-bottom-tight` — do not hand-write `bottom:2vh`

#### 3.2.1 · Chinese Heading Font Size Tiers (mandatory for Style B)

Chinese characters have larger visual mass — you cannot directly apply the English hero's 6.8-7vw. Before writing Chinese headings, determine the tier:

| Heading Type | Recommended Font Size |
|---|---|
| 1 line, ≤ 8 Chinese characters | `min(6.4vw,11.2vh)` |
| 2 lines, each ≤ 8 Chinese characters | `min(5.8vw,10.2vh)` |
| 2 lines, any line 9-12 Chinese characters | `min(5.2vw,9.2vh)` |
| 3 lines or longer | Rewrite the heading first; as a last resort use `min(4.6vw,8.2vh)` |

If the heading crowds out the image or body area, shorten the heading copy first, then reduce font size; do not push content below to force-fit.

Component details (fonts, colors, grids, icons, callouts, stat cards, etc.) are in `references/components.md`.

### Step 4 · Self-Check Against the Checklist

After generation, always open `references/checklist.md` and verify item by item. It summarizes **every pitfall encountered during real iterations** — P0-level issues (emoji, image overflow, heading line breaks, font assignment) must all pass.

#### 4.0 · Don't just review code: open the page for visual verification

Code can only prove class names and structure exist — it cannot prove the layout looks good. After generation, you must open the page and review each slide:

1. Open the original reference PPT, the current template or generated page, and the test PPT side by side; the original reference is `/Users/guohao/Documents/op7418的仓库/项目/Thin-Harness-Fat-Skills/ppt/index.html`.
2. Wait for entrance animations to settle before screenshotting (about 1-2 seconds) — do not mistake mid-animation states for layout issues.
3. Check visuals first: heading weight, heading-to-content spacing, whether images align with body text, whether images/captions overlap the bottom pagination component.
4. Then check code: confirm the chosen layout matches the content shape — don't use a data-specific layout for concept explanations, and don't pile optional components as decoration.
5. When comparing against the original reference template, base judgments on actual page usage, not just CSS helper definitions; the original pages' large text is mostly weight 200/300 in practice — don't be misled by raw CSS showing 700/800/900.
6. If a page feels off, first determine whether it's a wrong layout choice, missing required components, overused optional components, or spacing/safe-zone issues — don't just add margins as a quick fix.

#### Style A · Digital Magazine Mandatory Checks

1. **Headings must be serif** — if they render as sans-serif, 99% of the time Step 3.0 pre-flight was skipped and the `h-hero` class is missing from template.html
2. **Image grids use only `height:Nvh`, not `aspect-ratio`** (will overflow)
3. **Images must not pile up at the page bottom** — do not use `align-self:end`, use grid + `align-items:start` (see Step 3.2)
4. **Images must use standard ratios only** (16:10 / 4:3 / 3:2 / 1:1 / 16:9) — do not copy the source image's odd ratios
5. **Chinese headings ≤ 5 characters with `nowrap`** (prevent 1-character-per-line wrapping)
6. **Use Lucide, not emoji**
7. **Headings use serif, body uses sans-serif, metadata uses monospace**

#### Style B · Swiss International Style Mandatory Checks

1. **Sans-serif throughout** — any serif font appearing is wrong (check that `font-family` does not use `--serif` type variables)
2. **Only one accent color** — a single deck cannot have IKB blue + lemon yellow + safety orange or other multiple highlight colors simultaneously
3. **No gradients / shadows / rounded corners** — all color blocks are sharp-cornered solid fills; any `box-shadow` / `linear-gradient` / `border-radius` > 0 must be removed (except rule lines)
4. **Extreme type-size contrast** — heading-to-body ratio ≥ 8:1
5. **Large font sizes must have dual-constraint height capping** — `font-size:min(Xvw, Yvh)`; using only vw will overflow on standard 16:9 screens (lesson from P15/P20/P22)
6. **Large text weight 200** (ExtraLight) — the bigger the type, the thinner the weight; this is the soul of Swiss style. **Forbidden**: 600/700/800 on large text
7. **Card fill types are mutually exclusive** — `card-ink` / `card-accent` / `card-fill` / `card-outlined` **cannot be mixed** (no "blue fill + blue border", "gray fill + border", etc.)
8. **Uniform style for parallel cards** — 3-12 cards use the same type (prefer `card-fill` gray); to highlight a single item, switch only that one to `card-accent`, and **only one** is allowed
9. **Sharp corners everywhere** — no `border-radius` allowed; decorative elements use 8×8 sharp-cornered small squares, **not** 9px round dots
10. **Use Lucide for icons, don't hand-draw SVG** — `<i data-lucide="name"></i>` + `lucide.createIcons()`; choose angular styles (avoid round/bulky)
11. **Timeline alignment** — axis column fixed at 12px + dot absolutely positioned; **do not** use grid `justify-self` (causes misalignment with dashed lines)
12. **Section-level heading to content spacing ≥ 9vh** — avoid crowding (lesson from P15/P16)
13. **One semantic animation recipe per page** — not a uniform fade-up everywhere; numbers scale-bounce in, bars scaleY pull up, SVG strokes draw, nodes light up in sequence, etc. **Forbidden**: using the same generic recipe across all pages
14. **playSlide entry reveal container** — `[data-anim]` containers must first force opacity:1, then the recipe uses motion `{opacity:[0,1]}` to override; otherwise some pages will be "invisible"
15. **ESC index page visibility** — cloned slides must have a CSS override that sets `[data-anim]` to opacity:1 in thumbnails
16. **Helvetica/Inter Chinese font fallback** — Windows users don't have "PingFang SC"; must fall back to `"Microsoft YaHei UI", "Noto Sans SC"`
17. **Font weight conventions**: large text 200 / body 300 / `t-cat` SemiBold 600 / `t-meta` mono uppercase
18. **Preserve low-power shortcut** — bottom-right corner must display `B Static`; pressing `B` toggles `body.low-power`, stopping WebGL/ASCII canvas RAF and Motion entrance animations. Templates also respect `prefers-reduced-motion: reduce` — renders one frame then freezes all animation. Animations and WebGL automatically pause when the browser tab is hidden (Page Visibility API)
19. **Decorative elements strictly within the grid** — bar matrices, dot patterns, ring-mat must not bleed to the edge or overflow the page
20. **Reserve bottom nav space** — nav sits at ~97vh; content must not extend past 93vh (lesson from P22 KPI large text overflowing)
21. **Image containers: sharp corners, no shadows** — `.frame-img` gets no `border-radius` / `box-shadow`; borders use hairlines only
22. **P23/P24 images in the same group must be consistent** — uniform ratio, height, margins, and line thickness within a group; infographics/UI images add `.fit-contain`
23. **Component roles must be correct** — P23/P24 captions are required information anchors; P22 KPI/descriptions are required; data-specific layouts must have real data — don't fill them with copy text
24. **Distinguish general vs. specialized layouts** — P3/P8/P11/P19/P23 are versatile; P6/P7/P20/P21/P22 are data/case-specific; P14/P15/P17 are structure-specific

### Step 5 · Local Preview

Just open `index.html` in a browser. On macOS:

```bash
open "项目/XXX/ppt/index.html"
```

No local server needed. Images use relative paths `images/xxx.png`.

### Step 6 · Iterate

Modify based on user feedback — the template's CSS is highly parameterized; 90% of adjustments are inline style changes (font size `font-size:Xvw` / height `height:Yvh` / gap `gap:Zvh`).

---

## Resource File Guide

```
guizang-ppt-skill/
├── SKILL.md                  ← You are reading this
├── assets/
│   ├── template.html         ← Style A · Digital Magazine template (seed file)
│   ├── template-swiss.html   ← Style B · Swiss International Style template (seed file)
│   └── motion.min.js         ← Motion local copy (offline fallback, ~64KB, shared)
├── scripts/
│   └── validate-swiss-deck.mjs ← Style B static validation: registered layouts, image slots, SVG text, heading alignment
└── references/
    ├── components.md         ← Component manual (fonts, colors, grids, icons, callouts, stats, pipelines, animations... for Style A)
    ├── layouts.md            ← Style A · 10 page layout skeletons (ready to paste, with animation markers)
    ├── swiss-layout-lock.md  ← Style B · Original 22P layout lock; body pages must follow registrations here
    ├── layouts-swiss.md      ← Style B · Original 22P skeleton docs + clearly marked experimental sections
    ├── swiss-map-component.md ← Style B · S08 map extension component (MapLibre markers/connections/cards/controls)
    ├── themes.md             ← Style A · 5 theme color presets (choose only, no customization)
    ├── themes-swiss.md       ← Style B · 4 Swiss theme color presets (IKB / lemon yellow / lime green / safety orange)
    ├── image-prompts.md      ← GPT Image 2 image types, ratios, and base prompts
    └── checklist.md          ← Quality checklist (P0/P1/P2/P3 severity levels)
```

**Recommended loading order**:
1. Read all of `SKILL.md` (this file) first for the overview
2. Step 1 requirements clarification — **first question** determines Style A or B, then:
   - Style A: read `themes.md` to help the user pick a theme color
   - Style B: read `themes-swiss.md` to help the user pick a theme color
3. **Read the corresponding template's `<style>` block before starting** — this is the single source of class names; missing classes will break the entire page's styling
   - Style A → `assets/template.html`
   - Style B → `assets/template-swiss.html`
4. Read the corresponding layouts file to pick layouts:
   - Style A → `layouts.md` (top section has Pre-flight class checklist, theme rhythm planning, animation recipe decision tree)
   - Style B → **Read `swiss-layout-lock.md` first**, then read `layouts-swiss.md`; body pages must be chosen from S01-S22, each page needs `data-layout`
5. If Style B needs location, route, residence, or city relationship maps, read `swiss-map-component.md`
6. If generating images in Codex, read `image-prompts.md` for image types, ratios, and base prompts
7. For detail adjustments, read `components.md` for components (includes the Motion animation system chapter, primarily for Style A; Style B component details are in the `layouts-swiss.md` appendix)
8. After generation, first run `node scripts/validate-swiss-deck.mjs path/to/index.html`, then read `checklist.md` for self-check

**Animation notes**: The template has Motion's loader and recipe logic embedded in the bottom module script. You don't need to edit JS — just add `data-anim` / `data-animate` in the HTML following the skeletons in `layouts.md` / `layouts-swiss.md`. Offline presentations rely on `assets/motion.min.js`, which gracefully degrades to "no animation but readable content" when disconnected. The Style B template must preserve the `B` key low-power mode: when toggled, it stops WebGL/ASCII canvas RAF, cancels running Web Animations, and reveals current page content directly to its static final state.

## Core Design Principles (Philosophy)

### Style A · Digital Magazine (summarized from 5 rounds of iteration)

> Violating any one of these will break the magazine feel.

1. **Restraint over showmanship** — WebGL background only shows through on hero pages; barely visible on regular pages
2. **Structure over decoration** — no shadows, no floating cards, no padding boxes; all information hierarchy through **large type + font contrast + grid whitespace**
3. **Content hierarchy defined by both size and typeface** — largest serif = main heading, medium serif = subheading, large sans-serif = lead, small sans-serif = body, monospace = metadata
4. **Images are first-class citizens** — images only crop from the bottom, ensuring top and sides remain intact; grids use `height:Nvh` fixed, never `aspect-ratio` to stretch
5. **Rhythm comes from hero pages** — alternating hero and non-hero pages keeps eyes from tiring
6. **Consistent terminology** — Skills means Skills; do not mix Chinese-English translations

### Style B · Swiss International Style

> Violating any one of these will instantly downgrade the design from Swiss to PowerPoint.

1. **Single accent color** — one deck uses only one accent; multiple highlight colors are not allowed
2. **Extreme type-size contrast** — heading-to-body ratio ≥ 8:1; KPI must be a "Data Hero" (18-22% of screen width)
3. **Sans-serif and nothing else** — Inter / Helvetica / Noto Sans SC; any serif is wrong
4. **Sharp corners, solid fills** — no gradients / shadows / rounded corners (except rule lines)
5. **Grid above all** — all elements snap to a 12-col grid; left-aligned + generous whitespace for asymmetric aesthetics
6. **Hairlines are scalpels** — 1px ultra-thin dividers are enough; do not thicken, do not add shadows
7. **Dot matrix decoration only on hero pages** — body pages stay clean with solid backgrounds

## Reference Works

The two styles in this skill were respectively inspired by:

**Style A · Digital Magazine**:
- 歸藏 Guizang "One-Person Company: Organizations Folded by AI" talk (2026-04-22, 27 pages)
- *Monocle* magazine layouts
- YC President Garry Tan's "Thin Harness, Fat Skills" blog post demo

**Style B · Swiss International Style**:
- Massimo Vignelli's NYC Subway / Unimark system
- *Helvetica Forever*'s typographic design language
- Josef Müller-Brockmann's classic works on grid systems
- Contemporary design: Acne Studios / Off-White / IKEA / Beck Design

Use these as style anchors.
