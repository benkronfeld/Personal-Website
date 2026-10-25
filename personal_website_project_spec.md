# Project Spec: Personal Website

## Part 1: Product Requirements

### 1.1 Goal
A publicly hosted personal website that gives anyone a short bio and a handful of links (resume, GitHub, LinkedIn, email). It is the page I point people to from social media, LinkedIn, and my resume.

### 1.2 Primary visitor
Anyone who arrives from social media, LinkedIn, or my resume. They want to know who I am in a few seconds and reach the right link. No account, no navigation, no learning curve.

### 1.3 V1 scope

**In scope**
- Single page with a short bio that leads with the builder side
- Links: Resume (PDF), GitHub, LinkedIn, Email
- Clean, minimal UI
- Public hosting on a Vercel URL
- Auto-deploy from GitHub

**Out of scope (V1)**
- Animations or transitions
- Multi-page layout, blog, project portfolio
- On-page resume section
- Analytics, contact form, CMS
- Custom domain (added after V1 is live)
- Side ventures and content accounts as links

### 1.4 Design requirements
- White background, black text
- Times New Roman, falling back to a similar serif
- Centered, aligned text
- Single page, no animations
- Reference sites: darioamodei.com, jessezwas.com, ericzhu.com
- Must read cleanly on mobile (most social/LinkedIn traffic is mobile)

### 1.5 Content

**Name:** Ben Kronfeld

**Bio (draft, builder-first, 3 sentences):**
> I build things with AI, from an email agent that keeps my coursework deadlines in check to an e-commerce store I co-founded. I'm a business analytics and marketing student at Lehigh University and Alumni Relations Chair of Lehigh's AI Club. I'm interested in AI, health tech, and startups.

All claims in the draft come from the resume. Refine wording iteratively before build.

**Links:**

| Label | Target |
|---|---|
| Resume | `assets/benkronfeldresumeoctober.pdf` (uploaded PDF, opens in new tab) |
| GitHub | https://github.com/benkronfeld |
| LinkedIn | https://www.linkedin.com/in/benkronfeld |
| Email | `mailto:ben.e.kronfeld@gmail.com` |

### 1.6 Decisions (resolved)
1. **Bio focus:** lead with the builder side.
2. **Link list:** Resume, GitHub, LinkedIn, Email only.
3. **Resume format:** PDF link, not an on-page section.

### 1.7 Success criteria
- Live at a public URL
- Loads in under 1 second on a normal connection
- All four links work
- Resume PDF opens in a new tab
- Renders correctly on mobile and desktop
- Pushing to `main` redeploys automatically

---

## Part 2: Technical Design

### 2.1 Stack
- Plain HTML, CSS, and minimal JS (V1 likely has none)
- No framework, no build step, no dependencies
- Hosting: Vercel, deployed from a GitHub repository
- Custom domain: deferred

### 2.2 Repository structure
```
personal-site/
├── index.html
├── style.css
├── assets/
│   └── benkronfeldresumeoctober.pdf
├── favicon.ico        (optional)
├── project_spec.md
└── README.md
```
The uploaded resume currently has a prefixed filename ("1792902638909_Ben Kronfeld Resume October.pdf"). Copy it into `assets/` as `benkronfeldresumeoctober.pdf` so the URL is clean and has no spaces.

### 2.3 Page structure (`index.html`)
```
<main>
  <h1>Ben Kronfeld</h1>
  <p>Bio (3 sentences)</p>
  <nav>
    Resume · GitHub · LinkedIn · Email
  </nav>
</main>
```
- Semantic HTML (`main`, `nav`, `h1`)
- Resume link: `href="assets/benkronfeldresumeoctober.pdf"` with `target="_blank" rel="noopener"`
- GitHub and LinkedIn open in a new tab with `rel="noopener"`
- Email: `mailto:ben.e.kronfeld@gmail.com`
- Include `<meta name="viewport">`, a `<title>`, a meta description, and Open Graph title and description so LinkedIn and social shares preview correctly

### 2.4 Styling (`style.css`)
- `font-family: "Times New Roman", Times, serif`
- `background: #fff; color: #000`
- `body`: `text-align: center`, content in a centered column (`max-width` about 600px, `margin: 0 auto`), generous vertical padding
- Links: black, underlined, no color changes beyond a simple hover (underline or opacity)
- Relative units (`rem`), base font size about 18px
- No animations, no transitions
- Mobile: single column, side padding of at least 1rem

### 2.5 Deployment
1. `git init`, commit, create GitHub repo, push to `main`
2. Import the repo into Vercel
   - Framework preset: Other
   - Build command: none
   - Output directory: root
3. Vercel assigns a `*.vercel.app` URL; every push to `main` auto-deploys
4. Later: add custom domain in Vercel and point DNS

### 2.6 Build plan for Claude Code
1. Scaffold the repo and files in 2.2, copying in the resume PDF
2. Implement `index.html` and `style.css` per 2.3 and 2.4
3. Preview locally and check desktop and mobile widths
4. Push to GitHub, connect Vercel, confirm the live URL
5. Verify every link and the resume download on the live site

### 2.7 Acceptance checklist
- [ ] White background, black text, Times New Roman or serif fallback
- [ ] Centered single column, no animations
- [ ] Bio and four links present and working
- [ ] Resume PDF loads from the live site
- [ ] Social share preview shows correct title and description
- [ ] Mobile layout has no horizontal scroll
- [ ] Auto-deploys on push to `main`
