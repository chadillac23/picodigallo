# Publishing to GitHub Pages (picodigallo)

This document describes the process for deploying games from this private repo (`mangosalsa`) to the public GitHub Pages site hosted at:

**https://chadillac23.github.io/picodigallo/**

## Architecture

| Repo | Visibility | Purpose |
|------|-----------|---------|
| `chadillac23/mangosalsa` | Private | Source of truth for game development |
| `chadillac23/picodigallo` | Public | Static hosting via GitHub Pages |

Games are developed and tested locally in `mangosalsa`, then copied to `picodigallo` for public deployment.

## Local Directory Paths

- **Source (private):** `/Users/chadleonard/Downloads/mangosalsa/mangosalsa/`
- **Deploy (public):** `/tmp/picodigallo/`

If the deploy directory doesn't exist, clone it:

```bash
git clone https://github.com/chadillac23/picodigallo.git /tmp/picodigallo
```

## Deploying a New Game

### 1. Copy the game file to picodigallo

```bash
cp /Users/chadleonard/Downloads/mangosalsa/mangosalsa/NEWGAME.html /tmp/picodigallo/
```

### 2. Update the index.html game menu

Edit `/tmp/picodigallo/index.html` and add a new game card inside the `<div class="games">` container:

```html
<a class="game-card" href="NEWGAME.html">
  <div class="game-icon">EMOJI</div>
  <div class="game-title">Game Title</div>
  <div class="game-desc">Short description<br>Second line</div>
</a>
```

### 3. Commit and push

```bash
cd /tmp/picodigallo
git add NEWGAME.html index.html
git commit -m "Add GAME NAME and update menu"
git push origin main
```

### 4. Wait for deployment

GitHub Pages rebuilds automatically on push to `main`. Changes typically appear within 1-2 minutes at:

```
https://chadillac23.github.io/picodigallo/
```

## Updating an Existing Game

```bash
cp /Users/chadleonard/Downloads/mangosalsa/mangosalsa/EXISTINGGAME.html /tmp/picodigallo/
cd /tmp/picodigallo
git add EXISTINGGAME.html
git commit -m "Update GAME NAME - description of changes"
git push origin main
```

## Current Games Deployed

| File | Game | Description |
|------|------|-------------|
| `index.html` | Menu | Landing page with game cards |
| `catrunner.html` | Cat Runner | Side-scrolling runner with cat selection |
| `tictactoe.html` | Tic Tac Toe | 2-player or vs CPU with fireworks |
| `dressupdoll.html` | Dress Up Doll | Drag-and-drop fashion with snap zones |

## Important Notes

- **All games are single-file HTML** with embedded CSS/JS. No build step, no external dependencies.
- **GitHub Pages requires the repo to be public** on the free plan. That's why we use a separate public repo instead of enabling Pages on the private `mangosalsa` repo.
- **The `main` branch** is the deploy branch. GitHub Pages is configured to serve from the root of `main`.
- **Mobile/iOS compatibility** is critical. All games must work in iOS Safari when shared via iMessage link. Key rules:
  - Use `touch-action: manipulation` (never `none` on root elements)
  - Never globally `preventDefault()` on document-level touch events
  - Use `position: fixed` for full-screen overlays
  - Share URLs (not HTML files) — iOS Quick Look blocks JavaScript in file previews

## Troubleshooting

- **404 after push**: Wait 1-2 minutes for Pages to rebuild. Verify the file exists on `main` branch.
- **Changes not appearing**: Hard-refresh the browser (Cmd+Shift+R) to bypass cache.
- **Auth issues with git push**: The CLI uses a GitHub PAT. If push fails, verify the token has access to the `picodigallo` repo via GitHub Settings > Developer Settings > Fine-grained tokens.
