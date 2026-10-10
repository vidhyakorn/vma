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
  ndls\                      the NDLS project section
    _index.md                Overview
    progress.md              the timeline
    outputs.md               papers and essays tagged ndls
  blog\                      Writing — essays, dated, newest first
  posts\_index.md            the "More →" page: everything, newest first
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
  badge = 'reach out'                # the small pill at the top, with the green dot
  badgeLink = '/discuss'
  headline = ' '                     # big line under the pill
  subtitle = ' "an acorn's telos…" '  # the quotation
  intro = "What's on your mind?"     # one short paragraph
  #buttonText = 'reach out'          # a line starting with # is switched off
  #buttonLink = '/discuss'

  latest = 6                         # how many cards in What's New
  latestTitle = "What's New"         # the green heading
  latestMore = 'More'                # the green link beside it
  latestLink = '/posts'              # where that link goes
  feedExclude = ['about', 'discuss', 'posts']
```

Leave any line out and that piece simply isn't shown. Put a `#` at the start of
a line to switch it off without deleting it.

**Watch the quotes.** A value with an apostrophe in it — `What's` — must be in
double quotes, or the site will not build.

### What's New, and /posts/

**What's New** is every post on the site in one row, newest first: papers,
essays, methods notes, project write-ups, and anything in a section you add
later. You never list them; a new file appears there by itself. **More →** goes
to `/posts/`, which is the same list with nothing left off.

`feedExclude` is the only thing that keeps a page out: it names the top folders
that hold pages rather than posts. `about`, `discuss` and `posts` are in it
already — add a folder name there if you make another page of that kind.

To put a post at the top of the row, give it a later `date:`. Order is by date
alone.

## Colours

Monochrome, with one green for anything that should stand out. At the top of
`assets\css\custom.css`:

```css
:root {
  --primary-hue: 0deg;
  --primary-saturation: 0%;       /* 0% = Hextra's whole colour scale is grey */
  --primary-lightness: 11.3%;

  --ink: #111111;           /* headings, links, text */
  --muted: #555555;         /* dates, captions, card lines */
  --accent: #31A354;        /* the green: badge dot, link underlines,
                               active sidebar bar, card hover line */
  --accent-soft: #A1D99B;   /* the mid green, used in the card pictures */
  --accent-wash: #E5F5E0;   /* the palest green: behind a card's tag */
  --accent-text: #288545;   /* green words: What's New, More →, tags */
  --accent-fill: #288545;   /* green behind white text: buttons */
  --on-accent: #FFFFFF;     /* the text on --accent-fill */
}
```

The three `--primary-*` numbers are how Hextra works: it builds its entire
colour scale — search, focus rings, the active sidebar item — from one hue,
saturation and lightness. Saturation `0%` makes all of it grey, which is what
keeps the theme monochrome everywhere I haven't restyled by hand.

**Why the greens differ.** `#31A354` is only 3.1 : 1 against white, so it is
used where no text is involved: lines, dots, underlines, the pictures. Where
the green *is* the text, or sits behind white text, it is `#288545`, which
reads at 4.6 : 1. Links are black with a green underline, so the colour is in
the line, never in the words.

To swap the green for another colour, change `--accent`, then pick a darker
version for `--accent-text` and `--accent-fill` and check each reaches 4.5 : 1
at [webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/).

The card pictures are tinted separately, in
`layouts\_partials\custom\head-end.html` — see **Cards** below.

Dark mode has its own values just below, under `.dark { … }`. On a black
background the lighter green is readable as text, so there `--accent-text` is
`#A1D99B`.

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

One card shape is used everywhere — What's New, `/posts/`, and each section's
own page. It is a wide, short card: the picture on the left, then a green tag
saying what kind of post it is, the date, the title and a line or two of
summary.

**The tag** is the name of the folder the post is in — Publications, Writing,
Methods, Projects — taken from that folder's `_index.md` title. Rename the
folder's title and every tag changes with it. A new folder gets its own tag
with nothing to set up.

**The date** is the `date:` in the post's front matter, shown as `20 Sep 2026`.
A post with no date shows no date, and sorts last.

**Pictures** — first one filled in wins:

1. `cover: "/img/picture.jpg"` — a file in `static\img\`. The path starts at
   `/img/`, never `/static/img/`.
2. `cover: "https://example.org/picture.jpg"` — any image on the web.
3. `link: "https://journal.org/article"` — the site uses that page's preview
   image.
4. Nothing — a plain grey panel the same shape as a picture.

Every picture is cropped to the same 16 : 9 box, converted to a small WebP, and
run through a **duotone filter** that flattens it to two greens and a pale
neutral, so a row of very different photographs still reads as one set. If a
path is wrong, the card falls back to the grey panel and the build log names
the file it couldn't find.

The filter is in `layouts\_partials\custom\head-end.html`, as two short SVG
blocks: `#duotone` for light mode and `#duotone-dark` for dark. Each has three
`tableValues` numbers per colour — the shadows, the midtones and the
highlights, in that order. To make the pictures less green, move the numbers
closer to each other; to turn the effect off, delete these two lines from the
`CARDS` section of `custom.css`:

```css
.feed-card img.feed-card-image { filter: url(#duotone); }
.dark .feed-card img.feed-card-image { filter: url(#duotone-dark); }
```

**Shape and size** — in `custom.css`, the `CARDS` section:

```css
:root { --card-h: 135px; }                            /* height of every card */
.feed-card .feed-card-image { width: 38%; max-width: 170px; }   /* the picture */
.feed-card .feed-card-title { font-size: 0.85rem; -webkit-line-clamp: 2; }
.feed-card .feed-card-sub   { font-size: 0.78rem; -webkit-line-clamp: 2; }
```

`--card-h` resizes every card at once. Two lines of title and two of summary is
what fits at 135px; allow a third line and `--card-h` has to grow with it, or
the text is cut off.

**Card text** — a publication's card shows where and when it appeared, taken
from its DOI. Anything else shows its `description:`, or the start of the text.

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

## The NDLS project section

NDLS has its own top-level section, separate from Research, because it is a
project rather than a list of papers. It has three pages:

| Page | File | What it is |
|---|---|---|
| Overview | `content\ndls\_index.md` | what the project is and why |
| Progress | `content\ndls\progress.md` | the timeline, edited by hand |
| Outputs | `content\ndls\outputs.md` | everything tagged `ndls`, gathered automatically |

### Adding something to Outputs

Nothing to edit. Put `ndls` in a paper's or a post's tags:

```yaml
tags: [ndls, modelling]
```

and it appears on the Outputs page, while staying where it already was under
Publications or Writing. The newest is first.

That is what the `tagged` shortcode does, and it works for any tag:

```
{{</* tagged tag="ndls" */>}}
{{</* tagged tag="ndls" empty="Nothing yet." */>}}
```

### Adding to Progress

Each entry on the timeline is one block:

```
{{</* milestone date="September 2026" status="done" */>}}
**What happened.** A sentence or two about it.
{{</* /milestone */>}}
```

`status` is one of three, and only changes the dot:

- `done` — a filled green dot
- `now` — a green ring, for what you are working on
- `next` — a hollow grey dot, for what is planned (this is the default)

Put the newest entry at the top. `date` is free text, so "September 2026",
"Now" and "Later" are all fine.

### The logo

`static\img\ndls-logo.png`, shown by `{{</* ndls-mark */>}}` at the top of the
Overview. It sits on a white tile, because the mark is drawn on white and
would otherwise show a hard square on a dark page. Its size is the `110px` in
the `NDLS` section of `custom.css`. To use it elsewhere:
`{{</* ndls-mark src="/img/other.png" */>}}`.

### Starting another project section

Copy the `content\ndls\` folder, rename it, change the titles, pick a new tag
for its Outputs page, and add it to `[[menu.main]]` in `hugo.toml`. Also add
the folder name to `feedExclude` under `[params.home]`, so the project's own
pages stay out of What's New — the papers and essays tagged to it still appear
there, which is the point.

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

Two things reliably stop the build, both with a long error naming the file:

- an apostrophe inside `'single quotes'` in `hugo.toml` — use "double quotes";
- a comma between a shortcode's settings — use spaces.

And one for me rather than you: in a template, never write
`{{ with partial "x" . }}`. Put the result in a variable first
(`{{ $x := partial "x" . }}`), or Hugo 0.166 fails with a garbled error about
something being nil.

## Seeing changes instantly

Instead of pushing and waiting, run the site on your own machine:

```
cd D:\1project\vidhyakorn_website
hugo server
```

Open http://localhost:1313/vma/ and every save shows up at once. Stop it with
Ctrl+C. Publication details still need an internet connection to be fetched.
