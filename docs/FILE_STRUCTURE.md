# File Structure — infinitekb.com

## Root Directory

```
infinitekb.com/
├── app/                        # Next.js App Router — all routes live here
├── components/                 # Shared React components
├── content/                    # MDX article files
├── lib/                        # Utility functions and helpers
├── public/                     # Static assets (images, fonts, icons)
├── docs/                       # Claude Project knowledge files (not deployed)
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions build + FTP deploy
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── package.json
└── README.md
```

---

## app/ — Route Structure

```
app/
├── layout.tsx                  # Root layout (nav, footer, global styles)
├── page.tsx                    # Homepage /
├── globals.css                 # Tailwind base imports
│
├── tutorials/
│   ├── page.tsx                # /tutorials — category hub
│   └── [slug]/
│       └── page.tsx            # /tutorials/[slug] — individual article
│
├── ai-governance/
│   ├── page.tsx                # /ai-governance — category hub
│   └── [slug]/
│       └── page.tsx            # /ai-governance/[slug]
│
├── vibe-coding/
│   ├── page.tsx                # /vibe-coding — category hub
│   └── [slug]/
│       └── page.tsx            # /vibe-coding/[slug]
│
├── news/
│   ├── page.tsx                # /news — category hub
│   └── [slug]/
│       └── page.tsx            # /news/[slug]
│
├── tools/
│   ├── page.tsx                # /tools — AI tool reviews hub
│   └── [slug]/
│       └── page.tsx            # /tools/[slug]
│
├── glossary/
│   ├── page.tsx                # /glossary — full term index
│   └── [term]/
│       └── page.tsx            # /glossary/[term]
│
└── sitemap.ts                  # Auto-generated sitemap.xml
```

---

## content/ — MDX Articles

```
content/
├── tutorials/
│   └── how-to-use-claude-code.mdx
├── ai-governance/
│   └── ai-policy-frameworks-2025.mdx
├── vibe-coding/
│   └── prompt-engineering-best-practices.mdx
├── news/
│   └── anthropic-releases-claude-4.mdx
├── tools/
│   └── claude-vs-cursor-comparison.mdx
└── glossary/
    └── retrieval-augmented-generation.mdx
```

### MDX Frontmatter Schema

Every `.mdx` file must include this frontmatter:

```mdx
---
title: "How to Use Claude Code for Vibe Coding"
description: "A step-by-step guide to setting up Claude Code and using it for AI-assisted development."
category: "tutorials"
slug: "how-to-use-claude-code"
author: "Aaron Wykoff"
publishedAt: "2025-04-03"
updatedAt: "2025-04-03"
tags: ["claude", "vibe-coding", "claude-code"]
featured: false
schema: "HowTo"
---
```

**schema** options: `HowTo` · `Article` · `NewsArticle` · `FAQPage` · `DefinedTerm`

---

## components/ — Shared Components

```
components/
├── layout/
│   ├── Header.tsx              # Site navigation
│   ├── Footer.tsx              # Site footer
│   └── MobileMenu.tsx          # Mobile nav drawer
│
├── article/
│   ├── ArticleCard.tsx         # Card used in category hub grids
│   ├── ArticleHeader.tsx       # Title, author, date, tags
│   ├── TableOfContents.tsx     # Auto-generated from MDX headings
│   └── AuthorBio.tsx           # Author block at article bottom
│
├── category/
│   ├── CategoryHero.tsx        # Hub page header with description
│   └── CategoryGrid.tsx        # Article card grid layout
│
└── ui/
    ├── Badge.tsx               # Tag/category pill
    ├── NewsletterSignup.tsx     # Email capture block
    └── SearchBar.tsx           # Client-side article search
```

---

## lib/ — Utilities

```
lib/
├── mdx.ts                      # Read and parse MDX files from content/
├── metadata.ts                 # generateMetadata() helper
├── schema.ts                   # JSON-LD schema generators
└── types.ts                    # Shared TypeScript types
```

### Key Types (lib/types.ts)

```ts
export type Category =
  | 'tutorials'
  | 'ai-governance'
  | 'vibe-coding'
  | 'news'
  | 'tools'
  | 'glossary'

export type SchemaType =
  | 'HowTo'
  | 'Article'
  | 'NewsArticle'
  | 'FAQPage'
  | 'DefinedTerm'

export interface ArticleMeta {
  title: string
  description: string
  category: Category
  slug: string
  author: string
  publishedAt: string
  updatedAt: string
  tags: string[]
  featured: boolean
  schema: SchemaType
}
```

---

## Naming Conventions

| Thing | Convention | Example |
|---|---|---|
| Route folders | kebab-case | `ai-governance/` |
| MDX files | kebab-case | `prompt-engineering-tips.mdx` |
| Components | PascalCase | `ArticleCard.tsx` |
| Utility files | camelCase | `mdx.ts` |
| URL slugs | kebab-case | `/tutorials/how-to-use-claude-code` |
| Commit messages | conventional commits | `feat: add glossary hub page` |
