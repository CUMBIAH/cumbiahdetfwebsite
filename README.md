# cumbiahdetfwebsite

A simple [Quarto](https://quarto.org) website for the analysis documents used in the CUMBIAH
detection function project.

The site is built from `.qmd` (Quarto markdown) source files in this repository and published
automatically to GitHub Pages whenever changes are pushed to `main`.

## Contents

- [What you need](#what-you-need)
- [Opening the project in RStudio](#opening-the-project-in-rstudio)
- [Previewing the site](#previewing-the-site)
- [Repository layout](#repository-layout)
- [Editing an existing page](#editing-an-existing-page)
- [Adding a new analysis document](#adding-a-new-analysis-document)
- [Adding images](#adding-images)
- [Changing the navbar and site settings](#changing-the-navbar-and-site-settings)
- [Publishing changes](#publishing-changes)
- [Troubleshooting](#troubleshooting)

## What you need

- **R** — https://cran.r-project.org
- **RStudio** (2022.07 or newer) — https://posit.co/download/rstudio-desktop/.
  Recent versions of RStudio ship with Quarto built in, so there is usually nothing else to install.
- **Quarto** (only if your RStudio is older, or you want the latest version) — https://quarto.org/docs/get-started/
- **Git** — either the Git client built into RStudio, or GitHub Desktop / the command line.

To check Quarto is available, run this in the RStudio **Console**:

```r
quarto::quarto_version()
```

If the `quarto` R package is missing, install it with `install.packages("quarto")`. The package is
only a convenience wrapper — RStudio's Build pane works without it.

## Opening the project in RStudio

1. Clone the repository (once):
   - In RStudio: **File → New Project → Version Control → Git**, and use
     `https://github.com/CUMBIAH/cumbiahdetfwebsite.git` as the repository URL.
   - Or on the command line: `git clone https://github.com/CUMBIAH/cumbiahdetfwebsite.git`
2. Open `cumbiahdetfwebsite.Rproj` (double-click it, or **File → Open Project** in RStudio).

Always work with the `.Rproj` file open. It sets the working directory to the repository root,
which is what makes the relative paths in the `.qmd` files (images, links) resolve correctly, and
it makes RStudio show the **Build** and **Git** panes for this project.

## Previewing the site

With the project open, use the **Build** pane in the top-right of RStudio:

- **Render Website** — builds the whole site into `_site/` and opens it in RStudio's Viewer or your
  browser.
- **Preview Website** — same, but keeps running and live-reloads the browser each time you save a
  `.qmd` file. This is the most comfortable way to work: open the preview, edit, save, and watch
  the page update.

Press the **Stop** (red square) button in the Console or Build pane to end a preview.

You can also render just the file you are editing: open the `.qmd` and click **Render** above the
editor (or press <kbd>Ctrl/Cmd</kbd> + <kbd>Shift</kbd> + <kbd>K</kbd>). This is quicker than
rebuilding the whole site when you are working on one long document.

The equivalents from the Console are:

```r
quarto::quarto_preview()   # live preview of the whole site
quarto::quarto_render()    # one-off build of the whole site
```

Or from a Terminal tab: `quarto preview` / `quarto render`.

### Visual vs Source editor

Each `.qmd` file can be edited in either mode, using the **Source** / **Visual** toggle at the
top-right of the editor pane:

- **Visual** is a word-processor-like view — handy for tables, links and pasting images.
- **Source** shows the raw markdown — necessary for the YAML header at the top of a file and for
  anything with Quarto-specific syntax such as `::: {.callout-note}` blocks.

Both edit the same file, so you can switch back and forth freely.

## Repository layout

```
.
├── _quarto.yml            # site configuration: title, navbar, output dir, format options
├── index.qmd              # home page; lists the analysis documents automatically
├── links.qmd              # "Useful Links" page
├── analysisdocs/          # one .qmd per analysis document — new pages go here
│   ├── pamguard.qmd
│   ├── localiseporpoises.qmd
│   └── locatelanders.qmd
├── images/                # images used by the site
│   ├── cumbiah-icon.svg
│   ├── 250701_logo_cumbiah.png
│   └── pamguard/          # images for a specific document, in their own folder
├── .github/workflows/     # GitHub Actions workflow that builds and publishes the site
├── _site/                 # rendered output — generated, not committed
└── .quarto/               # Quarto's cache — generated, not committed
```

`_site/` and `.quarto/` are in `.gitignore`. Never edit anything inside them: they are overwritten
on every render.

## Editing an existing page

1. Open the `.qmd` file from the **Files** pane.
2. Edit the text. Standard markdown applies (`##` for headings, `**bold**`, `[text](url)` for
   links), plus [Quarto's extras](https://quarto.org/docs/authoring/markdown-basics.html) such as
   callout blocks and figure options.
3. Save, and check the result in the preview.

The block at the very top of each file, between the `---` lines, is the **YAML header**. It
controls the page's metadata, for example in `analysisdocs/pamguard.qmd`:

```yaml
---
title: "Tracking Porpoises with PAMGuard"
description: "Detecting, localising and exporting harbour porpoise clicks from ..."
author: "Jamie Macaulay"
image: ../images/pamguard/image35.png
toc: true              # show a table of contents for this page
toc-depth: 3
number-sections: true  # number the headings
lightbox: true         # click images to enlarge them
fig-align: center
---
```

YAML is indentation-sensitive — use spaces (never tabs), and keep the `---` lines intact. If a
render fails immediately after you edited a header, the header is the first place to look.

## Adding a new analysis document

The home page (`index.qmd`) uses a Quarto **listing** that picks up everything in `analysisdocs/`
automatically, so you do not need to register a new page anywhere.

1. Create a new file in `analysisdocs/`, for example `analysisdocs/myanalysis.qmd`
   (**File → New File → Quarto Document**, then save it into that folder). Use a short, lowercase
   filename with no spaces — it becomes part of the page's URL.
2. Give it a YAML header with at least a title, a description and an image. The description and
   image are what appear on the home page card:

   ```yaml
   ---
   title: "My New Analysis"
   description: "One-sentence summary shown on the home page."
   image: ../images/cumbiah-icon.svg
   toc: true
   ---
   ```
3. Write the content below the header.
4. Render the site and check the new card appears on the home page and that the page itself looks
   right.

Cards on the home page are sorted by title (set by `sort: "title"` in `index.qmd`).

## Adding images

Put images in `images/`. For a document with many figures, give it its own subfolder — as
`images/pamguard/` does — to keep things tidy.

Paths are relative to the `.qmd` file that uses them:

- from `index.qmd` or `links.qmd` (repository root): `images/cumbiah-icon.svg`
- from a file in `analysisdocs/`: `../images/cumbiah-icon.svg`

Insert one with:

```markdown
![Caption text](../images/pamguard/image1.png){fig-alt="Description for screen readers" width="600"}
```

In the Visual editor you can also paste an image straight from the clipboard; RStudio will ask
where to save the file, and you should point it at the appropriate `images/` folder.

## Changing the navbar and site settings

Site-wide settings live in `_quarto.yml`. To add a page to the top navigation bar, add an entry
under `website: navbar: left:`:

```yaml
website:
  title: "CUMBIAH DETECTION FUNCTION"
  navbar:
    left:
      - href: index.qmd
        text: Home
      - href: links.qmd
        text: Useful Links
```

Pages in `analysisdocs/` do **not** need a navbar entry — they are reached from the home page
listing. After changing `_quarto.yml`, restart the preview so the new configuration is picked up.

## Publishing changes

The site deploys itself. Nothing needs to be rendered or uploaded by hand.

1. Commit your changes in the RStudio **Git** pane: tick the changed files, click **Commit**,
   write a message, then **Push**.
2. Pushing to `main` triggers the workflow in `.github/workflows/publish.yml`, which renders the
   site on GitHub and publishes `_site/` to GitHub Pages.
3. Progress and any build errors appear under the **Actions** tab of the
   [repository on GitHub](https://github.com/CUMBIAH/cumbiahdetfwebsite). The live site updates a
   minute or two after the workflow finishes.

Only commit source files — `.qmd` files, images, `_quarto.yml`. The `_site/` and `.quarto/`
folders are ignored on purpose; if they ever show up in the Git pane, do not stage them.

Because pushing to `main` publishes immediately, always preview locally before pushing. For larger
changes, work on a branch and open a pull request instead.

## Troubleshooting

| Problem | What to try |
|---|---|
| **Build pane has no "Render Website" button** | The `.Rproj` file is not open. Open `cumbiahdetfwebsite.Rproj` rather than the individual files. |
| **`quarto: command not found`** | Quarto is not installed or not on your PATH. Install it from https://quarto.org/docs/get-started/ and restart RStudio. |
| **Images do not appear** | Check the relative path — files in `analysisdocs/` need `../images/...`. Filenames are case-sensitive on the GitHub build server even if they work on Windows. |
| **Render fails right after editing a header** | Check the YAML: matching `---` lines, spaces not tabs, and quotes around any title containing a colon. |
| **Preview looks stale** | Stop the preview and start it again; deleting the `_site/` and `.quarto/` folders forces a clean rebuild. |
| **Site renders locally but the GitHub build fails** | Open the failing run under the repository's **Actions** tab — the log shows which file and line caused the error. |
