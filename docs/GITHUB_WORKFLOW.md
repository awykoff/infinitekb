# GitHub Workflow — infinitekb.com

## Branch Strategy

| Branch | Purpose |
|---|---|
| `main` | Production — deploys automatically to Site5 |
| `feature/[name]` | New pages, components, or site features |
| `content/[slug]` | New or updated articles |
| `fix/[name]` | Bug fixes |

**main is protected.** No direct pushes. All changes require a PR with at least 1 approval before merging.

---

## Branch Naming

```
feature/tutorials-hub-page
feature/newsletter-signup
content/how-to-use-claude-code
content/ai-governance-framework-guide
fix/mobile-nav-overflow
fix/sitemap-missing-glossary
```

---

## Commit Message Format

Uses Conventional Commits standard.

```
type: short description (max 72 chars)

Optional longer body explaining what and why.
```

### Types

| Type | Use for |
|---|---|
| `feat` | New page, component, or feature |
| `content` | New or updated article |
| `fix` | Bug fix |
| `docs` | Changes to /docs knowledge files |
| `style` | Tailwind/CSS changes only |
| `refactor` | Code restructure, no behavior change |
| `chore` | Config, dependencies, tooling |

### Examples

```
feat: add tutorials category hub page
content: add how-to-use-claude-code article
fix: correct trailing slash on sitemap entries
docs: update CONTENT_CATEGORIES with tools phase 2
chore: upgrade Next.js to 15.1.0
style: adjust article card hover state
```

---

## Pull Request Template

Create this file at `.github/pull_request_template.md`:

```markdown
## Summary
<!-- What does this PR do? One or two sentences. -->

## Type
- [ ] Feature (new page or component)
- [ ] Content (new or updated article)
- [ ] Fix
- [ ] Docs
- [ ] Chore

## Checklist
- [ ] All new pages have `generateMetadata()` with title and description
- [ ] All new articles have complete MDX frontmatter
- [ ] JSON-LD schema added for article pages
- [ ] Tailwind dark mode classes included
- [ ] No TypeScript errors (`npm run build` passes locally)
- [ ] Tested in browser at localhost:3000

## Preview
<!-- Paste localhost URL or describe what to look at -->
```

---

## Branch Protection Settings

Configure at: GitHub → Settings → Branches → Add branch ruleset

**Target branch:** `main`

**Rules to enable:**
- Require a pull request before merging
- Require approvals: **1**
- Dismiss stale pull request approvals when new commits are pushed
- Require status checks to pass before merging (add `build` check once Actions is configured)
- Do not allow bypassing the above settings

---

## GitHub Actions — deploy.yml

Create at `.github/workflows/deploy.yml`:

```yaml
name: Build and Deploy

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to Site5 via FTP
        uses: SamKirkland/FTP-Deploy-Action@v4.3.5
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          local-dir: ./out/
          server-dir: /public_html/
          exclude: |
            **/.git*
            **/.git*/**
            **/node_modules/**
            **/docs/**
```

---

## GitHub Secrets Setup

Go to: GitHub → Settings → Secrets and variables → Actions → New repository secret

| Secret name | Value |
|---|---|
| `FTP_SERVER` | Site5 FTP hostname (e.g. `ftp.infinitekb.com`) |
| `FTP_USERNAME` | Site5 FTP username |
| `FTP_PASSWORD` | Site5 FTP password |

Find these in your Site5 cPanel under **FTP Accounts**.

---

## Day-to-Day Workflow

### Writing a new article

```bash
# 1. Create a content branch
git checkout -b content/how-to-use-mcp

# 2. Create the MDX file
touch content/tutorials/how-to-use-mcp.mdx

# 3. Write the article, commit as you go
git add .
git commit -m "content: add how-to-use-mcp article draft"

# 4. Push and open PR
git push origin content/how-to-use-mcp
```

Then open a PR on GitHub, fill out the PR template, and request review.

---

### Building a new feature

```bash
# 1. Create a feature branch
git checkout -b feature/search-bar

# 2. Build, commit with conventional commits
git add .
git commit -m "feat: add client-side search bar component"

# 3. Push and open PR
git push origin feature/search-bar
```

---

### After PR is approved and merged

GitHub Actions triggers automatically on merge to `main`:
1. Checks out the repo
2. Installs dependencies
3. Runs `npm run build` → outputs to `/out`
4. FTP uploads `/out` contents to Site5 `/public_html`

**Deploy time:** approximately 2–3 minutes after merge.

---

## Checking Deploy Status

Go to: GitHub → Actions tab → select the latest workflow run

- Green checkmark = deployed successfully
- Red X = build or deploy failed — click to see logs
