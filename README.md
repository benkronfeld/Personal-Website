# Personal Website

The one-page personal site for Ben Kronfeld: a short bio and links to Resume, GitHub, LinkedIn, and Email.

Plain HTML and CSS. No framework, no build step, no dependencies, no JavaScript.

## Files

```
index.html
style.css
assets/benkronfeldresumeoctober.pdf   (resume, served as a download/open-in-new-tab)
personal_website_project_spec.md      (product + technical spec)
```

## Local preview

There is nothing to install or build. Serve the directory and open the URL:

```powershell
python -m http.server 8765
# http://localhost:8765
```

## Deploy

Hosted on Vercel, deployed from this GitHub repository. Import the repo in the Vercel dashboard with:

- Framework Preset: **Other**
- Build Command: empty
- Output Directory: root
- Install Command: empty

Every push to `main` then deploys automatically.

## Pending

- `assets/benkronfeldresumeoctober.pdf` still needs to be added. Until then the Resume link returns 404.
- Add `og:url` (and optionally `og:image`) to `index.html` once Vercel assigns the live URL, then re-scrape the link with LinkedIn Post Inspector.
