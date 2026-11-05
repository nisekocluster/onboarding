# Niseko Village — Onboarding Book

Digital welcome book for the winter team. 38 pages, 10 languages.

**Everything is inside `index.html`**: the structure, the styles, the logic and
the text for all 10 languages. Only the images are separate, in `img/`.

There is nothing to compile. No npm, no build, no dependencies.

```
niseko-onboarding/
├── index.html      ← EVERYTHING is here (161 KB)
├── favicon.ico
├── fonts/          ← the flag font (for Windows)
└── img/            ← the photos and maps
```

---

## How to try it

**Double-click `index.html`**. It opens in the browser and works.

(Since there are no external code files, a local server is no longer needed. If
you still want one: `python3 -m http.server 8000`.)

---

## How to find your way around the file

`index.html` has about 3,200 lines, so you don't navigate it by reading: you
navigate it by searching. Open the file and use **Ctrl+F** (Cmd+F on Mac) with
these markers:

| Search for | To change |
|---|---|
| `SECCION: COLORES` | colors and typography |
| `SECCION: ESTILOS` | all the CSS |
| `SECCION: DENSIDAD` | a text that causes scrolling |
| `SECCION: ESTRUCTURA` | adding, removing or moving pages |
| `SECCION: AJUSTES` | speed, desktop threshold, starting language |
| `IDIOMA: ESPANOL` | the Spanish text |
| `IDIOMA: JAPONES` | the Japanese text |
| `SECCION: MOTOR` | the logic (normally left alone) |

The ten languages: `IDIOMA: ENGLISH`, `JAPONES`, `ESPANOL`, `TAILANDES`,
`TURCO`, `COREANO`, `CHINO`, `NEPALI`, `BIRMANO`, `VIETNAMITA`.

This same list is also at the top of `index.html` itself, in case you have the
file in front of you and not this document.

---

## How to edit a text

Search for the language (for example `IDIOMA: ESPANOL`), scroll down to `pages`,
and find the page number. Change what is between the tags:

```js
4: `
  <p>Hace mucho tiempo, Niseko era una región agrícola...</p>
`,
```

Each language has four parts:

```js
CONTENT.es = {
  ui:     { ... },   // buttons and menus: "Empezar a leer", "Contenidos"
  labels: { ... },   // chapter and section titles
  toc:    { ... },   // the table of contents
  pages:  { ... },   // the text of each page
};
```

### Tags you can use

| Tag | What it does |
|---|---|
| `<p>...</p>` | A paragraph |
| `<em>...</em>` | Italics |
| `<strong>...</strong>` | Bold |
| `<br>` | Line break |
| `<ul><li>...</li></ul>` | Bulleted list |
| `<div class="subsec">...</div>` | Blue subtitle |
| `<div class="alert-box">...</div>` | Red notice |
| `<div class="hotel-block"><div class="hotel-name">Name</div><p>...</p></div>` | Hotel block |
| `<a class="info-link" href="...">...</a>` | Small link |

### Rules when editing

- Texts go between **backticks** `` ` ``, not quotation marks. That way you can
  use `"` and `'` inside the text without any problem.
- If you need a literal backtick, write it as `` \` ``.
- Don't change the page numbers or remove the commas at the end.
- **Never write `</script>` inside a text.** It would cut the file in half. It
  is the only truly dangerous thing about having everything together.

### If the book stops loading

It is almost always a quotation mark or a comma. Open the browser console with
**F12**, *Console* tab: it tells you the exact line number.

Tip: save a copy of the file before a big batch of changes. With everything in
one file, a syntax error blanks the entire book, not just one language.

---

## How to change images

They are in `img/` with clear names: `p1.jpg`, `p3.jpg`, `p5.jpg`… The number
matches the page number.

**To replace a photo:** save the new one with the same name over the old one.
No code needs to be touched.

Recommended format: vertical JPG, around 960×1315 px, under 200 KB. The heavier
they are, the longer they take to load on staff phones.

Two images are special:

- `p23.jpg` is the landscape map that spans both pages on desktop.
- `p23-mobile.jpg` is the same image **physically rotated** so it reads
  vertically on a phone. If you change one, change the other.

---

## How to add, remove or move pages

Search for `SECCION: ESTRUCTURA`. Each line is a page:

```js
{ n: 5, type: 'img',  src: 'p5.jpg' },
{ n: 6, type: 'text', section: 'ourTeam' },
```

| `type` | What it is | Needs |
|---|---|---|
| `img` | Full-page image | `src` |
| `text` | Text page | the text in each language |
| `text-img` | Text on top, image below | `src` + the text |
| `split` | One image split across two pages | `src`, `half: 'left'` or `'right'` |
| `toc` | The table of contents (generated automatically) | — |
| `closing` | The final farewell page | — |

Extra options:

| Option | Effect |
|---|---|
| `cover: true` | The image fills the page even if it has to crop |
| `scroll: true` | Long image with vertical scrolling (schedules, maps) |
| `srcMobile: 'x.jpg'` | Uses a different image on mobile and tablet |
| `desktopOnly: true` | The page only appears in the desktop spread |
| `imgStyle: { maxWidth: '90%' }` | Extra styles for the image |

The titles (`eyebrow`, `title`, `section`, `subsection`) point to keys in
`labels`, so they translate automatically.

**If you add a text page**, add its text in all 10 languages. If it is missing
in any of them, the book shows the English instead of breaking. Also remember to
update the table of contents numbers (`toc.items`) in all 10 languages.

---

## How to change the behavior

Search for `SECCION: AJUSTES`:

```js
var CONFIG = {
  desktopMinWidth: 1024,   // from here up, two pages
  pageRatio:       0.72,   // proportions of one page
  mobilePageRatio: 0.58,   // on mobile the page is taller
  slideMs:         420,    // page-turn speed
  swipeThreshold:  45,     // how far the finger has to swipe
  defaultLang:     'en',   // starting language
};
```

If you change `slideMs`, also change `--slide-speed` in `SECCION: COLORES`.

---

## How to change the colors and typography

Search for `SECCION: COLORES`:

```css
:root {
  --paper:   #f8f5ef;   /* page background  */
  --ink:     #1a1a1a;   /* text color       */
  --accent:  #1a3a5c;   /* Niseko blue      */
  --accent2: #c8a96e;   /* gold             */
  --night:   #0d1b2a;   /* reader background */
  --red:     #b03a2b;   /* notices          */

  --font-display: "Fraunces", Georgia, "Times New Roman", serif;  /* titles */
  --font-body:    "Lato", ...;                                    /* text   */
}
```

`--font-display` is the font for the titles: the cover, "Chapter 1", the title
of each page, "Contents" and the final page. Change it here and it updates
across the whole book at once.

The fonts load from Google Fonts in the `<head>`. If the site has to work
without internet, delete those lines: the book will use Georgia and the system
font, and it still looks good (it only loses the distinctive touch of Fraunces).

### Links and shop names

`.info-link` (the links, such as the one to the town hall website or the shops)
is set to 18px.

The three shop names on the hanko page (Hanko Hiroba, Cocoroya, Ki Gift Shop)
use their own class, `.shop-name`, at 20px — deliberately separate from
`.subsec`, so that other subtitles in the book such as "Get here by Train" or
"Team Members Shuttle Bus", which stay at 14px, don't grow too.

If you add a new shop name in any language, use
`<div class="shop-name">...</div>` (not `class="subsec"`) so it inherits this
same size.

### The language selector on the cover

It is a normal browser `<select>` (search for `#langSelect` in the HTML and
`.lang-select` in the CSS) — not a grid of buttons. The dropdown, the highlight
on the chosen option and the scrolling when the 10 languages don't fit on screen
are all provided by the browser itself; there is nothing to program or adjust
for that.

The closed box is dark and translucent (`rgba(255,255,255,.09)` over the cover
background), with white text and the arrow in the site's gold (`--accent2`) — to
match the rest of the book. It has a fixed height (`height: 54px`) so it doesn't
change size when you choose a language with longer or shorter text (Burmese and
Vietnamese, for example).

The only customized part is the look of the closed box: the native arrow was
removed (`appearance: none`) and one was drawn by hand with
`.lang-select-wrap::after`. If you change the color or size of the box, remember
that this arrow is a separate element, not part of the `<select>`.

Technical note: the decorative background (`.welcome__aurora`) uses
`position: fixed` instead of `absolute` on purpose: if it goes back to
`absolute`, it creates a "phantom" scroll across the whole cover even though
nothing looks cut off (a known CSS bug when combining centering with overflow).

### The word "Chapter"

`.ch-eyebrow` controls the "Chapter 1" line (or its translation): it is blue
(the same `--accent` as the titles), italic, in the same font as the titles. The
first letter has no distinct formatting — the whole word uses exactly the same
style, size and color. In Japanese, Chinese, Korean, Thai, Nepali and Burmese it
stays upright (no italics), because those alphabets have no true italic form and
the browser fakes it by slanting the strokes, which looks crooked rather than
elegant — see the `:lang(...)` rule just below if you want to fine-tune which
languages behave this way.

### No scrolling on text pages

Text pages **must not cause scrolling**. If you add text and a page runs too
long, don't split the page: search for `SECCION: DENSIDAD` and tighten the
`line-height`.

All 10 languages have been checked on desktop, laptop, tablet and mobile with no
scrolling on any page. This includes the table of contents (page 2): its entries
no longer have a small fixed font size, they now grow like the rest of the book.

### The font size is the same on every page — except page 2

The paragraph size (`SECCION: DENSIDAD`) is uniform: the same on all text pages,
in all languages. It is around 24px on large screens.

Page 2 (the table of contents) and the "Contents" side menu are deliberately
**outside** this size: they use their own variable, `--toc-font-size` (in
`SECCION: COLORES`), and don't move even if you raise or lower the font size in
the rest of the book. Both share the same value, so they look the same.

`SECCION: AUTOAJUSTE` is the safety net: if a particular page ends up too long
for its space, it shrinks itself by the minimum needed to avoid scrolling. With
such a large base size, the two longest pages in the book (the hotels page and
the train schedule page) have to shrink quite a bit on the smallest phones — on
the narrowest one tested they end up smaller than the table of contents text.
This is an unavoidable compromise: you asked for 24px everywhere with no
scrolling anywhere, and those two pages have far more content than the others.
If you would rather they didn't shrink so much, the solution is not to touch the
CSS but to shorten the text on those two pages.

**Note on fonts.** Fraunces and Lato take a moment to arrive from Google Fonts.
Until they load, the browser measures the text with the fallback font (Georgia
or the system font), which doesn't have exactly the same line height. If the
automatic fit was calculated with that provisional measurement, it could fall
short as soon as the real font came into play — noticeable mainly right after
the page loads or right after changing language. Now, as soon as the browser
reports that the fonts have fully loaded (`document.fonts.ready`, in
`SECCION: MOTOR`), the size is recalculated with the final measurement. If that
isn't enough and some page still shows cut-off text in a particular language,
tell me the exact page and language and I'll look into it specifically.

**No scrollbar when the text fits exactly.** Even if a page shrinks to fit
exactly (without a pixel to spare), `SECCION: AUTOAJUSTE` removes its
`overflow-y: auto` as soon as it confirms it is no longer needed. Otherwise, some
systems (Windows, or macOS with "show scroll bars: always" turned on) draw a
fixed bar even when there is nothing to scroll. Only if a page truly doesn't fit
even at the minimum size is scrolling left on — better to be able to scroll than
to silently lose text.

```js
var FIT = {
  step:      0.5,   // the increments the size is tried in, in px
  growLimit: 0,      // 0 = uniform size; only shrinks, never grows
  shrinkMax: 13,      // the most it may shrink if it's a tight fit, in px
  floor:     11,      // never below this, no matter what
};
```

After changing any of these numbers, test the pages with the most text (the
garbage collection, the schedules, the hotels) on a small phone to confirm none
of them overflows.

### Jumping to a page by clicking in the table of contents

On both page 2 and the "Contents" side menu, tapping a chapter or a sub-entry
takes you straight to that page — both work the same way. If you add or move
pages (`SECCION: ESTRUCTURA`), you don't need to touch any of this: the jump uses
the page number from the table of contents itself.

---

## How to publish on GitHub Pages

```bash
git add .
git commit -m "Update texts"
git push origin main
```

In the repository's **Settings → Pages**, choose the `main` branch and the folder
that contains this `index.html`.

All paths are relative (`img/p1.jpg`), so it works the same at the root of the
domain as in a subfolder. No base path needs to be configured.

If you don't see the changes after uploading, it is the browser cache:
**Ctrl+Shift+R** (Cmd+Shift+R on Mac).

---

## Frequently asked questions

**I changed a text and now nothing loads.**
A quotation mark or a comma is missing. F12 → Console tells you the line.

**Japanese or Burmese characters show up as squares.**
The device doesn't have that font. Add the corresponding Noto font to the Google
Fonts link in the `<head>`.

**I want to add a new language.**
Copy the entire `CONTENT.en` block (from `IDIOMA: ENGLISH` to where the next
language starts), paste it below, change `CONTENT.en` to `CONTENT.pt`, translate
the content, and add `'pt'` to the `LANGS` list at the beginning of
`SECCION: MOTOR`.

**Can I split the files apart again?**
Yes. The CSS is all inside `<style>` and the JS inside `<script>`; moving them
out to `book.css` and `book.js` is just cut and paste.
