# ConstructOS Website

A static website for ConstructOS — consulting and technology for construction projects (including XER2XLSXGANTT, coming soon).

## Project Structure

```
ConstructOS/
├── docs/                 # GitHub Pages site (live at constructosx.com)
│   ├── index.html       # Home
│   ├── about.html       # About
│   ├── services.html    # Services
│   ├── contact.html     # Contact
│   └── static/         # CSS, JS, images, video
├── static/              # Source assets (keep in sync with docs/static when editing)
│   ├── css/styles.css
│   ├── js/
│   └── images/
├── CNAME                # Custom domain for GitHub Pages
└── docs/GITHUB-PAGES.md # Deploy instructions
```

## Running Locally

Open the site by opening `docs/index.html` in a browser, or use a simple local server:

```bash
# Python
python -m http.server 8000 --directory docs

# Then open http://localhost:8000
```

## Deployment (GitHub Pages)

1. Push the repo to GitHub.
2. In the repo: **Settings → Pages** → Source: **Deploy from a branch** → Branch: **main** → Folder: **/docs**.
3. The site is served from the `docs/` folder. Use a custom domain via **Settings → Pages → Custom domain** (e.g. constructosx.com).

See **docs/GITHUB-PAGES.md** for Formspree (contact form), video, and custom domain setup.

## Keeping the Live Site in Sync

When you edit **root `static/`** (e.g. `static/css/styles.css`), copy the updated files into **`docs/static/`** before pushing, or the live site will not reflect your changes.

Example:
- Copy `static/css/styles.css` → `docs/static/css/styles.css`

## Features

- Responsive layout
- Consulting and Technology divisions
- Contact form (Formspree)
- Custom domain support via CNAME

## Browser Support

Chrome, Firefox, Safari, Edge (latest).
