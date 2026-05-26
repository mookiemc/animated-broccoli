# Wayfinder Law Group — Website

Stripped-down static rebuild of wayfinderlawgroup.com. Plain HTML + CSS, no build step. Hosted on GitHub Pages.

## Files

- `index.html` — the single-page site
- `styles.css` — all styling (design tokens at the top)
- `CNAME` — tells GitHub Pages to serve at `wayfinderlawgroup.com`
- `.nojekyll` — skips Jekyll processing so files publish as-is
- `assets/` — drop your logo and photos here

## Editing content

Open `index.html` in any text editor. Each section is clearly commented (`<!-- HERO -->`, `<!-- BUSINESS INTRO -->`, etc.). Edit the text between tags. Save. Commit. Push. GitHub Pages republishes in ~1 minute.

## Adding real images

The current build uses CSS gradient placeholders where images go. To swap in real images:

1. Save the image into `assets/` (e.g., `assets/logo.png`, `assets/shaughan.jpg`, `assets/michael.jpg`, `assets/hero.jpg`).
2. In `index.html`, find the matching `<div class="image-placeholder">...</div>` block and replace it with `<img src="assets/your-file.jpg" alt="descriptive text" />`.
3. For the attorney circles, also remove the `image-placeholder-round` class and add a small CSS rule so the photo is round (or keep the existing div as a frame around the `<img>`).

## Local preview

Just double-click `index.html`. No server required.
