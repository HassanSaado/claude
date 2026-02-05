---
name: refactoring-ui
description: >
  Comprehensive UI design system and principles for building polished, professional interfaces.
  Use this skill whenever building or styling frontend components, pages, layouts, dashboards,
  forms, cards, navigation, tables, modals, buttons, or any user-facing HTML/CSS/React/Tailwind.
  Triggers include: creating new UI, reviewing or improving existing UI, choosing colors, spacing,
  typography, shadows, or layout strategies, fixing designs that look "off", and building design
  systems or component libraries. Apply these principles by default when generating any frontend code.
---

# Refactoring UI — Design System Skill

## Quick Reference: Top 15 Rules (Always Follow)

1. **Use a constrained spacing scale.** Never pick arbitrary values. Use: 4, 6, 8, 12, 16, 24, 32, 48, 64, 80, 96, 128, 160, 192, 224, 256 (in px or Tailwind equivalents).
2. **Establish visual hierarchy with weight and color, not just size.** Use font weight (400/500 for normal, 600/700 for emphasis) and color (dark/medium/light grey) before reaching for larger font sizes.
3. **Start with too much white space, then reduce.** Generous spacing always looks more professional than cramped layouts.
4. **Limit font sizes to a defined type scale.** Use: 12, 14, 16, 18, 20, 24, 30, 36, 48, 60, 72 (px). Never use arbitrary sizes like 13px or 15px.
5. **Start with a feature, not a layout.** Don't design the "shell" (nav/sidebar) first; design the core functionality and let it dictate the container.
6. **Design in grayscale first.** Use spacing, contrast, and size to do the heavy lifting before introducing color.
7. **Don't use grey text on colored backgrounds.** Instead, use a same-hue, lower-saturation/higher-lightness color, or use semi-transparent white only as a last resort.
8. **Primary actions = solid high-contrast buttons. Secondary = outlined or muted. Tertiary = links.** Always reflect action hierarchy in button styling.
9. **Line length: 45–75 characters.** Set `max-width: 65ch` (or Tailwind `max-w-prose`) on paragraph text.
10. **Use fewer borders.** Prefer box shadows, background color changes, or extra spacing to separate elements.
11. **Shadows convey elevation, not decoration.** Define 5 shadow levels (xs through 2xl) and use them to indicate z-axis position.
12. **Labels are a last resort.** Omit labels when data format is self-evident (email, phone, price, dates) or combine them into natural phrases ("12 left in stock" not "In stock: 12").
13. **Overlap elements to create depth.** Offset cards, images, and panels so they cross background boundaries.
14. **Avoid ambiguous spacing.** Space between groups must be noticeably larger than space within groups so relationships are clear.
15. **Don't overlook empty states.** Use illustrations and clear CTAs to guide users when no content exists — empty states are a user's first interaction with a new feature.

---

## 0. Starting From Scratch — Process & Mindset

### Start with a Feature, Not a Layout

Don't begin by designing the navigation bar or the "shell" of the app. You don't have enough information yet to decide on navigation structure. Instead, start with a piece of actual functionality — the search form, the dashboard metric, the comment thread.

An app is a collection of features. **Design one feature at a time**, then figure out the shell and navigation once you understand what needs to fit inside it.

### Detail Comes Later

In the earliest stages, don't get hung up on typefaces, shadows, or icons. That stuff matters eventually, but not right now.

**Design on paper first.** Jason Fried of Basecamp recommends designing with a thick Sharpie — you literally cannot obsess over little details with such a blunt instrument, which makes it a great tool for quickly exploring layout ideas.

**Hold the color.** Even when you move to higher fidelity, resist introducing color right away. By designing in grayscale, you're forced to use spacing, contrast, and size to do all of the heavy lifting. This produces a clearer interface with a strong hierarchy that's easy to enhance with color later.

**Don't over-invest in mockups.** Sketches and wireframes are disposable — users can't do anything with static mockups. Use them to explore your ideas, and leave them behind when you've made a decision.

### Work in Cycles

Don't try to design every feature before moving to implementation. Work in short cycles:

1. Design a simple version of the next feature
2. Build it
3. Iterate on the working design until problems are solved
4. Jump back to design for the next feature

It's a lot easier to fix design problems in an interface you can actually use than it is to imagine every edge case in advance.

### Be a Pessimist

Don't imply functionality in your designs that you aren't ready to build. If you plan for attachments in a comment system but can't ship them yet, the entire comment feature sits unfinished. **A comment system with no attachments is still better than no comment system at all.** Design the smallest useful version you can ship to reduce risk.

### Shrink the Canvas

If you're having a hard time designing a small interface on a large canvas, **shrink the canvas.** It's much easier to design something small when the constraints are real.

Start with a ~400px canvas and design the mobile layout first. Once you're happy with that, bring it to a larger screen size. You'll be surprised how little needs to change.

```html
<!-- Mobile-first approach: start narrow, expand later -->
<div class="w-full max-w-sm mx-auto px-4">
  <!-- Design this first at 400px -->
</div>

<!-- Then adapt for larger screens -->
<div class="w-full max-w-sm md:max-w-2xl lg:max-w-5xl mx-auto px-4">
  <!-- Adjust only what felt like a compromise on mobile -->
</div>
```

### Don't Force It

Just as you shouldn't worry about filling the whole screen, don't try to cram everything into a small area unnecessarily either. If you need a lot of space, go for it. Just don't feel obligated to fill it if you don't have to.

---

## 1. Hierarchy & Visual Weight

### The Core Principle

Visual hierarchy is the single most important factor in making a UI look "designed." Not all elements are equal — deliberately de-emphasize secondary and tertiary information so primary content stands out.

### Size Isn't Everything

**Never rely on font size alone for hierarchy.** Oversized primary text + undersized secondary text both hurt readability.

Instead, combine three tools:
- **Color** — 2–3 levels: dark (primary), medium grey (secondary), light grey (tertiary)
- **Weight** — 2 levels: normal (400–500) for body, heavy (600–700) for emphasis
- **Size** — Use sparingly, as a supplemental signal

```css
/* Three-level text hierarchy */
.text-primary   { color: #1a1a2e; font-weight: 500; }
.text-secondary  { color: #6b7280; font-weight: 400; }
.text-tertiary   { color: #9ca3af; font-weight: 400; }
```

```html
<!-- Tailwind equivalent -->
<h2 class="text-gray-900 font-semibold text-lg">Primary content</h2>
<p class="text-gray-500 text-sm">Secondary content</p>
<span class="text-gray-400 text-xs">Tertiary content</span>
```

**Never use font weights below 400 for UI text.** Light/thin weights (100–300) are only acceptable for very large headings (36px+). At body sizes, they're unreadable.

### Emphasize by De-emphasizing

When a primary element doesn't stand out enough, don't try to make it louder. Instead, make everything else quieter.

- Soften the color of inactive nav items instead of brightening the active one
- Remove background color from sidebars that compete with main content
- Reduce contrast on supporting icons and metadata

This applies to bigger pieces of an interface too. If a sidebar feels like it's competing with your main content area, don't give it a background color — let the content sit directly on the page background instead. The sidebar doesn't need its own visual weight to function.

```html
<!-- BAD: sidebar with its own background competes with main content -->
<div class="flex">
  <aside class="w-64 bg-gray-800 text-white p-4">...</aside>
  <main class="flex-1 p-6">...</main>
</div>

<!-- BETTER: sidebar without heavy background lets main content dominate -->
<div class="flex">
  <aside class="w-64 p-4 text-gray-500">...</aside>
  <main class="flex-1 p-6">...</main>
</div>
```

### Balance Weight and Contrast

Heavy visual elements (bold text, solid icons) cover more surface area and feel emphasized. To balance:
- **Heavy elements that should feel lighter:** Reduce their color contrast (use grey instead of black for icons alongside text)
- **Light elements that should feel heavier:** Increase their weight (a 2px border is less harsh than a darker 1px border)

```html
<!-- Icon next to text: reduce icon contrast to balance -->
<div class="flex items-center gap-2">
  <svg class="text-gray-400 w-5 h-5">...</svg>  <!-- softer than text -->
  <span class="text-gray-700">Settings</span>
</div>
```

### Button Hierarchy

Every page has a pyramid of action importance. Style buttons accordingly:

| Level | Style | Tailwind Example |
|---|---|---|
| **Primary** | Solid, high-contrast background | `bg-blue-600 text-white font-semibold` |
| **Secondary** | Outline or muted background | `border border-gray-300 text-gray-700` or `bg-gray-100 text-gray-700` |
| **Tertiary** | Link-style, no background | `text-gray-500 hover:text-gray-700 underline` |

**Destructive actions are not automatically primary.** A "Delete" button on a settings page should be secondary or tertiary. Reserve big-red-bold styling for the confirmation dialog where the destructive action *is* the primary action.

When you take a hierarchy-first approach to designing the actions on a page, the result is a much less busy UI that communicates more clearly. Don't fall into the trap of designing actions based purely on semantics (e.g., making every "Delete" button big and red). The semantic meaning is important, but **hierarchy always comes first.**

### Labels Are a Last Resort

When displaying data:
1. **Omit labels entirely** when the format is self-evident (email addresses, phone numbers, prices, dates)
2. **Combine label and value** into a natural phrase: "12 left in stock" instead of "In stock: 12", "3 bedrooms" instead of "Bedrooms: 3"
3. **De-emphasize labels** when they're necessary: smaller size, lighter color, lighter weight. The data is what matters.
4. **Emphasize labels** only on info-dense pages where users scan for label names (like tech spec sheets). Even then, don't over-dim the values.

### Separate Visual Hierarchy from Document Hierarchy

Use semantic heading tags (`h1`–`h6`) for accessibility, but **don't let default browser heading sizes dictate visual style.** Section titles in app UIs often function as labels, not headlines — style them small and understated, not large and bold.

Taken to the extreme, you can include section titles in your markup for accessibility but **hide them visually** when the content is self-evident — use `sr-only` (Tailwind) or a visually-hidden utility class.

```html
<!-- Semantic h2, but styled as a small label -->
<h2 class="text-xs font-semibold uppercase tracking-wide text-gray-500">
  Account Settings
</h2>

<!-- Semantic heading present for screen readers, hidden visually -->
<h2 class="sr-only">User Profile</h2>
```

---

## 2. Layout & Spacing

### Start with Too Much White Space

**Always begin with more space than you think you need, then reduce.** The default instinct is to add the minimum whitespace to stop things looking bad. Flip this: start generous, trim until balanced.

Dense UIs (dashboards, data tables) are the exception — but that should be a deliberate choice, never the default.

**Dense UIs have their place.** If you're designing a dashboard where a lot of information needs to be visible at once, packing information together so it all fits on one screen might be worth making the design feel more busy. The important thing is to make this a **deliberate decision** instead of just being the default. It's a lot more obvious when you need to remove white space than it is when you need to add it.

### The Spacing & Sizing Scale

Use a scale where adjacent values differ by at least ~25%. A base-16 system works well. The book's full recommended scale, built from multiples and factors of 16:

```
4  →  8  →  12  →  16  →  24  →  32  →  48  →  64  →  96  →  128  →  192  →  256  →  384  →  512  →  640  →  768
```

The values at the small end are packed together (4, 8, 12, 16) and get progressively more spaced apart as you go up. Each value is a factor or multiple of 16:

```
4px   (16 × 0.25)
8px   (16 × 0.5)
12px  (16 × 0.75)
16px  (16 × 1)
24px  (16 × 1.5)
32px  (16 × 2)
48px  (16 × 3)
64px  (16 × 4)
96px  (16 × 6)
128px (16 × 8)
192px (16 × 12)
256px (16 × 16)
384px (16 × 24)
512px (16 × 32)
640px (16 × 40)
768px (16 × 48)
```

Tailwind mapping:
```
1 (4px) → 2 (8px) → 3 (12px) → 4 (16px) → 6 (24px) → 8 (32px) → 12 (48px) → 16 (64px) → 24 (96px) → 32 (128px) → 48 (192px) → 64 (256px) → 96 (384px) → [512px] → [640px] → [768px]
```
(Values beyond 256px may need custom Tailwind values or arbitrary values like `w-[512px]`.)

**Never use arbitrary values** like 13px, 37px, or 125px. When a spacing decision feels hard, pick from the scale and compare the options on either side.

**Why a linear scale doesn't work:** A system like "make sure everything is a multiple of 4px" doesn't make it any easier to choose between 120px and 125px. For a system to be truly useful, it needs to account for the **relative difference** between adjacent values. At the small end of the scale, jumping from 12px to 16px is a 33% increase — very noticeable. But at the large end, going from 500px to 520px is only 4%, which is eight times less significant. That's why each step in the scale needs to be proportionally bigger as the absolute values increase.

### Don't Fill the Whole Screen

Just because you have 1400px of viewport doesn't mean content should span it. If 600px is the right width, use 600px. Constrain content to what it needs.

```html
<!-- Don't stretch forms to full width -->
<div class="max-w-md mx-auto">
  <form>...</form>
</div>

<!-- Different sections can have different max widths -->
<div class="max-w-2xl mx-auto"> <!-- text content -->
  <p>...</p>
</div>
<div class="max-w-5xl mx-auto"> <!-- wider image/chart area -->
  <img ... />
</div>
```

**When a narrow element feels unbalanced** in a wide container, split it into columns rather than stretching it.

```html
<!-- Instead of a wide form, add a description column -->
<div class="grid grid-cols-3 gap-12">
  <div class="col-span-1">
    <h3 class="text-lg font-medium">Profile</h3>
    <p class="text-sm text-gray-500">Your public information.</p>
  </div>
  <div class="col-span-2">
    <form>...</form>
  </div>
</div>
```

### Grids Are Overrated

Don't force everything into a 12-column percentage grid.

- **Sidebars** should have a fixed width (e.g., `w-64`), not a fluid column (25%). A sidebar doesn't need to grow with the viewport.
- **Main content** flexes to fill the remaining space.
- **Cards and modals** should have a `max-width`, not a column-based percentage width. Let them be their natural size until the screen gets too small.

```html
<!-- Fixed sidebar + flexible main content -->
<div class="flex">
  <aside class="w-64 shrink-0">...</aside>
  <main class="flex-1 min-w-0">...</main>
</div>
```

**Don't shrink elements until you need to.** Give a component a `max-width` and only let it shrink below that on smaller viewports.

Consider a login card: if you give it 50% width (6 columns) on large screens and 66% (8 columns) on medium screens, there's a range where the card is paradoxically **wider on medium screens than on large screens** because column widths are fluid. If 500px is the optimal size for the card, why should it ever get smaller than that if you have the space?

Instead, use `max-width` so it stays at its ideal size and only shrinks when the screen forces it to:

```html
<!-- BAD: fluid percentage widths cause paradoxical sizing -->
<div class="w-1/2 md:w-2/3 mx-auto">
  <div class="bg-white rounded-lg shadow p-8">Login form...</div>
</div>

<!-- GOOD: fixed max-width, only shrinks when necessary -->
<div class="w-full max-w-md mx-auto px-4">
  <div class="bg-white rounded-lg shadow p-8">Login form...</div>
</div>
```

Don't be a slave to the grid — give your components the space they need and don't make compromises until it's actually necessary.

### Relative Sizing Doesn't Scale

Don't assume proportional relationships hold across screen sizes.

- A heading that's 2.5× body size on desktop might need to be only 1.5× on mobile
- Large elements shrink faster than small elements as viewport narrows
- Button padding should be more generous at larger sizes and tighter at smaller sizes — not a fixed em ratio

**Independently tune size, padding, and spacing at each breakpoint** rather than using a single relative unit.

**Relationships within elements:** The idea that things should scale independently doesn't just apply to screen sizes — it applies to the properties of a single component too. If you define button padding in `em` units relative to font size, a large button and a small button will have the same proportions. But that's not what you want — a large button should feel like a **larger button** with more generous padding, and a small button should feel compact with tighter padding, not like you simply zoomed in or out.

```html
<!-- BAD: same proportions via em-based padding — all buttons look like scaled copies -->
<button class="text-xs px-[1em] py-[0.75em]">Small</button>
<button class="text-base px-[1em] py-[0.75em]">Medium</button>
<button class="text-lg px-[1em] py-[0.75em]">Large</button>

<!-- GOOD: padding tuned independently — each size has appropriate proportions -->
<button class="text-xs px-2.5 py-1">Small</button>        <!-- tighter -->
<button class="text-sm px-4 py-2">Medium</button>          <!-- balanced -->
<button class="text-base px-6 py-3">Large</button>         <!-- generous -->
```

As a general rule: **large elements on large screens need to shrink faster than elements that are already small.** The difference between small and large elements should be less extreme at small screen sizes.

### Avoid Ambiguous Spacing

When elements are grouped without a visible separator, **spacing is the only signal of relationships.** The space within a group must be visibly smaller than the space between groups.

```html
<!-- BAD: same gap everywhere — labels feel disconnected from inputs -->
<div class="space-y-4">
  <label>Name</label>
  <input />
  <label>Email</label>
  <input />
</div>

<!-- GOOD: tight gap within group, larger gap between groups -->
<div class="space-y-6">
  <div class="space-y-1.5">
    <label>Name</label>
    <input />
  </div>
  <div class="space-y-1.5">
    <label>Email</label>
    <input />
  </div>
</div>
```

This applies everywhere: form fields, section headings and their content, list items, horizontally arranged elements.

**In article design:** Make sure section headings have more space above them (separating from the previous section) than below them (connecting to their content). If the space above and below a heading is equal, it looks like it belongs to neither section.

**In bulleted lists:** When the space between bullets matches the line-height of a single multi-line bullet, it's hard to tell where one bullet ends and the next begins. Give noticeably more space between bullets than within them.

**In horizontal layouts:** The same mistake is easy to make with components laid out side by side. Icon-plus-label groups, for example, need tighter spacing within the group and wider spacing between groups.

```html
<!-- BAD: ambiguous horizontal spacing — icon could belong to either label -->
<div class="flex gap-4">
  <svg><!-- icon --></svg>
  <span>Dashboard</span>
  <svg><!-- icon --></svg>
  <span>Settings</span>
</div>

<!-- GOOD: tight spacing within groups, wider spacing between groups -->
<div class="flex gap-8">
  <div class="flex items-center gap-2">
    <svg><!-- icon --></svg>
    <span>Dashboard</span>
  </div>
  <div class="flex items-center gap-2">
    <svg><!-- icon --></svg>
    <span>Settings</span>
  </div>
</div>
```

Whenever you're relying on spacing to connect a group of elements, **always make sure there's more space around the group than there is within it** — interfaces that are hard to understand always look worse.

---

## 3. Typography

### The Type Scale

Define a restricted set of font sizes. A practical hand-crafted scale:

```
12px  •  14px  •  16px  •  18px  •  20px  •  24px  •  30px  •  36px  •  48px  •  60px  •  72px
```

Tailwind equivalents:
```
text-xs (12) • text-sm (14) • text-base (16) • text-lg (18) • text-xl (20) • text-2xl (24) • text-3xl (30) • text-4xl (36) • text-5xl (48) • text-6xl (60) • text-7xl (72)
```

**Never use arbitrary sizes** like 13px or 15px. Pick from the scale. If two adjacent sizes both seem wrong, pick the closer one.

### Why Not a Modular Scale?

Some approaches calculate a type scale using a mathematical ratio, like 4:5 ("major third"), 2:3 ("perfect fifth"), or the golden ratio 1:1.618. While the mathematical purity is alluring, this approach has two practical problems:

1. **Fractional values.** A 16px base with a 4:5 ratio produces sizes like 31.25px and 39.063px. Browsers handle subpixel rounding differently, so you'll get inconsistencies across browsers.
2. **Not enough sizes.** A rounded 3:4 scale gives you 12px, 16px, 21px, 28px — but in practice you'll wish you had a size between 12 and 16, and another between 16 and 21. Interface design needs more granularity than long-form content.

A **hand-crafted scale** avoids both problems. You have total control over which sizes exist, there are no rounding errors, and you can ensure the scale has enough options for real interface work.

**Avoid em units** for type scales — nested em values compound and produce sizes outside your scale. Use `px` or `rem`.

### Line Height Is Proportional

Line height and font size are **inversely proportional:**
- **Small body text (14–16px):** line-height ~1.5–1.75
- **Medium text (18–24px):** line-height ~1.4–1.5
- **Large headings (30px+):** line-height ~1.1–1.25
- **Very large display text (48px+):** line-height ~1.0

Line height also depends on line length: wider text blocks need taller line-height (up to 2.0). Narrow columns can use tighter values (1.4–1.5).

```html
<p class="text-sm leading-relaxed">Body text in a wide column</p>   <!-- ~1.625 -->
<p class="text-sm leading-normal">Body text in a narrow column</p>  <!-- ~1.5 -->
<h1 class="text-4xl leading-tight">Large heading</h1>               <!-- ~1.25 -->
<h1 class="text-6xl leading-none">Display text</h1>                 <!-- 1.0 -->
```

### Line Length

**Keep paragraph text to 45–75 characters per line.** Use `max-width` on text containers:

```html
<p class="max-w-prose">...</p>         <!-- Tailwind: ~65ch -->
<p style="max-width: 32em;">...</p>    <!-- CSS: roughly 45-75 chars -->
```

When mixing text with wider elements (images, charts), **constrain only the paragraph width**, not the whole container.

### Alignment

- **Left-align** nearly all body text (for LTR languages)
- **Center-align** only for headlines or short independent blocks (2–3 lines max). If it's too long, rewrite it shorter.
- **Right-align numbers** in tables for easy scanning
- **Justified text** needs `hyphens: auto` to avoid ugly word gaps. Without hyphenation, justified text creates awkward, uneven gaps between words. Justified text works best in situations where you're trying to mimic a print look (online magazine or newspaper). Even then, left-aligned text works great too — it's a matter of preference.

```css
/* If you use justified text, always enable hyphenation */
.justified-text {
  text-align: justify;
  hyphens: auto;
  -webkit-hyphens: auto;
}
```

### Baseline Alignment

When mixing font sizes on the same line (e.g., a title and an action link), **align to baseline**, not vertical center.

```html
<div class="flex items-baseline justify-between">
  <h2 class="text-xl font-bold">Dashboard</h2>
  <a class="text-sm text-blue-600">View all</a>
</div>
```

### Letter Spacing

- **Trust the typeface designer** in most cases
- **Tighten letter-spacing for large headlines** using a wide-spaced body font: `tracking-tight` or `letter-spacing: -0.025em`
- **Increase letter-spacing for ALL CAPS text:** `tracking-wide` or `letter-spacing: 0.05em`

```html
<h1 class="text-4xl font-bold tracking-tight">Welcome Back</h1>
<span class="text-xs font-semibold uppercase tracking-wide text-gray-500">Section</span>
```

### Font Selection

- For UI text, neutral sans-serifs are the safest bet
- **Filter for quality:** only consider typefaces with 5+ weights (10+ styles counting italics). On Google Fonts, filtering to 10+ styles cuts out 85% of options, leaving fewer than 50 sans-serifs to choose from.
- Fonts meant for headlines (tight letter-spacing, short x-height) look bad at small sizes
- Fonts meant for body text (wide letter-spacing, tall x-height) look bland at large sizes
- System font stack as a safe fallback: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif`

### Understand x-Height

When someone designs a font family, they design it for a specific purpose. **x-height** is the height of the lowercase letters (like "x", "n", "e") relative to the overall character size.

- **Headline fonts** (like Futura PT): shorter x-height, tighter letter-spacing — looks great big, but becomes hard to read at small sizes
- **Body/UI fonts** (like Proxima Nova): taller x-height, wider letter-spacing — highly legible at small sizes, but looks bland as a headline

Keep this in mind and **avoid using condensed typefaces with short x-heights for your main UI text.** If you want a headline-style font for headings and a legible font for body text, use two different families rather than trying to force one to do both jobs.

### Trust the Wisdom of the Crowd

If a font is popular, it's probably a good font. Sort font directories by popularity to quickly narrow your choices. This is especially useful when picking something other than a neutral UI typeface — choosing a nice serif with personality can be tough, and leveraging the collective decision-making of thousands helps.

### Steal from People Who Care

Inspect typefaces on your favorite sites using browser developer tools. Great design teams have strong opinions about typography and often choose excellent fonts you might never have found otherwise. Once you start paying closer attention to typography on well-designed sites, you'll quickly develop an eye for quality.

### Not Every Link Needs Color

In interfaces where everything is a link (nav menus, settings panels, dashboards), don't make every link blue/colored. Use heavier font weight or darker color for emphasis. Reserve colored/underlined treatment for links embedded in prose.

**Ancillary links** — links that are secondary and not part of the main user path — might not even need to be emphasized by default at all. Consider adding an underline or color change only on hover. They'll still be discoverable to users who think to try, but won't compete with more important actions.

```html
<!-- Primary link in prose: styled to stand out -->
<a class="text-blue-600 underline decoration-blue-300 underline-offset-2 hover:decoration-blue-600">
  Read the documentation
</a>

<!-- Navigation link in a UI where everything is a link: subtle emphasis -->
<a class="text-gray-700 font-medium hover:text-gray-900">Dashboard</a>

<!-- Ancillary/secondary link: only emphasized on hover -->
<a class="text-gray-500 hover:text-gray-700 hover:underline">View changelog</a>
```

---

## 4. Color

### Use HSL

Work in HSL (Hue, Saturation, Lightness), not hex or RGB, for all color reasoning:
- **Hue (0–360°):** position on color wheel (0°=red, 120°=green, 240°=blue)
- **Saturation (0–100%):** vibrancy (0%=grey, 100%=vivid)
- **Lightness (0–100%):** brightness (0%=black, 50%=pure color, 100%=white)

**HSL vs HSB — don't confuse them.** HSB (Brightness) is common in design tools, but browsers only understand HSL. In HSB, 100% brightness at 100% saturation gives you a vivid color; in HSL, 100% lightness is always pure white regardless of saturation. When working for the web, always think in HSL.

### Build a Full Palette

You need far more than 5 colors. A complete UI palette includes:

1. **Greys (8–10 shades):** From near-black (`gray-900`) to near-white (`gray-50`). Avoid pure black (#000) — start from a very dark grey.
2. **Primary color (5–10 shades):** Your brand color at various lightness levels (50 through 900).
3. **Accent colors (5–10 shades each):** For semantic states — red (danger/error), yellow (warning), green (success), blue (info), plus any extras for features, categories, tags.

**Total: often 8–10 colors × 9 shades each.**

Each end of the shade range has a natural use case:
- **Ultra-light shades (50–100):** tinted backgrounds for alerts, badges, banners, and highlighted sections
- **Mid-range shades (400–600):** button backgrounds, active states, icons
- **Dark shades (700–900):** text on light colored backgrounds

An alert component is a good reference that uses the full range of a single color:

```html
<!-- Alert using darkest shade for text, lightest for background, mid for icon -->
<div class="bg-blue-50 border-l-4 border-blue-500 p-4">
  <div class="flex items-start gap-3">
    <svg class="w-5 h-5 text-blue-500 shrink-0"><!-- info icon --></svg>
    <p class="text-blue-900 text-sm">Your trial expires in 3 days.</p>
  </div>
</div>
```

### Defining Shades (The 100–900 Method)

For each color:
1. **Pick the base (500):** The shade that would work as a button background.
2. **Pick the edges:** 900 = darkest, used for text on colored backgrounds. 100 = lightest, used for tinted background fills.
3. **Fill the gaps:** 700 and 300 first (midpoints), then 800, 600, 400, 200.
4. **Adjust by eye.** Trust visual balance over math.

```css
:root {
  --blue-50:  hsl(214, 100%, 97%);
  --blue-100: hsl(214, 95%, 93%);
  --blue-200: hsl(213, 97%, 87%);
  --blue-300: hsl(212, 96%, 78%);
  --blue-400: hsl(213, 94%, 68%);
  --blue-500: hsl(217, 91%, 60%);  /* base — button backgrounds */
  --blue-600: hsl(221, 83%, 53%);
  --blue-700: hsl(224, 76%, 48%);
  --blue-800: hsl(226, 71%, 40%);
  --blue-900: hsl(224, 64%, 33%);  /* text on light colored backgrounds */
}
```

### Saturation Compensation

As lightness moves away from 50%, saturation visually weakens. **Increase saturation for very light and very dark shades** to keep them from looking washed out.

If your base color is already at 100% saturation, you can't increase it further. In that case, use hue shifting (see below) to compensate — getting some brightness from hue instead of lightness keeps colors vibrant.

### Perceived Brightness and the Hue Brightness Curve

Every hue has an inherent **perceived brightness** independent of HSL lightness. At the same HSL lightness (50%), yellow looks dramatically lighter than blue to the human eye.

The perceived brightness of a color can be approximated from its RGB components:

```
perceived brightness = √(0.299 × R² + 0.587 × G² + 0.114 × B²)
```

Sampling different hues at 100% saturation and 50% lightness reveals that perceived brightness does **not** change linearly around the color wheel. Instead, there are three peaks and three valleys:

### Hue Shifting for Richer Shades

Every hue has an inherent **perceived brightness** independent of HSL lightness. At the same HSL lightness (50%), yellow looks dramatically lighter than blue to the human eye.

The color wheel has three **bright peaks** and three **dark valleys:**
- **Bright hues (high perceived brightness):** yellow (~60°), cyan (~180°), magenta (~300°)
- **Dark hues (low perceived brightness):** red (~0°), green (~120°), blue (~240°)

Normally when you want to change how light a color looks, you adjust lightness. While this works, you often lose intensity — the color looks closer to white or black, not just lighter or darker. Since different hues have different perceived brightness, **another way to change brightness is by rotating the hue:**

Use this to create richer shade palettes:
- To make a color **lighter** without washing it out: **rotate the hue towards the nearest bright hue** (60°, 180°, or 300°)
- To make a color **darker** without muddying it: **rotate towards the nearest dark hue** (0°, 120°, or 240°)

Example: when building a blue palette, shift lighter shades toward cyan. When building a yellow palette, shift darker shades toward orange (preventing muddy browns).

**This is particularly powerful for yellow.** By gradually rotating the hue towards orange as you decrease the lightness, the darker shades will feel warm and rich instead of dull and brown:

```css
/* Yellow palette with hue shifting to prevent brown dark shades */
--yellow-100: hsl(55, 95%, 90%);   /* pure yellow, light */
--yellow-300: hsl(50, 90%, 70%);   /* slightly toward orange */
--yellow-500: hsl(48, 90%, 55%);   /* base */
--yellow-700: hsl(42, 85%, 38%);   /* rotated toward orange */
--yellow-900: hsl(36, 80%, 25%);   /* firmly orange-ish, avoids brown */
```

You can combine hue rotation with lightness adjustments — getting some brightness from hue and some from lightness gives the richest results.

**Limit hue rotation to 20–30°** to avoid it looking like a completely different color.

### Warm and Cool Greys

True grey (saturation 0%) feels lifeless. Saturate your greys slightly:
- **Cool greys:** Add a touch of blue (`hsl(210, 10%, ...)`) — feels like "cool white" light bulbs
- **Warm greys:** Add a touch of yellow/orange (`hsl(40, 10%, ...)`) — feels like "warm white" light bulbs

Keep the temperature consistent across all grey shades. Increase saturation for lighter and darker ends of the scale to compensate for the lightness-saturation interaction. How much you saturate is up to you — a small amount tips the temperature slightly, while a larger amount makes the interface lean strongly in one direction.

```css
/* Cool grey palette (blue-tinted) */
--gray-50:  hsl(210, 15%, 97%);  /* extra saturation at edges */
--gray-100: hsl(210, 12%, 93%);
--gray-200: hsl(210, 10%, 85%);
--gray-300: hsl(210, 8%, 72%);
--gray-400: hsl(210, 6%, 56%);
--gray-500: hsl(210, 8%, 44%);
--gray-600: hsl(210, 10%, 36%);
--gray-700: hsl(210, 12%, 27%);
--gray-800: hsl(210, 14%, 18%);
--gray-900: hsl(210, 18%, 11%);  /* extra saturation at edges */

/* Warm grey palette (yellow-tinted) */
--gray-50:  hsl(40, 15%, 97%);
--gray-100: hsl(40, 12%, 93%);
/* ...same pattern with warm hue... */
--gray-900: hsl(40, 18%, 11%);
```

### Don't Use Grey Text on Colored Backgrounds

On a white background, grey text works because it reduces contrast. On colored backgrounds, grey text looks dull and washed out.

**Solution:** Pick a new color with the same hue as the background, adjusted in saturation and lightness.

```html
<!-- BAD: grey on blue looks washed out -->
<div class="bg-blue-600">
  <p class="text-gray-400">Muted text</p>
</div>

<!-- BAD: white with reduced opacity — also washed out, background bleeds through -->
<div class="bg-blue-600">
  <p class="text-white/50">Muted text</p>
</div>

<!-- GOOD: hand-picked same-hue lighter blue -->
<div class="bg-blue-600">
  <p class="text-blue-200">Muted text</p>
</div>
```

### Accessibility

- **Normal text (<18px):** minimum 4.5:1 contrast ratio (WCAG AA)
- **Large text (≥18px bold or ≥24px):** minimum 3:1 contrast ratio

### Flipping the Contrast

When white text on a colored background fails contrast requirements and darkening the background makes it too visually heavy, **flip the contrast:** use dark colored text on a light colored background instead.

```html
<!-- PROBLEM: white text on blue needs a very dark blue to meet 4.5:1 -->
<!-- This dark background grabs too much attention -->
<div class="bg-blue-800 text-white p-4">
  <p>Your trial expires in 3 days.</p>
</div>

<!-- SOLUTION: flip the contrast — dark text on light background -->
<!-- Same color family, but much less visually heavy -->
<div class="bg-blue-50 text-blue-900 p-4">
  <p>Your trial expires in 3 days.</p>
</div>
```

The color is still there to support the text, but it's way less in-your-face and doesn't interfere as much with other actions on the page. This is especially useful for secondary elements like alerts and badges that shouldn't dominate the page.

### Rotating Hue for Accessible Secondary Text

For colored text on a colored background (secondary text inside a dark-colored panel), **rotate the hue toward a brighter hue** (cyan, magenta, yellow) to boost contrast without approaching pure white. This makes it easier to keep the text accessible while still feeling colorful and distinct from primary white text.

```html
<!-- Secondary text on a dark blue panel -->
<div class="bg-blue-900 p-6">
  <h3 class="text-white font-semibold">Plan Details</h3>
  <!-- Instead of near-white, rotate toward cyan for accessible colored secondary text -->
  <p class="text-cyan-300 text-sm">Billed annually, cancel anytime</p>
</div>
```

### Don't Rely on Color Alone

Always pair color with another indicator:
- Icons (↑ ↓ ✓ ✗) alongside green/red for positive/negative
- Patterns or line styles for chart differentiation
- Contrast differences (light vs. dark) over hue differences for colorblind users

---

## 5. Shadows & Depth

### Emulate a Light Source from Above

All depth effects assume light comes from above:
- **Raised elements:** lighter top edge, shadow below
- **Inset elements:** darker top edge (shadow from above lip), lighter bottom edge

```css
/* Raised button */
.btn-raised {
  background: hsl(220, 80%, 55%);
  box-shadow:
    inset 0 1px 0 hsl(220, 80%, 65%),   /* light top edge */
    0 1px 3px rgba(0,0,0,0.12);           /* shadow below */
}

/* Inset well */
.well-inset {
  background: hsl(220, 15%, 94%);
  box-shadow:
    inset 0 2px 4px rgba(0,0,0,0.06),    /* shadow at top */
    inset 0 -1px 0 hsl(0, 0%, 100%);     /* light bottom edge */
}
```

For the top-edge highlight on raised elements: **hand-pick a lighter version of the background color** rather than overlaying semi-transparent white, which can desaturate the base color.

The inset treatment applies to any element that should feel recessed: text inputs, checkboxes, toggle tracks, and well/container components. A subtle inset shadow at the top edge plus a light bottom edge creates a natural "pressed in" appearance.

```css
/* Inset text input */
input[type="text"] {
  background: #fff;
  border: 1px solid #d1d5db;
  box-shadow: inset 0 2px 4px rgba(0,0,0,0.05);
}

/* Toggle track (inset/recessed feel) */
.toggle-track {
  background: hsl(220, 15%, 90%);
  border-radius: 9999px;
  box-shadow:
    inset 0 2px 4px rgba(0,0,0,0.06),
    inset 0 -1px 0 hsl(0, 0%, 100%);
}

/* Checkbox (unchecked, inset feel) */
.checkbox {
  width: 20px;
  height: 20px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  box-shadow: inset 0 1px 2px rgba(0,0,0,0.05);
}
```

**Don't get carried away simulating light.** Borrowing a few visual cues from the real world adds polish. Trying to make things look photo-realistic leads to busy, cluttered interfaces.

### The Shadow Elevation System

Define exactly 5 shadow levels and use them consistently:

```css
:root {
  --shadow-xs:  0 1px 2px rgba(0,0,0,0.05);
  --shadow-sm:  0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06);
  --shadow-md:  0 4px 6px rgba(0,0,0,0.07), 0 2px 4px rgba(0,0,0,0.06);
  --shadow-lg:  0 10px 15px rgba(0,0,0,0.1), 0 4px 6px rgba(0,0,0,0.05);
  --shadow-xl:  0 20px 25px rgba(0,0,0,0.1), 0 8px 10px rgba(0,0,0,0.04);
}
```

Usage by component:
| Elevation | Components | Tailwind |
|---|---|---|
| xs | Buttons at rest | `shadow-xs` |
| sm | Cards, inputs | `shadow-sm` |
| md | Dropdowns, popovers | `shadow-md` |
| lg | Fixed sidebars, sticky elements | `shadow-lg` |
| xl | Modals, dialogs | `shadow-xl` |

### Two-Part Shadows

Professional shadows combine two layers:
1. **Larger, softer shadow:** simulates direct light source — higher vertical offset, bigger blur
2. **Tighter, darker shadow:** simulates ambient light occlusion — less offset, smaller blur

```css
.card {
  box-shadow:
    0 10px 15px -3px rgba(0,0,0,0.08),   /* ambient / large */
    0 4px 6px -4px rgba(0,0,0,0.1);       /* direct / tight */
}
```

At higher elevations, the tight/dark shadow should fade away (because the element is far from the surface, ambient occlusion disappears).

**Accounting for elevation with two-part shadows:** As an object gets further away from a surface, the small, dark shadow from ambient light occlusion slowly disappears. So when building your elevation system with two-part shadows, make the tight/dark shadow more subtle as elevation increases. It should be quite distinct at your lowest elevation and almost (or completely) invisible at your highest elevation.

```css
/* Low elevation: strong ambient shadow */
--shadow-sm: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.08);

/* Medium elevation: ambient shadow weakens */
--shadow-md: 0 4px 6px rgba(0,0,0,0.07), 0 2px 4px rgba(0,0,0,0.05);

/* High elevation: ambient shadow almost gone */
--shadow-xl: 0 20px 25px rgba(0,0,0,0.1), 0 8px 10px rgba(0,0,0,0.02);
```

### Interactive Shadows

Shadows aren't only useful for positioning elements statically — they're a powerful way to provide visual cues during user interaction:

- **Hover on a card:** increase shadow level (sm → md) to suggest it's lifting toward the user
- **Click/press a button:** decrease shadow level (sm → xs) or remove shadow to suggest pressing in
- **Drag an item:** jump to a high shadow (lg/xl) to indicate it's floating above the page — this makes it clear the user can drag it and reinforces the feeling of direct manipulation

```html
<!-- Hover-to-lift card -->
<div class="bg-white rounded-lg shadow-sm hover:shadow-md transition-shadow cursor-pointer">
  ...
</div>

<!-- Press-to-sink button -->
<button class="bg-blue-600 text-white shadow-sm active:shadow-none transition-shadow">
  Submit
</button>

<!-- Drag-to-float list item -->
<div class="bg-white rounded-lg shadow-sm" data-state="idle">...</div>
<div class="bg-white rounded-lg shadow-xl scale-105" data-state="dragging">...</div>
```

Using shadows in a meaningful way like this is a great way to hack the process of choosing shadows. **Don't think about the shadow itself — think about where you want the element to sit on the z-axis** and assign the corresponding shadow level.

### Flat Depth (No Shadows)

Even without shadows, you can create depth:
- **Color:** lighter elements feel raised, darker elements feel inset
- **Solid shadows:** short vertical offset, zero blur radius — maintains a flat aesthetic while adding dimension

```html
<!-- Solid shadow for flat design -->
<div class="bg-white shadow-[0_4px_0_0_rgb(0,0,0,0.1)]">...</div>
```

### Overlapping Elements

Create layers by overlapping elements across background boundaries:

```html
<!-- Card overlapping a colored header section -->
<div class="bg-blue-600 pt-16 pb-32">
  <h1 class="text-white text-center">Dashboard</h1>
</div>
<div class="max-w-4xl mx-auto -mt-24 relative z-10">
  <div class="bg-white rounded-lg shadow-lg p-6">
    <!-- card content -->
  </div>
</div>
```

You can also make an element **taller than its parent** so it overlaps on both sides, or let controls overlap another component (like carousel navigation arrows overlapping the carousel image).

```html
<!-- Element taller than parent, overlapping on both sides -->
<div class="bg-gray-100 py-12">
  <div class="max-w-4xl mx-auto flex items-center gap-8">
    <div class="flex-1">
      <h2>Our Product</h2>
      <p>Description text...</p>
    </div>
    <!-- Image taller than section, overlapping top and bottom -->
    <img class="w-80 -my-20 relative z-10 rounded-lg shadow-xl" src="..." />
  </div>
</div>

<!-- Carousel controls overlapping the image -->
<div class="relative">
  <img class="rounded-lg" src="..." />
  <button class="absolute left-0 top-1/2 -translate-y-1/2 -translate-x-1/2
                  w-10 h-10 bg-white rounded-full shadow-lg flex items-center justify-center">
    ←
  </button>
  <button class="absolute right-0 top-1/2 -translate-y-1/2 translate-x-1/2
                  w-10 h-10 bg-white rounded-full shadow-lg flex items-center justify-center">
    →
  </button>
</div>
```

For overlapping images, add an "invisible border" matching the background color to create a gap:

```html
<img class="ring-4 ring-white rounded-full" src="..." />
```

This creates the appearance of layers — you'll still see the overlap effect but without the ugly clashing that occurs when images sit directly on top of each other.

---

## 6. Working with Images

### Use Good Photos

Bad photos ruin a design, even if everything else looks great. If your design needs photography, either hire a professional or use high-quality stock photography (Unsplash, Pexels, etc.). **Never design with placeholder images and expect to swap in smartphone photos later** — it never works. Photography quality is a non-negotiable foundation.

### Text on Images

Photos have dynamic light and dark areas. Text placed over them won't have consistent contrast.

**Solutions (in order of preference):**

1. **Semi-transparent overlay:**
```html
<div class="relative">
  <img src="..." class="absolute inset-0 w-full h-full object-cover" />
  <div class="absolute inset-0 bg-black/40"></div>
  <div class="relative z-10 text-white">...</div>
</div>
```

2. **Lower image contrast + adjust brightness** (via CSS filters):
```css
.hero-image { filter: contrast(0.8) brightness(0.9); }
```

3. **Colorize the image** (desaturate + multiply blend with brand color):
```css
.hero-image {
  filter: contrast(0.8) saturate(0);
  mix-blend-mode: multiply;
  background-color: hsl(220, 60%, 40%);
}
```

4. **Text shadow (glow, not drop):** large blur, no offset:
```css
.hero-text { text-shadow: 0 0 40px rgba(0,0,0,0.5); }
```

### Icons at Their Intended Size

- Icons drawn at 16–24px look chunky at 3–4× their size. **Don't just scale up SVG icons.**
- Instead, enclose small icons in a larger shape with a background color:

```html
<!-- Wrap a 24px icon in a colored circle for a feature section -->
<div class="w-12 h-12 rounded-full bg-blue-100 flex items-center justify-center">
  <svg class="w-6 h-6 text-blue-600">...</svg>
</div>
```

- **Don't scale down icons either.** Icons designed at larger sizes become choppy and fuzzy at small sizes. The most extreme example is favicons — if you try to shrink a logo drawn at 128px down to a 16px favicon, it all turns to mush as the browser tries to render all that detail in a tiny square. Instead, **redraw a super simplified version** at the target size, so you control the compromises instead of leaving them to the browser. Favicons and small icons need to be redrawn as simplified versions — don't just shrink a detailed 128px logo to 16px.

### Don't Scale Down Screenshots

If you shrink a full-size screenshot by 70% to fit a features page, the 16px text in your app becomes 4px text in the screenshot — unreadable. Instead:
- Take the screenshot at a **smaller screen size** (tablet layout) so it needs less shrinking
- Show a **partial screenshot** (just the relevant section) so it fits without heavy scaling
- If you must show the full app in a small space, **draw a simplified illustration** of the UI with details removed and small text replaced by placeholder lines

The simplified illustration approach is powerful: it still communicates the big-picture design without tempting visitors to try and make out all the details. Use simple rectangles, circles, and lines to represent the major layout areas.

```html
<!-- Simplified UI illustration: represent text with grey lines, images with colored boxes -->
<div class="w-64 bg-white rounded-lg shadow-lg p-4 text-left">
  <div class="flex items-center gap-2 mb-3">
    <div class="w-6 h-6 rounded-full bg-blue-200"></div>
    <div class="h-2 bg-gray-300 rounded-full w-20"></div>
  </div>
  <div class="h-2 bg-gray-200 rounded-full w-full mb-2"></div>
  <div class="h-2 bg-gray-200 rounded-full w-4/5 mb-2"></div>
  <div class="h-2 bg-gray-200 rounded-full w-3/5 mb-4"></div>
  <div class="h-24 bg-gray-100 rounded mb-3"></div>
  <div class="flex gap-2">
    <div class="h-6 bg-blue-500 rounded w-16"></div>
    <div class="h-6 bg-gray-200 rounded w-16"></div>
  </div>
</div>
```

### User-Uploaded Content

- **Control shape:** Use `object-cover` with fixed-size containers. Never display at intrinsic aspect ratio if layout consistency matters.
- **Prevent background bleed:** When an image's background color is similar to the UI background, the image loses its shape. A hard border will often clash with the image's colors. Instead, use a subtle inset shadow or semi-transparent inner border.

```html
<!-- Tailwind: object-cover in a fixed container -->
<div class="w-16 h-16 rounded-full overflow-hidden ring-1 ring-black/5">
  <img src="..." class="w-full h-full object-cover" />
</div>

<!-- Alternative: subtle inset shadow to prevent background bleed -->
<div class="w-16 h-16 rounded-full overflow-hidden shadow-[inset_0_0_0_1px_rgba(0,0,0,0.05)]">
  <img src="..." class="w-full h-full object-cover" />
</div>
```

```css
/* CSS: background-image technique for user-uploaded avatars */
.avatar {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  background-size: cover;
  background-position: center;
  box-shadow: inset 0 0 0 1px rgba(0,0,0,0.05);
}
```

If you don't like the slight "inset" look from a box shadow, a **semi-transparent inner border** works great too. Borders will often clash with the colors in the image, while most people will barely notice the shadow or inner border is there.

```html
<!-- Semi-transparent inner border alternative (using outline or ring) -->
<div class="w-16 h-16 rounded-full overflow-hidden relative">
  <img src="..." class="w-full h-full object-cover" />
  <div class="absolute inset-0 rounded-full ring-1 ring-inset ring-black/10"></div>
</div>
```

---

## 7. Borders & Separation

### Use Fewer Borders

Borders are the bluntest tool for separation. Prefer, in order:

1. **Box shadow** — subtle outline without visual weight:
```html
<div class="shadow-sm rounded-lg">...</div>
```

2. **Different background colors** — adjacent elements with contrasting fills:
```html
<aside class="bg-gray-50">...</aside>
<main class="bg-white">...</main>
```

3. **Extra spacing** — increase the gap between groups:
```html
<div class="space-y-8">  <!-- more space = more separation -->
```

4. **Borders as last resort** — fine for dense tables or tightly packed lists where other methods waste too much space.

---

## 8. Finishing Touches

### Supercharge the Defaults

Replace generic default elements with richer versions:
- **Bulleted lists → icon lists.** Use checkmarks, arrows, or topic-specific icons instead of `•`.
- **Blockquotes → styled testimonials.** Enlarge the quotation mark into a decorative visual element — increase its size dramatically and give it a brand color or lighter shade.
- **Links → custom underlines.** Use `text-decoration-color`, `text-underline-offset`, and `text-decoration-thickness` for thick, colorful underlines that partially overlap the text.
- **Checkboxes/radios → custom styled.** Just using one of your brand colors for the selected state instead of browser defaults is often enough to take a form from boring to polished.

```html
<!-- Custom checkbox with brand color -->
<label class="flex items-center gap-3 cursor-pointer">
  <input type="checkbox" class="sr-only peer" />
  <div class="w-5 h-5 rounded border-2 border-gray-300
              peer-checked:bg-blue-600 peer-checked:border-blue-600
              flex items-center justify-center transition-colors">
    <svg class="w-3.5 h-3.5 text-white hidden peer-checked:block"><!-- check icon --></svg>
  </div>
  <span class="text-sm text-gray-700">Remember me</span>
</label>

<!-- Custom radio with brand color -->
<label class="flex items-center gap-3 cursor-pointer">
  <input type="radio" name="option" class="sr-only peer" />
  <div class="w-5 h-5 rounded-full border-2 border-gray-300
              peer-checked:border-blue-600
              flex items-center justify-center transition-colors">
    <div class="w-2.5 h-2.5 rounded-full bg-blue-600 hidden peer-checked:block"></div>
  </div>
  <span class="text-sm text-gray-700">Option A</span>
</label>
```

```html
<!-- Icon list instead of bullets -->
<ul class="space-y-3">
  <li class="flex items-start gap-3">
    <svg class="w-5 h-5 text-green-500 mt-0.5 shrink-0"><!-- check icon --></svg>
    <span>Feature description here</span>
  </li>
</ul>

<!-- Styled testimonial with decorative quote mark -->
<blockquote class="relative pl-12">
  <span class="absolute left-0 top-0 text-6xl text-blue-200 leading-none font-serif">"</span>
  <p class="text-gray-700 text-lg italic">The product changed how we work...</p>
</blockquote>

<!-- Custom link underline -->
<a class="text-blue-600 underline decoration-blue-300 decoration-2 underline-offset-2 hover:decoration-blue-600">
  Learn more
</a>
```

### Accent Borders

A single colored border strip can make a bland element feel designed:
- Top of a card: `border-t-4 border-blue-500`
- Left of an alert: `border-l-4 border-yellow-400`
- Active nav item: `border-b-2 border-blue-600`
- Below a heading: a short colored line
- Top of the whole page: `border-t-4 border-indigo-500` on body/header

### Decorate Backgrounds

When hierarchy and spacing are solid but the design still feels flat:
- **Change background color** between sections (white → gray-50 → white). This works great for emphasizing individual panels as well as for adding distinction between entire page sections.
- **Subtle gradients** using two hues no more than ~30° apart. For a more energetic look without being garish.
- **Repeating patterns** at very low contrast. Resources like Hero Patterns provide subtle repeatable backgrounds. A pattern designed to repeat along a single edge can look great too — it doesn't have to tile across the entire background.
- **Simple geometric shapes** positioned at edges of sections. These don't need to be complex — a couple of circles or angled rectangles at the corners add visual interest.
- **Small chunks of a repeatable pattern** in specific positions rather than across the whole background.
- **Simplified illustrations** like a world map or abstract shape, placed behind content at very low contrast.
- Keep pattern/decoration contrast low to preserve readability

```html
<!-- Alternating section backgrounds -->
<section class="bg-white py-16">...</section>
<section class="bg-gray-50 py-16">...</section>
<section class="bg-white py-16">...</section>

<!-- Subtle gradient background -->
<section class="bg-gradient-to-br from-indigo-600 to-blue-500 py-16">
  <div class="text-white">...</div>
</section>

<!-- Decorative shape positioned at section edge -->
<section class="relative overflow-hidden bg-gray-50 py-16">
  <div class="absolute -top-16 -right-16 w-64 h-64 bg-blue-100 rounded-full opacity-50"></div>
  <div class="absolute -bottom-8 -left-8 w-48 h-48 bg-indigo-100 rounded-full opacity-40"></div>
  <div class="relative z-10"><!-- content --></div>
</section>
```

### Empty States

Never ship an empty state that's just blank space. When content depends on user input:
- Include an illustration or icon to draw attention
- Provide a clear call-to-action ("Create your first project")
- Hide irrelevant supporting UI (tabs, filters) until there's content to act on

Empty states are a user's **first interaction** with a new product or feature. Use them as an opportunity to be interesting and exciting — don't settle for plain and boring.

The common mistake: you spend time crafting the perfect realistic sample data, pick beautiful usernames and avatars, design a gorgeous screen — then deploy to production. When an excited user clicks the new item in the nav, they see... nothing. A blank table with headers and no rows. The empty state should be a priority, not an afterthought.

```html
<!-- BAD: empty table with just headers -->
<table>
  <thead><tr><th>Name</th><th>Status</th><th>Date</th></tr></thead>
  <tbody></tbody>
</table>

<!-- GOOD: designed empty state with illustration and CTA -->
<div class="text-center py-16">
  <div class="w-16 h-16 mx-auto mb-4 rounded-full bg-blue-50 flex items-center justify-center">
    <svg class="w-8 h-8 text-blue-400"><!-- empty folder icon --></svg>
  </div>
  <h3 class="text-lg font-medium text-gray-900">No projects yet</h3>
  <p class="mt-1 text-sm text-gray-500 max-w-sm mx-auto">
    Get started by creating your first project. You can always add more later.
  </p>
  <button class="mt-6 bg-blue-600 text-white px-4 py-2 rounded-md text-sm font-medium hover:bg-blue-700">
    Create a project
  </button>
</div>
```

### Think Outside the Box

Don't default to generic component shapes. Reimagine common patterns:
- **Dropdowns** can have sections, columns, icons, and supporting text — not just a plain list of links
- **Tables** can combine related columns (name + email in one cell), include images, use color accents, and have hierarchical text styling within cells
- **Radio buttons** can become selectable cards with descriptions and icons
- **Form inputs** can incorporate buttons inside them (search bars with an inline submit)
- **Headlines** can use two different font colors to create emphasis within a single line
- **Datepickers** can invert the background color (dark background, light text) for a distinctive feel
- **Table content** doesn't have to be plain text — add images, color badges, or progress bars within cells

#### Rich Dropdown Example

```html
<!-- Instead of a boring list of links, use sections, icons, and descriptions -->
<div class="absolute top-full mt-2 w-[540px] bg-white rounded-xl shadow-xl border border-gray-100 p-4">
  <div class="grid grid-cols-2 gap-3 pb-4 border-b border-gray-100">
    <a class="flex items-start gap-3 p-3 rounded-lg hover:bg-gray-50">
      <div class="w-10 h-10 rounded-lg bg-blue-50 flex items-center justify-center shrink-0">
        <svg class="w-5 h-5 text-blue-600"><!-- icon --></svg>
      </div>
      <div>
        <div class="font-medium text-gray-900 text-sm">Engagement</div>
        <div class="text-xs text-gray-500">Measure actions users take</div>
      </div>
    </a>
    <!-- More items... -->
  </div>
  <div class="pt-3 space-y-1">
    <a class="flex items-center gap-3 p-2 rounded-lg hover:bg-gray-50 text-sm text-gray-600">
      <svg class="w-4 h-4 text-gray-400"><!-- icon --></svg>
      Documentation
    </a>
  </div>
</div>
```

#### Selectable Card Radio Buttons

```html
<!-- Instead of boring radio buttons, use selectable cards -->
<fieldset class="grid grid-cols-4 gap-3">
  <label class="cursor-pointer">
    <input type="radio" name="plan" value="hobby" class="sr-only peer" />
    <div class="rounded-lg border-2 border-gray-200 p-4 text-center
                peer-checked:border-green-500 peer-checked:ring-1 peer-checked:ring-green-500
                hover:border-gray-300 transition-colors">
      <div class="text-xs font-semibold uppercase tracking-wide text-gray-500">Hobby</div>
      <div class="text-2xl font-bold text-gray-900 mt-1">1 <span class="text-sm font-normal">GB</span></div>
      <div class="text-sm text-gray-500 mt-1">$5 / mo</div>
    </div>
  </label>
  <!-- More plan options... -->
</fieldset>
```

#### Rich Table with Combined Columns and Color Badges

```html
<table class="w-full">
  <thead>
    <tr class="text-left text-xs font-medium text-gray-500 uppercase tracking-wide">
      <th class="pb-3 pl-4">Name</th>
      <th class="pb-3">Policy</th>
      <th class="pb-3">Location</th>
      <th class="pb-3">Status</th>
      <th class="pb-3"></th>
    </tr>
  </thead>
  <tbody>
    <tr class="border-t border-gray-100">
      <td class="py-3 pl-4">
        <div class="flex items-center gap-3">
          <img class="w-8 h-8 rounded-full object-cover" src="..." />
          <div>
            <div class="font-medium text-gray-900 text-sm">Molly Sanders</div>
            <div class="text-xs text-gray-500">VP of Sales</div>
          </div>
        </div>
      </td>
      <td>
        <div class="text-sm font-medium text-gray-900">$20,000</div>
        <div class="text-xs text-gray-500">All-Inclusive Policy</div>
      </td>
      <td class="text-sm text-gray-700">Denver, CO</td>
      <td>
        <span class="inline-flex px-2.5 py-0.5 rounded-full text-xs font-medium bg-green-50 text-green-700">
          Approved
        </span>
      </td>
      <td class="text-right pr-4">
        <button class="text-gray-400 hover:text-gray-600">•••</button>
      </td>
    </tr>
  </tbody>
</table>
```

#### Search Input with Inline Button

```html
<div class="relative">
  <input type="text" placeholder="Search..."
    class="w-full pl-4 pr-24 py-3 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-blue-500" />
  <button class="absolute right-1.5 top-1.5 bg-blue-600 text-white px-4 py-1.5 rounded-md text-sm font-medium hover:bg-blue-700">
    Search
  </button>
</div>
```

#### Two-Color Headlines

```html
<!-- Use different colors within a single heading for emphasis -->
<h1 class="text-4xl font-bold">
  <span class="text-gray-900">Build your</span>
  <span class="text-blue-600"> dream website</span>
</h1>
```

### Unintuitive Creative Tricks

When studying well-designed interfaces, look for decisions you wouldn't have made yourself. Common discoveries that elevate a design:
- Reducing line-height on headings (line-height: 1 or even tighter)
- Adding letter-spacing to uppercase text
- Combining multiple box-shadows
- Using different text colors within a single heading
- Placing interactive elements inside other components (button inside an input, controls overlapping a carousel)
- Inverting the background color of a normally-white component (e.g., dark datepicker)

---


## 9. Design System Fundamentals

### Choosing a Personality

The feel of a UI comes from concrete decisions:
- **Serif typeface** → elegant, classic (e.g., Freight Text, Georgia, Playfair Display)
- **Rounded sans-serif** → playful, friendly (e.g., Proxima Soft, Nunito, Varela Round)
- **Neutral sans-serif** → professional, clean (e.g., Freight Sans, Inter, system fonts)
- **Large border radius** → playful (`rounded-xl`, `rounded-2xl`)
- **Small border radius** → neutral (`rounded`, `rounded-md`)
- **No border radius** → serious, formal (`rounded-none`)
- **Blue palette** → safe, trustworthy — nobody ever complains about blue
- **Gold/amber** → premium, sophisticated — might say "expensive"
- **Pink/coral** → fun, approachable — not so serious

**Deciding what you actually want:** If you're unsure about the personality you're going for, look at other sites used by the people you want to reach. If they're mostly "serious business", maybe that's how your site should look too. If they're playful with a bit of humor, maybe that's a better direction. Just try not to borrow too much from direct competitors — you don't want to look like a second-rate version of something else.

**Language and tone** matter as much as visual choices. Formal, impersonal copy ("Your account has been updated") feels professional. Casual, friendly copy ("All set! Your account is updated.") feels approachable. The words in your UI shape its personality just as much as the typeface or color palette.

**Stay consistent.** Don't mix square corners with rounded corners in the same interface. Don't mix casual and formal tone in the same flow. Whatever personality choices you make, stick with them throughout.

```html
<!-- Elegant/Classic personality -->
<div class="font-serif rounded-none border-b-2 border-amber-600">
  <h1 class="text-3xl tracking-tight">Timeless Design</h1>
</div>

<!-- Playful/Friendly personality -->
<div class="font-sans rounded-2xl bg-pink-50 p-6">
  <h1 class="text-3xl font-bold text-pink-600">Let's get started! 🎉</h1>
</div>

<!-- Professional/Clean personality -->
<div class="font-sans rounded-md border border-gray-200 shadow-sm p-6">
  <h1 class="text-xl font-semibold text-gray-900">Account Settings</h1>
</div>
```

### Systematize Everything

Pre-define constrained value sets for:

| Property | Recommended Scale |
|---|---|
| Font size | 12, 14, 16, 18, 20, 24, 30, 36, 48, 60, 72 |
| Font weight | 400, 500, 600, 700 |
| Line height | 1, 1.25, 1.375, 1.5, 1.625, 1.75, 2 |
| Spacing/sizing | 4, 6, 8, 12, 16, 24, 32, 48, 64, 96, 128 |
| Border radius | 2, 4, 6, 8, 12, 16, 9999 (full) |
| Border width | 1, 2, 4 |
| Box shadows | 5 elevation levels (see Shadows section) |
| Opacity | 0, 5, 10, 20, 25, 40, 50, 60, 75, 80, 90, 95, 100 |

Using a system means you only agonize over initial setup. After that, every decision is a quick pick from constrained options.

### Process Tips

- **Start with a feature, not a layout.** Design one piece of functionality first, then figure out navigation/shell later. An "app" is a collection of features — you don't have the information to decide on navigation structure until you've designed a few features.
- **Design in grayscale first.** Forces you to nail hierarchy through spacing, size, and weight before reaching for color. It's a little more challenging, but you'll end up with a clearer interface that's easy to enhance with color later.
- **Design the smallest useful version.** Ship a simple working version rather than a half-finished complex one.
- **Be a pessimist.** Don't imply functionality in your designs that you aren't ready to build. An incomplete feature that ships is better than a complete design that never ships.
- **Don't over-invest in mockups.** Sketches and wireframes are disposable — use them to explore ideas, then leave them behind and build the real thing. Users can't do anything with static mockups.
- **Work in cycles:** design → build → fix problems → design next feature. It's a lot easier to fix design problems in an interface you can actually use than it is to imagine every edge case in advance.
- **Mobile first.** Start with ~400px canvas. Most things won't change much at larger sizes. Once you have a mobile design you're happy with, bring it to a larger screen and adjust what felt like a compromise.
- **If part of a feature is a "nice-to-have", design it later.** Build the simple version first and you'll always have something to fall back on.
- **Look for decisions you wouldn't have made yourself.** Study well-designed interfaces and notice unintuitive choices — this is the fastest way to add new tools to your design toolkit.
- **Rebuild your favorite interfaces from scratch.** Without peeking at dev tools, try to recreate a design you admire. The differences between your version and the original will teach you more than any tutorial.

---

## 10. Forms

### Form Layout

Vertical stacked forms are the default. Labels go above inputs, left-aligned. Avoid multi-column forms unless the form is very long or expert-oriented.

```html
<!-- Tailwind -->
<label class="block space-y-1">
  <span class="text-sm font-medium text-gray-700">Email</span>
  <input class="w-full rounded-md border-gray-300 shadow-sm" />
</label>
```

```css
/* CSS equivalent */
.form-field label {
  display: block;
  font-size: 0.875rem;
  font-weight: 500;
  color: #374151;
  margin-bottom: 4px;
}
.form-field input {
  width: 100%;
  border: 1px solid #d1d5db;
  border-radius: 6px;
}
```

**Placeholders are examples, not labels.** Never use a placeholder as the only indicator of what a field is for. If space is tight, use smaller label text — not no label.

### Grouping & Spacing in Forms

Apply the ambiguous-spacing rule aggressively in forms:
- **Label → input:** tight (~4–8px)
- **Input → help/error text:** tight (~4–8px)
- **Between field groups:** medium (~16–24px)
- **Between form sections:** large (~32–48px)

The label-to-input gap must always be noticeably smaller than the gap between field groups, or labels will feel disconnected from their inputs.

### Help Text & Errors

- Help text is secondary — lighter color, smaller size
- Errors should appear close to the input, use color + icon + text (never color alone), and avoid shifting the layout dramatically

```html
<p class="mt-1 text-sm text-gray-500">We'll never share your email.</p>

<p class="mt-1 text-sm text-red-600 flex items-center gap-1">
  <svg class="w-4 h-4 shrink-0"><!-- alert icon --></svg>
  Invalid email address
</p>
```

### Required vs Optional

Mark **optional** fields, not required ones. Required-by-default reduces visual noise — most fields in a form are required anyway.

```html
<label class="text-sm font-medium text-gray-700">
  Phone number <span class="text-gray-400 font-normal">(optional)</span>
</label>
```

### Form Actions

One primary button per form. Secondary actions (Cancel, Reset) use visually weaker styling. Destructive actions within forms should look clearly different from the primary submit.

```html
<div class="flex items-center gap-3 pt-6 border-t border-gray-100">
  <button class="bg-blue-600 text-white px-4 py-2 rounded-md font-medium">Save changes</button>
  <button class="text-gray-600 px-4 py-2">Cancel</button>
</div>
```

---

## 11. Tables

### When to Use Tables

Tables are for **comparing** and **scanning** structured data. If users are reading row-by-row like prose, cards or lists are a better fit.

### Alignment in Tables

- Text columns → left-aligned
- Numeric columns → right-aligned
- Icon/status columns → center-aligned
- Headers align with their column content

Use `tabular-nums` (or Tailwind `tabular-nums`) on numeric columns so digits are monospaced and align vertically.

```html
<th class="text-right text-xs font-medium text-gray-500 uppercase tracking-wide">Revenue</th>
<td class="text-right tabular-nums text-gray-900">$12,483.00</td>
```

```css
/* CSS equivalent */
td.numeric {
  text-align: right;
  font-variant-numeric: tabular-nums;
}
```

### Table Headers

Headers function as labels — give them secondary visual weight: smaller size, lighter color, or uppercase with tracking. Avoid heavy borders under headers; a subtle background tint or a single thin bottom border is enough (not both).

### Row Separation

Prefer these over gridlines:
1. Generous vertical padding on rows
2. Very subtle zebra striping (`bg-gray-50` on every other row)
3. Hover background (`hover:bg-gray-50`)

If borders are needed, use 1px low-contrast horizontal lines only. Avoid full gridlines in all directions — they make tables feel like spreadsheets.

```html
<tr class="border-b border-gray-100 hover:bg-gray-50">
  <td class="py-3 px-4">...</td>
</tr>
```

### Actions in Tables

Don't overload rows with buttons. The primary action should be a row click or a single visible button. Secondary actions belong in a hover state, a kebab/overflow menu, or a context menu.

---

## 12. Dashboards

### Dashboard Layout

A dashboard is a summary, not a report. Each section should answer "what changed?" or "what needs attention?" — not try to show every possible data point.

- Use a constrained width — don't go full-bleed by default
- Group related metrics together
- Use consistent card sizing per row

```html
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  <!-- Metric cards at consistent sizes -->
</div>
```

### Metrics & KPIs

Apply the hierarchy principle to metric displays:
- **Value** is primary → large, heavy
- **Label** is secondary → small, lighter
- **Trend/change** is tertiary → small, colored for direction

```html
<div>
  <div class="text-sm text-gray-500">Monthly Revenue</div>
  <div class="text-3xl font-semibold text-gray-900">$42,381</div>
  <div class="text-sm text-green-600 flex items-center gap-1">
    <svg class="w-4 h-4"><!-- arrow up --></svg>
    12% vs last month
  </div>
</div>
```

Use color for direction (up/down, good/bad) — not magnitude. Always pair the color with an icon or text indicator.

### Charts in Dashboards

- Titles should explain *what* and *timeframe* ("Revenue, last 12 months")
- Axes and gridlines should be subtle — lighter color, thinner weight
- Legends should be unnecessary if inline labels are clear
- Avoid decorative gradients, heavy borders, and too many colors

### Loading & Empty States in Dashboards

- Prefer skeleton placeholders over spinners — they preserve layout and feel faster
- Empty dashboards should explain why they're empty and what the user should do next

---

## 13. Cards

### When to Use Cards

Cards are for grouping related content into a scannable preview. Use cards when content is heterogeneous and doesn't fit neatly into table rows. Don't use cards when a simple list or table would be clearer.

### Card Anatomy

Typical structure: optional header, main content, optional footer/actions. Padding within a card should be generous (~16–24px). Space between cards should be noticeably larger (~24–32px).

### Card Separation: Shadow vs Border

Default to a light background with a subtle shadow. Use borders instead when:
- Cards sit on an already-elevated surface
- Cards are very dense and shadows would be too heavy
- You need precise visual separation

**Never combine a heavy border and a heavy shadow on the same card.**

```html
<!-- Shadow card (default) -->
<div class="bg-white rounded-lg shadow-sm p-6">...</div>

<!-- Border card (on elevated surfaces or dense layouts) -->
<div class="bg-white rounded-lg border border-gray-200 p-6">...</div>
```

### Clickable Cards

If a card is clickable, the entire card should be the click target (not just a hidden link inside it). The hover state must be obvious — a shadow increase or subtle background shift. Cursor should indicate interactivity.

```html
<div class="bg-white rounded-lg shadow-sm p-6 hover:shadow-md transition-shadow cursor-pointer">
  ...
</div>
```

```css
/* CSS equivalent */
.card-clickable {
  cursor: pointer;
  transition: box-shadow 150ms ease;
}
.card-clickable:hover {
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1), 0 2px 4px -2px rgba(0,0,0,0.1);
}
```

---

## 14. Lists

### Lists vs Tables vs Cards

- **List** → simple, linear information in a single visual track
- **Table** → structured comparison across multiple attributes
- **Card** → grouped preview of heterogeneous content

Don't blur these roles. If you find yourself adding many columns to a list, switch to a table. If list items have rich, varied content, switch to cards.

### List Item Structure

Primary text first, secondary metadata smaller and lighter, actions separated visually to the right or behind an overflow menu.

```html
<div class="flex items-center justify-between py-3">
  <div>
    <div class="font-medium text-gray-900">Invoice #4832</div>
    <div class="text-sm text-gray-500">Due in 3 days</div>
  </div>
  <button class="text-sm text-blue-600 hover:text-blue-800">View</button>
</div>
```

---

## 15. Tabs, Filters & Controls

### Tabs

Tabs switch views, not trigger actions. The active tab must be visually obvious without relying on color alone — use a combination of border, weight, and/or background. Keep tab count manageable (roughly 5–7 max before reconsidering the navigation pattern).

```html
<!-- Active tab: bottom border + font weight + darker text -->
<button class="border-b-2 border-blue-600 text-blue-600 font-medium pb-2 px-4">
  Overview
</button>
<!-- Inactive tab -->
<button class="text-gray-500 hover:text-gray-700 pb-2 px-4">
  Settings
</button>
```

### Filters

Filters are secondary to the content they control — style them lighter than the main content area. Group related filters together. Hide advanced or rarely-used filters behind a disclosure ("More filters") by default.

### Destructive Confirmations

Destructive actions should always require a confirmation step. On the initial page, style the destructive action as secondary or tertiary. In the confirmation dialog, the destructive action becomes the primary action and gets the bold, red styling.

```html
<!-- On the settings page: destructive action as secondary -->
<button class="text-red-600 text-sm hover:text-red-800">Delete project</button>

<!-- In the confirmation modal: destructive action as primary -->
<button class="bg-red-600 text-white font-medium px-4 py-2 rounded-md">
  Yes, delete project
</button>
```

---

## 16. Decision Frameworks

### When Unsure About Spacing

Start larger than you think is right, then reduce one scale step at a time. The first value that looks "a little too much" in isolation is usually "just right" in the full-page context.

### When Choosing a Size for Any Element

Pick a value from the system. Try the values on either side. Eliminate the obvious losers. If both neighbors look bad, the middle value is correct.

### When Something Feels "Off" but You Can't Name Why

Check these in order — the problem is almost always one of them:
1. **Spacing** — ambiguous grouping, not enough breathing room, inconsistent gaps
2. **Hierarchy** — too many elements competing for attention, weak primary/secondary contrast
3. **Contrast** — text too light, backgrounds too similar, icons competing with text
4. **Variety** — too many font sizes, too many colors, too many border styles

The fix is almost never "add more decoration." It's almost always "remove, simplify, or increase contrast."

### The Guiding Principle

When generating UI code:

> **Prefer clarity over cleverness. Prefer systems over tweaks. Prefer removing over adding.**

If something looks wrong, it's almost always spacing, hierarchy, or contrast — not color or decoration.

---

## 17. Common Mistakes (Never Do X, Instead Do Y)

1. **Never pick arbitrary spacing values.** Instead: use a constrained scale (4, 8, 12, 16, 24, 32, 48, 64...).
2. **Never rely on font size alone for hierarchy.** Instead: combine size, weight, and color.
3. **Never use font weights below 400 for body text.** Instead: use lighter color or smaller size to de-emphasize.
4. **Never use grey text on colored backgrounds.** Instead: pick a same-hue color with adjusted saturation/lightness.
5. **Never reduce opacity of white text as the only way to de-emphasize on colored backgrounds.** Instead: hand-pick a color — opacity makes text look washed out and lets backgrounds bleed through.
6. **Never make destructive buttons big and red by default.** Instead: style them as secondary/tertiary; reserve bold destructive styling for confirmation dialogs.
7. **Never use the same spacing inside a group and between groups.** Instead: always make inter-group spacing noticeably larger than intra-group spacing.
8. **Never stretch content to fill available width.** Instead: give elements the width they need, add max-width constraints.
9. **Never use CSS preprocessor `lighten()`/`darken()` to generate shades on the fly.** Instead: define all shades in advance.
10. **Never scale icons far beyond their intended size.** Instead: enclose them in a background shape.
11. **Never make all sidebar/nav widths fluid percentages.** Instead: use fixed widths for sidebars, flexible widths for main content.
12. **Never use the same line-height for all text.** Instead: tighter for large headings (~1–1.25), looser for small body text (~1.5–1.75).
13. **Never center-align text longer than 2–3 lines.** Instead: left-align, or rewrite to be shorter.
14. **Never forget empty states.** Instead: design them as a first-class part of the UI with illustration and CTA.
15. **Never rely on color alone to communicate information.** Instead: always pair color with icons, labels, or pattern/contrast differences.
16. **Never vertically center-align mixed font sizes on the same line.** Instead: align to baseline.
17. **Never reach for a border as the first way to separate elements.** Instead: try shadow, background color difference, or more spacing first.
18. **Never set line length beyond 75 characters.** Instead: constrain paragraphs to `max-w-prose` or `max-width: 65ch`.
19. **Never use a linear spacing scale (4, 8, 12, 16, 20, 24...).** Instead: use a scale with increasing gaps (4, 8, 12, 16, 24, 32, 48, 64...).
20. **Never define heading styles based on HTML heading level.** Instead: style headings based on their role in the visual hierarchy, independently of their semantic level.
21. **Never design the app shell (navigation, sidebar, layout) first.** Instead: start with a specific feature. You don't have enough information to decide on navigation until you've designed several features.
22. **Never use a modular type scale for interface design.** Instead: hand-craft a scale. Modular scales produce fractional values and don't have enough granularity for UI work.
23. **Never use placeholder text as the only label for a form field.** Instead: always include a visible label. If space is tight, use a smaller label, not no label.
24. **Never assume proportional relationships will hold across screen sizes.** Instead: independently tune font sizes, padding, and spacing at each breakpoint.
25. **Never design with placeholder images expecting to swap in smartphone photos later.** Instead: use professional photography or high-quality stock photos from the start.
26. **Never try to shrink a detailed logo to favicon size.** Instead: redraw a simplified version at the target size.
27. **Never use CSS `lighten()`/`darken()` preprocessor functions to generate shades on the fly.** Instead: define all shades as a fixed set up front. That's how you end up with 35 slightly different blues that all look the same.
28. **Never apply the same proportional padding to all button sizes.** Instead: use more generous padding for large buttons and tighter padding for small ones — they should feel different, not zoomed.

## 18. Leveling Up

### Look for Decisions You Wouldn't Have Made

Whenever you stumble across a design you really like, ask yourself: **"Did the designer do anything here that I never would have thought to do?"**

These unintuitive decisions are where the real design gold is:
- The way they inverted the background color on a datepicker to make it dark
- The way they positioned a button **inside** a text input instead of outside it
- Something as simple as using two different font colors for a headline
- Controls overlapping a carousel image instead of sitting below it
- A dropdown that uses a multi-column layout with icons and descriptions instead of a boring list

Paying attention to these sorts of decisions is a great way to discover new ideas that you can apply to your own work.

### Rebuild Your Favorite Interfaces

The absolute best way to notice the little details that make a design look polished is to **recreate it from scratch, without peeking at the developer tools.**

When you're trying to figure out why your version looks different from the original, you'll discover tricks on your own:
- "Reduce your line-height for headings"
- "Add letter-spacing to uppercase text"
- "Combine multiple shadows for depth"
- "Use a subtle inset shadow instead of a border on images"

By continually studying the work that inspires you with a careful eye, you'll be picking up design tricks for years to come.

---
