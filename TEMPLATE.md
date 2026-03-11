# Vite + Bun → GitHub Pages Template

A minimal template for deploying a Vite/TypeScript app to GitHub Pages via GitHub Actions.

## Stack

- **Bun** — package manager and script runner
- **Vite** — build tool
- **TypeScript** — type checking
- **GitHub Actions** — CI/CD
- **GitHub Pages** — hosting

## Repo Structure

```
.
├── .github/
│   └── workflows/
│       └── deploy.yml   # CI/CD pipeline
├── src/
│   └── main.ts          # Entry point
├── index.html           # App shell (Vite entry)
├── .nojekyll            # Disables Jekyll processing on GitHub Pages
├── package.json
├── tsconfig.json
└── bun.lock
```

## How Deployment Works

On every push to `main`, the workflow:

1. Installs dependencies with `bun install --frozen-lockfile`
2. Runs `bun run typecheck` (tsc --noEmit)
3. Runs `bun run build` (Vite outputs to `dist/`)
4. Copies `index.html`, `dist/`, and `.nojekyll` into a `deploy/` directory
5. Uploads `deploy/` as a GitHub Pages artifact and deploys it

The `index.html` at the root is the Vite entry point (used for dev), and is also copied directly into the deploy directory so it sits alongside `dist/` at the root of the hosted site.

## First-Time Setup for a New Repo

1. **Enable GitHub Pages** in the repo:
   - Settings → Pages → Source → **GitHub Actions**

2. **Clone and install:**
   ```sh
   git clone <repo>
   cd <repo>
   bun install
   ```

3. Push to `main` — the action runs automatically.

## Local Development

```sh
bun run dev        # start dev server
bun run build      # production build to dist/
bun run typecheck  # type check only
```

## Key Files to Note

- **`.nojekyll`** — must be present at the root of the deployed directory, otherwise GitHub Pages' Jekyll processor will ignore files and folders starting with `_` (like Vite's `_assets`).
- **`deploy.yml` permissions** — requires `pages: write` and `id-token: write` for the deploy actions to work.
- **`bun.lock`** — commit this so `bun install --frozen-lockfile` works in CI.
