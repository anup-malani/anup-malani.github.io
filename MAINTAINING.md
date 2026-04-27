# Maintaining anupmalani.com

This document is the durable, repo-local guide to maintaining the site. It assumes nothing about the tooling that put it together. A future human or AI agent should be able to pick up the site from here without any external context.

> **AI-assisted workflow.** A set of Claude Code skills under `~/UChicago Law Dropbox/Anup Malani/assistants/research-manager/projects/website/skills/` automates everything below. If those skills are available, prefer them. If not, follow the manual instructions here.

---

## What this site is

- **URL:** `https://anupmalani.com` (custom domain) or `https://anup-malani.github.io` (default GitHub Pages URL).
- **Built with:** [al-folio](https://github.com/alshedivat/al-folio), a Jekyll-based academic theme. This repo is a fork; merging upstream changes occasionally is fine but conflicts in customized files (see "Customizations from upstream" below) need manual resolution.
- **Hosted by:** GitHub Pages, deployed to the `gh-pages` branch by the `Deploy site` GitHub Action on every push to `main`.
- **Owned by:** Anup Malani (`anup-malani` on GitHub).

## Repo layout

```
.
├── _bibliography/papers.bib         # ALL publications, in BibTeX. Source of truth.
├── _data/
│   ├── press.yml                    # Op-eds, blogs, podcasts, talks (Press page)
│   ├── substack.yml                 # Substack post archive (Substack page)
│   └── socials.yml                  # Social-icon links: email, X, LinkedIn, Scholar
├── _pages/
│   ├── about.md                     # Home page (bio + selected publications widget)
│   ├── publications.md              # Full publications list (rendered from papers.bib)
│   ├── press.md                     # Press list (rendered from press.yml)
│   ├── blog.md                      # Substack archive (renamed "Substack" in nav; rendered from substack.yml)
│   └── cv.md                        # CV download link
├── _includes/
│   ├── selected_papers.liquid       # Home-page Selected Publications widget
│   └── header.liquid                # Top nav + social icons
├── _layouts/about.liquid            # Home-page template (bio + photo + widgets)
├── _sass/                           # CSS overrides
├── assets/img/prof_pic.jpg          # Hero photo
├── _config.yml                      # Site config (title, scholar, plugins, ...)
├── CNAME                            # Custom domain pointer
└── .github/workflows/deploy.yml     # GitHub Actions build → gh-pages branch
```

## Source-of-truth map

| Data | Lives in | Format |
|---|---|---|
| Publications metadata | `_bibliography/papers.bib` | BibTeX |
| Press items | `_data/press.yml` | YAML |
| Substack archive | `_data/substack.yml` | YAML |
| Social links | `_data/socials.yml` | YAML |
| Bio | `_pages/about.md` | Markdown |
| CV PDF | `https://github.com/anup-malani/website_personal` (separate repo) | PDF |
| Publication PDFs | same — `website_personal/publications/*.pdf` | PDF |

### Why `_bibliography/` and `_data/` are separate

Forced by the al-folio + jekyll-scholar plugin architecture, not arbitrary:

- `_bibliography/papers.bib` is read by **jekyll-scholar**, which only accepts BibTeX from this exact path. The plugin handles category grouping, year sorting, the "selected papers" widget, citation badges, etc. — all the publication-rendering machinery on the site.
- `_data/*.yml` is Jekyll's generic data mechanism — accepts only YAML/JSON/CSV/TSV. Anything not in BibTeX goes here.

Don't try to merge them. Either you abandon jekyll-scholar (and lose the publications features) or you hack BibTeX-as-YAML (ugly).

---

## Build flow

1. Push to `main` → GitHub Action `Deploy site` runs (`.github/workflows/deploy.yml`).
2. Action installs Ruby 3.3.5, Bundler, Jekyll, ImageMagick, Python (for nbconvert).
3. Runs `bundle exec jekyll build` (production env). Writes to `_site/`.
4. Runs `purgecss` for CSS minification.
5. Pushes the built `_site/` to the `gh-pages` branch via `JamesIves/github-pages-deploy-action@v4`.
6. GitHub Pages serves from `gh-pages` branch (configured at `https://api.github.com/repos/anup-malani/anup-malani.github.io/pages`).

Total time: ~60–90 seconds from push to live.

**To trigger a rebuild without changing content:** `gh workflow run "Deploy site" --repo anup-malani/anup-malani.github.io --ref main`.

**To debug a failed build:** `gh run list --repo anup-malani/anup-malani.github.io --workflow "Deploy site" -L 5` then `gh run view <run-id> --log-failed`.

---

## Adding a publication (manual workflow)

If the AI-assisted `/add-paper` skill is unavailable, do this by hand:

### 1. Get the PDF onto GitHub

Save the paper's PDF to your local clone of the **other** repo: `~/github/website_personal/publications/<canonical-name>.pdf`.

Naming convention: `YYYY_FirstAuthorMalani_Venue_ShortTitle.pdf`
- Examples: `2024_MalaniEtAl_SciReports_CellullarImmunity.pdf`, `2025_LederLuisMalani_NBER33592_HealthcareFraud.pdf`, `2008_Malani_HarvLRev_LawAsLocalAmenity.pdf`

Commit + push:
```bash
cd ~/github/website_personal
git add "publications/<canonical-name>.pdf"
git commit -m "Add: <short-title> (<year>)"
git push
```

### 2. Add the BibTeX entry

Edit `_bibliography/papers.bib` in **this** repo. Append something like:

```bibtex
@article{citationkey,
  title    = {Paper Title},
  author   = {Lastname, Firstname and Lastname, Firstname},
  journal  = {Journal Name},
  volume   = {N},
  number   = {N},
  pages    = {N--N},
  year     = {YYYY},
  doi      = {10.xxxx/yyyy},                                                      % bare DOI, no URL prefix
  pdf      = {https://raw.githubusercontent.com/anup-malani/website_personal/main/publications/<canonical-name>.pdf},
  category = {peer-reviewed-health}                                               % see categories in _pages/publications.md
}
```

Other entry types: `@book` (publisher), `@incollection` (booktitle, editor, publisher, pages), `@techreport` (institution, number — use for NBER/SSRN/medRxiv/arXiv), `@misc` (legal briefs).

Categories (must match what's queried in `_pages/publications.md`): `peer-reviewed-health`, `peer-reviewed-statistics`, `peer-reviewed-econ-growth-dev`, `peer-reviewed-blockchain`, `peer-reviewed-law-reg`, `other-articles`, `books`, `conf-proceedings`, `reports`, `legal-briefs`, `working-papers`.

To feature on the home page, add `selected = {true}`.

### 3. Add the CV LaTeX entry

The CV is authored in LaTeX in `~/UChicago Law Dropbox/Anup Malani/Apps/Overleaf/Resume/`. Edit one of:

- **`Pubs_Sorted.tex`** for peer-reviewed papers — find the right `\subsection*{Health|Statistics|Economic growth and development|Blockchain|Law & regulation|Journal Articles}` block, insert in correct year position:
  ```latex
  \item[YYYY] \tab{}APA-formatted citation. \href{DOI-URL}{DOI-URL} \href{https://github.com/anup-malani/website_personal/blob/main/publications/<canonical-name>.pdf}{[PDF]}
  ```
- **`Pubs_Other.tex`** for working papers, books, book chapters, legal briefs, reports.

LaTeX-escape gotchas: `&` → `\&`; `_` inside `\href{}{}` second arg → `\_`; `#` → `\#`; `%` → `\%`. After editing, run `latexmk -pdf -interaction=nonstopmode main_sorted.tex` and confirm the compile is clean (15 pages or so, no `Misplaced alignment tab character &` errors).

### 4. Commit and push the site repo

```bash
cd <this-repo>
git add _bibliography/papers.bib
git commit -m "Add publication: <citation-key>"
git push
```

GitHub Action rebuilds in ~60–90s. Verify at https://anupmalani.com/publications/.

### 5. Recompile + push the CV (when ready)

The CV PDF is **not** rebuilt automatically. After adding 1–N papers:

1. `cd "/Users/amalani/UChicago Law Dropbox/Anup Malani/Apps/Overleaf/Resume" && latexmk -pdf -interaction=nonstopmode main_sorted.tex`
2. Mint dated filename: `Malani Resume Sorted YYMM.pdf` (where YYMM is the current year-month, e.g., `2604`).
3. Copy to `~/github/website_personal/`, commit + push to `website_personal`.
4. Edit `_pages/cv.md` in **this** repo: replace the previous YYMM in the GitHub blob URL with the new YYMM.
5. Commit + push this repo. GitHub Action rebuilds; the `/cv/` page now serves the new PDF.

Old dated CV PDFs stay in `website_personal/` forever — never delete them.

---

## Adding a press item

Edit `_data/press.yml`. Pick the right section (`op_eds`, `blogs`, `podcasts`, or `talks`). Insert at the top (most-recent-first ordering is curated by hand). Schema:

```yaml
- date: "Month DD, YYYY"   # verbatim string, not parsed
  title: "Piece title"
  venue: "Publication name"
  url: "https://..."
  coauthors: "Name1, Name2"   # optional
  pdf: "https://github.com/anup-malani/website_personal/blob/main/publications/<file>.pdf"   # optional
  host: "Host name"           # optional, for podcasts only
```

Commit and push. Site rebuilds.

---

## Adding a Substack post

Edit `_data/substack.yml`:

```yaml
posts:
  - date: "Mon DD, YYYY"   # e.g., "Apr 27, 2026"
    title: "Post title"
    url: "https://anupmalani.substack.com/p/post-slug"
```

Commit and push.

---

## Common issues

**Build fails with "Liquid Exception" in `_layouts/about.liquid`** — usually a missing or empty value in `_data/socials.yml`. The `jekyll-socials` plugin fails on nil values; ensure every key has a real value, or remove the key entirely.

**Build fails with "private method 'select' called for nil" in `_pages/publications.md`** — caused by `--query "@*[category=foo]"` syntax that some jekyll-scholar versions reject. Use `--group_by year` (no category filter) and rely on per-section `<h2>` headers instead.

**Custom domain not resolving over HTTPS** — DNS records must be:
- Apex `anupmalani.com` → A: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- `www.anupmalani.com` → CNAME `anup-malani.github.io`

After DNS change, GitHub Pages settings → Custom domain → wait ~10 min for cert provisioning. CNAME file in repo root must contain `anupmalani.com`.

**`pages-build-deployment` workflow keeps failing** — that's the default GitHub Pages builder, not our al-folio Action. It fails because al-folio uses Jekyll plugins not whitelisted by GitHub Pages' built-in builder. Ignore it; the `Deploy site` Action is what publishes.

---

## Customizations from upstream al-folio

This fork diverges from `alshedivat/al-folio` in:

- `_config.yml`: site identity (title, name, URL), scholar name, `enable_navbar_social: true`, `enable_publication_thumbnails: false`, `baseurl:` blank
- `_data/socials.yml`: real social handles (no `cv_pdf` here — CV is on the main nav menu)
- `_pages/about.md`: real bio (no subtitle — kept clean for layout)
- `_pages/publications.md`: simplified to default `{% bibliography %}` (year-grouped descending)
- `_pages/press.md`: new file rendering `_data/press.yml`
- `_pages/blog.md`: rewritten as Substack archive; permalink `/substack/`, title "Substack"
- `_pages/cv.md`: simplified to one download button
- Hidden pages (`nav: false`): `blog`, `projects`, `repositories`, `teaching`, `profiles`, `dropdown`, `books`
- `_news/`: removed two example announcements (kept one as a placeholder)
- `_sass/_components.scss`: `.more-info` font fixed to `inherit` (was monospace)
- `_sass/_navbar.scss`: navbar social icons sized down to 1.3rem
- `_sass/_publications.scss`: `h2.bibliography` color overridden to `rgba(0,0,0,0.55)` for legibility
- `_includes/header.liquid`: dropped visible "ctrl k" label from search button (icon only)
- `_includes/selected_papers.liquid`: changed to `--group_by year` so home-page widget is chronological
- `_layouts/about.liquid`: dropped subtitle paragraph; dropped contact-note footer; added `margin-top: 2.5rem` on Selected Publications header
- `_bibliography/papers.bib`: replaced upstream Einstein examples with Anup's full publication list (101+ entries)
- `assets/img/prof_pic.jpg`: replaced placeholder with Anup's headshot

To pull upstream updates: `git remote add upstream https://github.com/alshedivat/al-folio.git && git fetch upstream && git merge upstream/main`. Resolve conflicts by hand in the files above.

---

## Tooling assumptions

- `git` and `gh` CLI configured for the `anup-malani` GitHub account
- TeX Live 2024 with `latexmk` and `pdflatex` for the CV side (`/Library/TeX/texbin/`)
- Ruby 3.3.5 and Bundler for local Jekyll preview (`bundle exec jekyll serve`) — but local preview is rarely needed; the GitHub Action handles production builds
- ImageMagick if running `bundle exec jekyll build` locally (the Action installs it)

## Pointer to AI workflow

If `~/UChicago Law Dropbox/Anup Malani/assistants/research-manager/` exists, the `projects/website/skills/` folder has SKILL.md files that automate the steps above. The four skills:

- `/add-paper` — DOI / PDF in, papers.bib + LaTeX + GitHub PDF out
- `/add-press-item` — URL in, press.yml entry out
- `/update-cv-only` — for positions, awards, talks (no website publication entry)
- `/publish-cv` — recompile main_sorted.tex, push the dated CV PDF, update /cv/ link

Each skill includes its own approval gate, so nothing publishes without explicit "yes". If you're a future AI agent, read those SKILL.md files for the orchestration logic.
