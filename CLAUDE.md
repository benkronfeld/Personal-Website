# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project state

V1 is built and pushed. `personal_website_project_spec.md` remains the authoritative source for product requirements, content, markup structure, styling, and deployment. The site is a static `index.html` + `style.css` pair, initialized as a git repo with `main` tracking `https://github.com/benkronfeld/Personal-Website`.

Two V1 items are still pending and are owned by the user: adding `assets/benkronfeldresumeoctober.pdf` (the Resume link 404s until it exists) and importing the repo into Vercel (no Vercel CLI is installed; deployment is done in the dashboard).

When working here, treat the spec as the contract and implement against it — do not invent pages, sections, or links beyond what V1 scopes.

## What this is

A one-page personal site for Ben Kronfeld, reachable from social/LinkedIn/resume traffic. A visitor sees a short bio, a reach-out email line, and four links (LinkedIn, Instagram, GitHub, Resume) and nothing else. No navigation, no accounts, no learning curve.

Stack: plain HTML and CSS, no framework, no build step, no dependencies, no JS in V1. Hosted on Vercel from a GitHub repo, with auto-deploy on push to `main`. Custom domain is deferred.

## Project architecture overview

There is no application architecture in the usual sense — no server, no database, no application framework, no build pipeline, no client-side state. The whole product is a static document plus its presentation, delivered by a CDN. Understanding the two axes below is enough to work anywhere in the repo.

**Runtime (what happens on a request):**

```
Browser ──GET /──────────────▶ Vercel CDN ──▶ index.html
        ──GET /style.css─────▶            ──▶ style.css
        ──GET /assets/*.pdf──▶            ──▶ assets/benkronfeldresumeoctober.pdf (new tab)
        ──GET /favicon.ico───▶            ──▶ favicon.ico (optional)
```

Vercel serves files straight from the repository root with no transformation. There is no routing, no serverless function, no API call, and no JavaScript execution. Every byte the visitor receives is a file committed to the repo.

**Composition (how the document is built):**

```
index.html
├── <head>      title, meta description, viewport, Open Graph tags   → social share previews
├── <main>
│   ├── <h1>    name
│   ├── <p>     short bio
│   ├── <p>     reach-out email (mailto)
│   └── <nav>   LinkedIn · Instagram · GitHub · Resume
└── <link>      style.css                                            → all presentation
```

The separation is strict: `index.html` owns content and semantics, `style.css` owns every visual decision, and `assets/` holds binaries. Nothing crosses those boundaries — no inline styles, no content in CSS, no HTML strings anywhere else.

**Change and deploy flow:**

```
edit files → feature branch → commit → push branch → merge to main
                                                        │
                                                        ▼
                                             Vercel auto-deploy
                                                        │
                                                        ▼
                                               public *.vercel.app URL
```

The deploy step is a no-op build: Vercel copies the repo and publishes it. This is why a broken link or an unstyled page shows up immediately in production rather than failing a build — verification happens by opening the deployed URL, not by a compiler or test suite.

**Architectural invariants** (violating any of these changes the deployment model, not just the code):

- Zero dependencies and zero build step. No package manager, no bundler, no framework. Every dependency added creates a build requirement Vercel was not configured for.
- Zero runtime logic. No scripts in V1; if behavior is needed later, that is a deliberate scope change against the spec.
- No font downloads. Times New Roman is a system font — a webfont would add a network request to the critical path and work against the sub-second load target.
- All paths are relative (`assets/...`, `style.css`) so the site works identically at a `*.vercel.app` URL and under a future custom domain without rewrites.

## Target file layout

```
index.html
style.css
assets/benkronfeldresumeoctober.pdf
favicon.ico            (optional)
personal_website_project_spec.md
README.md
```

The spec's tree calls the spec file `project_spec.md`; the actual file in this repo is `personal_website_project_spec.md`. Keep using the actual filename.

## Commands

There is no build, lint, or test tooling — the site is static files served as-is.

Local preview (no install needed):

```powershell
python -m http.server 8000
# then open http://localhost:8000
```

Alternatively `npx serve .`. Vercel will serve `index.html` from the repo root; set the framework preset to "Other", leave the build command empty, and set the output directory to the root.

## Implementation conventions

Everything lives in two files plus an asset directory:

- `index.html` — a single `<main>` containing `<h1>`, the bio `<p>`, a contact `<p>` with the email, and a `<nav>` of four links. Semantic tags only.
- `style.css` — loaded by `index.html`; contains all presentation. No inline styles.

Constraints that matter and are easy to violate accidentally:

- **Visual identity is fixed:** white background (`#fff`), black text (`#000`), `"Times New Roman", Times, serif`, centered text in a centered column (`max-width` ~600px) that is also vertically centered in the viewport via flexbox, base font size ~18px using `rem`, side padding of at least `1rem` on mobile.
- **No animations and no transitions.** Hover treatment is limited to a simple underline or opacity change; links stay black.
- **Links:** nav order is LinkedIn, Instagram, GitHub, Resume. All four use `target="_blank" rel="noopener"`; Resume points at `assets/benkronfeldresumeoctober.pdf`. Links are spaced with flex `gap`, never separator characters. The email is not in the nav: it appears above it as `Reach out: <mailto>` in a `.contact` paragraph.
- **Head metadata is required, not optional:** `<meta name="viewport">`, a `<title>`, a meta description, and Open Graph title + description so LinkedIn shares preview correctly.
- **Resume asset:** the source upload has a prefixed, space-containing filename. Copy it into `assets/` as `benkronfeldresumeoctober.pdf` — never link to a filename with spaces or the timestamp prefix.
- **Success means four working links, a resume PDF that opens in a new tab, no horizontal scroll on mobile, and sub-second load.** Anything that adds weight (frameworks, fonts to download, scripts) works against that.

Content is fixed: the bio is the short copy currently in `index.html`, and the link list is exactly LinkedIn, Instagram, GitHub, Resume with the email in the contact line above. This supersedes the spec's §1.5 link table (Resume, GitHub, LinkedIn, Email) per an explicit user change; do not revert to the spec's list. Side ventures and content accounts beyond the approved Instagram link, a blog, a portfolio, an on-page resume section, analytics, and contact forms remain out of scope for V1. Bio wording may be refined iteratively, but do not add claims not supported by the resume.

## Deployment

Done: git repo initialized, `main` pushed to `https://github.com/benkronfeld/Personal-Website` (public).

Remaining: import the repo into Vercel (framework preset "Other", no build command, root output directory). After the import, every push to `main` auto-deploys. Once Vercel assigns the `*.vercel.app` URL, add `og:url` (and optionally `og:image`) to `index.html`. The custom domain is added later.

Verify against the spec's acceptance checklist (section 2.7) before calling the site done. Live-URL items cannot be verified until the Vercel import happens.

## Branching
- Always create a feature branch before starting major changes
- Never commit directly to 'main'
- Branch naming: 'feature/description' or 'fix/description'