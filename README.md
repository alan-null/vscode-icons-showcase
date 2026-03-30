# vscode-icons showcase

A GitHub Pages site that renders an icon preview gallery of all SVGs from
[vscode-icons/vscode-icons](https://github.com/vscode-icons/vscode-icons),
auto-updated whenever upstream publishes a new release.

## Live site

👉 `https://<your-username>.github.io/vscode-icons-showcase/`

## How it works

| Workflow | Trigger | What it does |
|---|---|---|
| `check-release.yml` | Daily at 08:00 UTC (+ manual) | Hits the GitHub API, compares the latest upstream tag against `.last-release`, calls `deploy.yml` if changed |
| `deploy.yml` | Called by check, or manually | Sparse-checkouts `icons/` from vscode-icons at the release tag, runs `New-IconPreview.ps1`, commits updated `.last-release`, deploys to GitHub Pages |

## Manual deploy

Go to **Actions → Build & deploy showcase → Run workflow**.  
Leave the tag field empty to always pull the current latest release.

## Local preview

```powershell
.\scripts\New-IconPreview.ps1 -IconsPath "path\to\vscode-icons\icons" -Recurse
# opens icon-preview.html in your browser
Invoke-Item .\icon-preview.html
```

## Repo setup checklist

1. **Enable GitHub Pages** — Settings → Pages → Source: **GitHub Actions**
2. **No extra secrets needed** — the built-in `GITHUB_TOKEN` is sufficient

## Custom icons

Drop your own `.svg` files into `custom-icons/` and extend the deploy step
in `deploy.yml` to copy them into `upstream/icons/` before running the script.
