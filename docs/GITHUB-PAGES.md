# Deploy to GitHub Pages

This static site is ready for GitHub Pages. Follow these steps to deploy.

## 1. Set Up Formspree (Contact Form)

The contact form uses [Formspree](https://formspree.io) to receive submissions without a backend.

1. Go to [formspree.io](https://formspree.io) and sign up (free)
2. Create a new form
3. Copy your form ID (looks like `xyzabcde`)
4. Edit `contact.html` and replace `YOUR_FORM_ID` in the form action:
   ```html
   action="https://formspree.io/f/YOUR_FORM_ID"
   ```
   Change to:
   ```html
   action="https://formspree.io/f/xyzabcde"
   ```
   (using your actual form ID)

## 2. Add Your Background Video (Optional)

To add a background video to the hero section:

1. Add your video file(s) to `static/video/`:
   - `hero-bg.mp4` (recommended)
   - `hero-bg.webm` (optional)

2. See `static/video/README.md` for recommended specs.

If you skip this, the hero will use the static image.

## 3. Enable GitHub Pages

1. Push this repo to GitHub (if not already)
2. Go to your repo on GitHub → **Settings** → **Pages**
3. Under **Source**, select **Deploy from a branch**
4. Under **Branch**, select `main` and `/docs`
5. Click **Save**

Your site will be live at:  
`https://YOUR_USERNAME.github.io/ConstructOS/`

## 4. Custom Domain (Namecheap)

To use your Namecheap domain:

1. In GitHub repo **Settings** → **Pages** → **Custom domain**
2. Enter your domain (e.g. `constructos.com`)
3. In Namecheap **Advanced DNS**:
   - Add **CNAME**: Host `www`, Value `YOUR_USERNAME.github.io`
   - For root domain (`@`): Add **ALIAS** or **A** record per GitHub's instructions

4. Enable **Enforce HTTPS** in GitHub Pages settings once DNS propagates

## 5. Keep GitHub Pages in Sync With Local Styling

GitHub Pages serves the site from the **`docs/`** folder. The files in **`docs/static/`** (CSS, JS, images) are what the live site uses. Your Flask app uses the **root `static/`** folder.

**If you change styles or assets in the root `static/` folder, copy them into `docs/static/` before pushing**, or the live site will look different (e.g. missing division badges, hero layout, or other updates). For example:

- Copy `static/css/styles.css` → `docs/static/css/styles.css`
- Or sync the whole folder: copy `static/*` into `docs/static/`

After syncing, commit and push; the site will update within a few minutes.

## Done!

Push changes to the `main` branch and the site updates automatically.
