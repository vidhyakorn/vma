# Changing how the site looks

The site uses the [Hextra](https://imfing.github.io/hextra/docs/) theme, laid
out after [developers.osuny.org](https://developers.osuny.org/) (which is itself
built on Hextra). Nearly everything you will want to change lives in two files:

| What | File |
|---|---|
| Homepage text, menu, icons, site title | `hugo.toml` |
| Colours, card look, spacing, anything finer | `assets\css\custom.css` |

`themes\hextra\` is the theme. **Never edit anything in there** — a theme update
would overwrite it. Everything in `layouts\` at the top level is a copy that
overrides the theme's version, and those are safe to edit.

After any change: save, commit and push in GitHub Desktop. The site rebuilds by
itself and is live in two or three minutes.

---

## Where things live

```
content\
  _index.md                  the homepage (its text is in hugo.toml, see below)
  research\                  everything in the left-hand sidebar
    _index.md                the Research overview page
    publications\            one .md per paper
    projects\                ongoing research
    methods\                 methods notes
  blog\                      Writing — essays, dated, newest first
  about\index.md             About, with your photo and CV
static\img\                  pictures
static\cv\vidhyakorn-cv.pdf  the CV shown on About
```

**The folder decides where a page appears.** A file in
`content\research\projects\` shows up in the sidebar under Projects; a file in
`content\blog\` shows up under Writing. There is no category setting inside the
file.

To add a new sidebar group, make a folder under `content\research\` with an
`_index.md` in it:

```yaml
---
title: "Teaching"
description: "Courses and workshops."
weight: 4          # position in the sidebar: 1 is first
---
```

---

## The homepage

In `hugo.toml`, under `[params.home]`:

```toml
[params.home]
  badge = 'MORU · Bangkok'           # the small pill at the top, with the green dot
  badgeLink = '/about'
  headline = 'τὸ τέλος τοῦ βαλάνου ἐστὶ τὸ γενέσθαι δρῦς'
  subtitle = '“An acorn’s telos is to become an oak tree.” — Aristotle'
  intro = 'I work on …'              # one short paragraph
  buttonText = 'Read my research'
  buttonLink = '/research'
  publications = 3                   # how many cards in each grid
  writing = 3
```

Leave any line out and that piece simply isn't shown. The card grids under the
button — latest publications, latest writing — fill themselves in from your
content; you never edit them. Anything you write in `content\_index.md` appears
below the grids.

---

## Colours

Monochrome, with green for anything that should stand out. At the top of
`assets\css\custom.css`:

```css
:root {
  --primary-hue: 0deg;
  --primary-saturation: 0%;       /* 0% = Hextra's whole colour scale is grey */
  --primary-lightness: 11.3%;

  --ink: #111111;          /* headings, links, text */
  --muted: #555555;        /* dates, captions, card lines */
  --accent: #31A354;       /* the strong green: badge dot, link underlines,
                              active sidebar bar, card hover line */
  --accent-soft: #A1D99B;  /* the mid green: tints, the letter on card tiles */
  --accent-wash: #E5F5E0;  /* the pale green: panel and hover backgrounds */
  --accent-text: #288545;  /* green words: "Abstract", "All publications →" */
  --accent-fill: #288545;  /* green behind white text: buttons, filled pills */
  --on-accent: #FFFFFF;    /* the text on --accent-fill */
}
```

The three `--primary-*` numbers are how Hextra works: it builds its entire
colour scale — search, focus rings, the active sidebar item — from one hue,
saturation and lightness. Saturation `0%` makes all of it grey, which is what
keeps the theme monochrome everywhere I haven't restyled by hand.

**Why the greens differ.** `#31A354` is 3.2 : 1 against white — enough for a
2px underline or a dot, not enough for words, and white words on it are just
as hard to read. So it is used only where no text is involved. Buttons and
anything else filled with green use `#288545`, deep enough that their white
text reads at 4.6 : 1, and that same `#288545` is used where green *is* the
text. `#A1D99B` and `#E5F5E0` are the two pale greens, for tints and
backgrounds that never carry small text.

To change the green, swap `--accent`, then check whatever you choose for
`--accent-fill` reaches 4.5 : 1 against white at
[webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/).

Dark mode has its own values just below, under `.dark { … }`. On a black
background the pale green reads easily, so there green words use `#A1D99B`.

---

## Fonts

| Used for | Font | Where it comes from |
|---|---|---|
| Headings, menu, buttons, card titles | Linux Biolinum O → Libertinus Sans | `static\fonts\` |
| Body text | Spectral | `static\fonts\` |

**About Linux Biolinum O.** It isn't published anywhere a website can load it
from, so the site asks for it first — anyone who has it installed (it ships with
TeX Live and LibreOffice) sees it — and otherwise uses **Libertinus Sans**, the
maintained fork of Biolinum: the same letterforms, with years of fixes since. It also covers polytonic Greek,
which the homepage headline needs.

To serve the original Biolinum to everyone instead, copy `LinBiolinum_R.otf`
and `LinBiolinum_RB.otf` from your TeX Live folder
(`texmf-dist\fonts\opentype\public\libertine\`) into `static\fonts\` and ask me
to wire them in.

Both fonts are kept in the repo rather than loaded from Google, so nothing is
fetched from a third party.
They are loaded in `layouts\_partials\custom\head-end.html`, and assigned in the
`FONTS` section of `custom.css`:

```css
--font-heading: 'Linux Biolinum O', 'Linux Biolinum', 'Libertinus Sans', …;
--font-body: 'Spectral', Georgia, 'Times New Roman', serif;
```

Libertinus Sans comes in regular, italic and bold only, so headings are bold —
asking for semibold would make the browser fake it.

---

## Cards

**Pictures** — first one filled in wins:

1. `cover: "/img/picture.jpg"` — a file in `static\img\`. The path starts at
   `/img/`, never `/static/img/`.
2. `cover: "https://example.org/picture.jpg"` — any image on the web.
3. `link: "https://journal.org/article"` — the site uses that page's preview
   image.
4. Nothing — a dark tile with the title's first letter in green. The shade
   is chosen from the page's address, so each post keeps the same tile.

Every picture is cropped to the same 16 : 9 box and converted to a small WebP
when the site builds, so a 300 KB photo arrives as a few kilobytes and a grid
stays even. If a path is wrong, the card falls back to the tile and the build
log names the file it couldn't find.

**Shape and size** — in `custom.css`, the `CARDS` section. The picture sits
beside the text, and every card in a row is the same height:

```css
:root { --card-h: 135px; }                                  /* card height */
.hextra-card .hextra-card-image { width: 38%; }             /* picture width */
.hextra-card .hextra-card-icon     { -webkit-line-clamp: 2; }  /* title lines */
.hextra-card .hextra-card-subtitle { -webkit-line-clamp: 2; }  /* summary lines */
```

Two lines of title and two of summary is what fits in 135px. Allow a third
line of either and you have to raise `--card-h` to match, or the extra line
is cut off.

**Card text** — a publication's card shows where and when it appeared, taken
from its DOI. Anything else shows its `description:`, or the start of the text.

---

## Publications — no typing

In `content\research\publications\`, a new file needs only this:

```yaml
---
date: 2026-09-22
doi: "10.1186/s12889-025-25811-5"
tags: [modelling]
---
```

When the site builds, the title, every author, the journal, the year and the
abstract are fetched and put on the page, with "Read the paper" and DOI
buttons. The title also appears in the sidebar, the card and the browser tab.
Journal DOIs come from Crossref; figshare, Zenodo and other repository DOIs from
DataCite. With no DOI, use `source: "https://…"` and the page's own citation
tags are read instead.

Anything you write yourself wins over what is fetched:

- `title:` — a different title everywhere
- `linkTitle:` — a short label for the sidebar only, e.g. `"PHPIT, 2025"`
- `description:` — replaces the fetched line under the card
- text in the body — replaces the fetched abstract on the page

`content\_templates\new-publication.md` is a ready-made starting point.

---

## Pictures and diagrams in the text

```markdown
![What the diagram shows](/img/model-structure.png)
```

That fills the column. To size it or add a caption:

```markdown
{{< figure src="/img/model-structure.png"
           alt="Compartments and the flows between them"
           caption="**Figure 1.** Structure of the model."
           width="520" >}}
```

`width` is in pixels; `align="left"` or `"right"` wraps the text around it.
**Separate the settings with spaces, not commas** — a comma stops the whole site
from building.

To paste from Obsidian, make the post a folder (`hanami2026\index.md`) and keep
its pictures beside it; then `![](diagram.png)` needs no path. Obsidian's own
`![[diagram.png]]` does not work.

---

## Menu and icons

The `[[menu.main]]` blocks in `hugo.toml`, left to right by `weight`. An entry
with an `icon` shows as an icon:

```toml
[[menu.main]]
  name = 'ORCID'
  url = 'https://orcid.org/0009-0008-2468-8870'
  weight = 5
  [menu.main.params]
    icon = 'orcid'
```

Hextra has icons for GitHub, LinkedIn, X, Mastodon, Bluesky and more. ORCID and
Google Scholar were added in `data\icons.yaml`; add others there the same way.

`[[menu.sidebar]]` blocks add links to the bottom of the Research sidebar.

---

## About page

Your photo is `content\about\profile.jpg`. Its size on the page is set in
`custom.css` under `ABOUT PAGE` — change `200px`. The CV comes from
`static\cv\vidhyakorn-cv.pdf`; export a new one from Typst over the top of it.

---

## When a change seems to do nothing

1. **Wait for the build.** Pushing is not publishing. On GitHub, open
   **Actions** and wait for the green tick.
2. **Check the build didn't fail.** A red cross means the live site is still
   the previous version. Click it — the error names the file and the line.
3. **Hard refresh** with Ctrl+F5. The stylesheet is named after a hash of its
   contents, so it can't be stale, but a page can be cached for a minute.
4. **Check you edited the right file** — never a file under `themes\`.

## Seeing changes instantly

Instead of pushing and waiting, run the site on your own machine:

```
cd D:\1project\vidhyakorn_website
hugo server
```

Open http://localhost:1313/vma/ and every save shows up at once. Stop it with
Ctrl+C. Publication details still need an internet connection to be fetched.
