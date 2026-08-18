# Digital Business Card

Exercise 1 — a personal digital business card, built with **HTML and CSS only**.
No JavaScript, no frameworks, no build step.

## Live site

<https://guyhavia.github.io/digital-business-card/>

## Requirements checklist

| Requirement | Where |
| --- | --- |
| Full name and role / field | `index.html` — `.hero__name`, `.hero__role` |
| Profile photo | `index.html` — `.portrait`, `assets/portrait.jpg` |
| Short paragraph about myself | `index.html` — `#about` |
| GitHub link | `index.html` — `#contact` |
| LinkedIn link | `index.html` — `#contact` |
| Phone number | `index.html` — `#contact` (`tel:` link) |
| Email address | `index.html` — `#contact` (`mailto:` link) |
| Dark / light mode button | checkbox `#theme-toggle` + `:root:has(...)` in `css/styles.css` |
| Responsive (mobile → desktop) | `css/styles.css` §16, mobile-first, 5 breakpoints |
| Valid semantic HTML | `<header> <main> <section> <figure> <dl> <ol> <footer>` |
| CSS in an external file | all styling lives in `css/styles.css` |
| No JavaScript | zero `<script>` tags, zero `.js` files |

## How the theme switch works without JavaScript

A single hidden checkbox holds the state:

```html
<input class="theme-input" type="checkbox" id="theme-toggle">
```

The root element reads that state with `:has()` and swaps every design token
at once, so the whole page re-themes from one declaration block:

```css
:root                              { --bg: #0a0b0e; --fg: #eceef2; /* … */ }
:root:has(#theme-toggle:checked)   { --bg: #efede7; --fg: #14161b; /* … */ }
```

The visible control is a `<label for="theme-toggle">` styled as a switch. The
input itself stays in the accessibility tree and keyboard focus order — it is
hidden with opacity, not `display: none` — and focus is mirrored onto the track
with `#theme-toggle:focus-visible ~ .shell .switch__track`.

`color-scheme` is set alongside the tokens so form controls and scrollbars
follow the theme too.

## Structure

```
.
├── index.html
├── css/
│   └── styles.css
├── assets/
│   ├── portrait.jpg
│   ├── portrait@0.5x.jpg
│   └── favicon.svg
└── README.md
```

## Beyond the minimum

- Five content sections: about, experience, projects, skills, contact.
- Scroll-driven reveal animations using `animation-timeline: view()`, wrapped in
  `@supports` so unsupporting browsers just show the content.
- Full `prefers-reduced-motion` support — all motion collapses to nothing.
- Skip link, `:focus-visible` rings everywhere, `aria-hidden` on decorative
  glyphs, alt text on the portrait.
- `@media (hover: none)` block so hover states do not stick on touch devices.
- A print stylesheet that turns the page into a clean one-pager and expands
  link URLs in the margin.
- Layered backdrop: dot grid, film grain and an accent glow, all CSS-generated.
- Responsive portrait via `srcset` / `sizes`, so phones fetch the 500px file.

## Verification

| Check | Result |
| --- | --- |
| W3C Nu HTML validator | 0 errors, 0 warnings |
| Horizontal overflow, 320 → 1920px (11 widths) | none at any width |
| Theme switch, driven by real clicks | tokens, `color-scheme` and knob position flip both ways |
| WCAG AA contrast, both themes | every text/background pair ≥ 4.5:1 |
| Print | 4 pages, no clipped or blank content |

The W3C CSS validator reports three "property doesn't exist" errors for
`animation-timeline` and `animation-range`. Those are CSS scroll-driven
animations, which the validator has not implemented yet; they are already
wrapped in `@supports (animation-timeline: view())`, so browsers without
support simply skip them.

## Running locally

Open `index.html` in a browser — there is nothing to install or compile.

## Note on content

Per the exercise brief, the persona, contact details, photo and projects on this
card are fictional. The focus of the exercise is the implementation and the
design, not the truth of the content.
