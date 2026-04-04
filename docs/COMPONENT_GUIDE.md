# Component Guide — infinitekb.com

## Overview

All components are written in TypeScript with Tailwind CSS classes only.
No inline styles, no CSS modules, no styled-components.
Components live in `components/` and are organized by function.

---

## Layout Components

### Header.tsx

```tsx
// components/layout/Header.tsx
import Link from 'next/link'

const NAV_LINKS = [
  { label: 'Tutorials', href: '/tutorials' },
  { label: 'Vibe Coding', href: '/vibe-coding' },
  { label: 'AI Governance', href: '/ai-governance' },
  { label: 'News', href: '/news' },
  { label: 'Tools', href: '/tools' },
  { label: 'Glossary', href: '/glossary' },
]

export default function Header() {
  return (
    <header className="border-b border-gray-200 dark:border-gray-800 bg-white dark:bg-gray-950">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div className="flex items-center justify-between h-16">
          <Link href="/" className="text-xl font-semibold text-gray-900 dark:text-white">
            InfiniteKB
          </Link>
          <nav className="hidden md:flex items-center gap-6">
            {NAV_LINKS.map((link) => (
              <Link
                key={link.href}
                href={link.href}
                className="text-sm text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white transition-colors"
              >
                {link.label}
              </Link>
            ))}
          </nav>
        </div>
      </div>
    </header>
  )
}
```

---

### Footer.tsx

```tsx
// components/layout/Footer.tsx
import Link from 'next/link'

export default function Footer() {
  return (
    <footer className="border-t border-gray-200 dark:border-gray-800 bg-white dark:bg-gray-950 mt-24">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <div className="grid grid-cols-2 md:grid-cols-4 gap-8">
          <div>
            <p className="text-sm font-semibold text-gray-900 dark:text-white mb-4">Learn</p>
            <ul className="space-y-2">
              <li><Link href="/tutorials" className="text-sm text-gray-500 hover:text-gray-900 dark:hover:text-white">Tutorials</Link></li>
              <li><Link href="/vibe-coding" className="text-sm text-gray-500 hover:text-gray-900 dark:hover:text-white">Vibe Coding</Link></li>
              <li><Link href="/ai-governance" className="text-sm text-gray-500 hover:text-gray-900 dark:hover:text-white">AI Governance</Link></li>
            </ul>
          </div>
          <div>
            <p className="text-sm font-semibold text-gray-900 dark:text-white mb-4">Explore</p>
            <ul className="space-y-2">
              <li><Link href="/news" className="text-sm text-gray-500 hover:text-gray-900 dark:hover:text-white">News</Link></li>
              <li><Link href="/tools" className="text-sm text-gray-500 hover:text-gray-900 dark:hover:text-white">Tools</Link></li>
              <li><Link href="/glossary" className="text-sm text-gray-500 hover:text-gray-900 dark:hover:text-white">Glossary</Link></li>
            </ul>
          </div>
        </div>
        <div className="mt-8 pt-8 border-t border-gray-200 dark:border-gray-800">
          <p className="text-sm text-gray-500">
            © {new Date().getFullYear()} Infinite Knowledge Base. All rights reserved.
          </p>
        </div>
      </div>
    </footer>
  )
}
```

---

## Article Components

### ArticleCard.tsx

Used in category hub grids. Accepts an `ArticleMeta` object.

```tsx
// components/article/ArticleCard.tsx
import Link from 'next/link'
import type { ArticleMeta } from '@/lib/types'
import Badge from '@/components/ui/Badge'

interface ArticleCardProps {
  article: ArticleMeta
}

export default function ArticleCard({ article }: ArticleCardProps) {
  const href = `/${article.category}/${article.slug}`

  return (
    <Link href={href} className="group block">
      <article className="border border-gray-200 dark:border-gray-800 rounded-xl p-6 hover:border-gray-400 dark:hover:border-gray-600 transition-colors">
        <div className="flex items-center gap-2 mb-3">
          <Badge label={article.category} />
          {article.featured && <Badge label="Featured" variant="featured" />}
        </div>
        <h2 className="text-lg font-semibold text-gray-900 dark:text-white group-hover:text-indigo-600 dark:group-hover:text-indigo-400 transition-colors mb-2">
          {article.title}
        </h2>
        <p className="text-sm text-gray-600 dark:text-gray-400 line-clamp-2 mb-4">
          {article.description}
        </p>
        <div className="flex items-center justify-between text-xs text-gray-400 dark:text-gray-500">
          <span>{article.author}</span>
          <span>{new Date(article.publishedAt).toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' })}</span>
        </div>
      </article>
    </Link>
  )
}
```

---

### ArticleHeader.tsx

Used at the top of individual article pages.

```tsx
// components/article/ArticleHeader.tsx
import type { ArticleMeta } from '@/lib/types'
import Badge from '@/components/ui/Badge'

interface ArticleHeaderProps {
  article: ArticleMeta
}

export default function ArticleHeader({ article }: ArticleHeaderProps) {
  return (
    <header className="mb-10">
      <div className="flex items-center gap-2 mb-4">
        <Badge label={article.category} />
        {article.tags.slice(0, 3).map((tag) => (
          <Badge key={tag} label={tag} variant="tag" />
        ))}
      </div>
      <h1 className="text-4xl font-bold text-gray-900 dark:text-white mb-4">
        {article.title}
      </h1>
      <p className="text-xl text-gray-600 dark:text-gray-400 mb-6">
        {article.description}
      </p>
      <div className="flex items-center gap-4 text-sm text-gray-500 dark:text-gray-400 pb-8 border-b border-gray-200 dark:border-gray-800">
        <span>{article.author}</span>
        <span>·</span>
        <time dateTime={article.publishedAt}>
          {new Date(article.publishedAt).toLocaleDateString('en-US', { month: 'long', day: 'numeric', year: 'numeric' })}
        </time>
        {article.updatedAt !== article.publishedAt && (
          <>
            <span>·</span>
            <span>Updated {new Date(article.updatedAt).toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' })}</span>
          </>
        )}
      </div>
    </header>
  )
}
```

---

### TableOfContents.tsx

Auto-generates from MDX headings passed as props.

```tsx
// components/article/TableOfContents.tsx

interface Heading {
  id: string
  text: string
  level: 2 | 3
}

interface TableOfContentsProps {
  headings: Heading[]
}

export default function TableOfContents({ headings }: TableOfContentsProps) {
  if (headings.length === 0) return null

  return (
    <nav className="border border-gray-200 dark:border-gray-800 rounded-xl p-6 mb-8">
      <p className="text-sm font-semibold text-gray-900 dark:text-white mb-4">
        On this page
      </p>
      <ul className="space-y-2">
        {headings.map((heading) => (
          <li
            key={heading.id}
            className={heading.level === 3 ? 'pl-4' : ''}
          >
            <a
              href={`#${heading.id}`}
              className="text-sm text-gray-600 hover:text-indigo-600 dark:text-gray-400 dark:hover:text-indigo-400 transition-colors"
            >
              {heading.text}
            </a>
          </li>
        ))}
      </ul>
    </nav>
  )
}
```

---

### AuthorBio.tsx

```tsx
// components/article/AuthorBio.tsx

interface AuthorBioProps {
  name: string
  bio?: string
}

export default function AuthorBio({ name, bio }: AuthorBioProps) {
  const initials = name.split(' ').map((n) => n[0]).join('')

  return (
    <div className="flex items-start gap-4 border border-gray-200 dark:border-gray-800 rounded-xl p-6 mt-12">
      <div className="w-12 h-12 rounded-full bg-indigo-100 dark:bg-indigo-900 flex items-center justify-center flex-shrink-0">
        <span className="text-sm font-semibold text-indigo-600 dark:text-indigo-300">
          {initials}
        </span>
      </div>
      <div>
        <p className="text-sm font-semibold text-gray-900 dark:text-white mb-1">{name}</p>
        {bio && <p className="text-sm text-gray-600 dark:text-gray-400">{bio}</p>}
      </div>
    </div>
  )
}
```

---

## Category Components

### CategoryHero.tsx

```tsx
// components/category/CategoryHero.tsx

interface CategoryHeroProps {
  title: string
  description: string
  articleCount: number
}

export default function CategoryHero({ title, description, articleCount }: CategoryHeroProps) {
  return (
    <div className="border-b border-gray-200 dark:border-gray-800 pb-10 mb-10">
      <h1 className="text-4xl font-bold text-gray-900 dark:text-white mb-4">
        {title}
      </h1>
      <p className="text-xl text-gray-600 dark:text-gray-400 max-w-2xl mb-4">
        {description}
      </p>
      <p className="text-sm text-gray-400 dark:text-gray-500">
        {articleCount} {articleCount === 1 ? 'article' : 'articles'}
      </p>
    </div>
  )
}
```

---

### CategoryGrid.tsx

```tsx
// components/category/CategoryGrid.tsx
import ArticleCard from '@/components/article/ArticleCard'
import type { ArticleMeta } from '@/lib/types'

interface CategoryGridProps {
  articles: ArticleMeta[]
}

export default function CategoryGrid({ articles }: CategoryGridProps) {
  if (articles.length === 0) {
    return (
      <p className="text-gray-500 dark:text-gray-400 text-sm">
        No articles yet. Check back soon.
      </p>
    )
  }

  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {articles.map((article) => (
        <ArticleCard key={article.slug} article={article} />
      ))}
    </div>
  )
}
```

---

## UI Components

### Badge.tsx

```tsx
// components/ui/Badge.tsx

interface BadgeProps {
  label: string
  variant?: 'default' | 'featured' | 'tag'
}

const VARIANTS = {
  default: 'bg-indigo-50 text-indigo-700 dark:bg-indigo-900/30 dark:text-indigo-300',
  featured: 'bg-amber-50 text-amber-700 dark:bg-amber-900/30 dark:text-amber-300',
  tag: 'bg-gray-100 text-gray-600 dark:bg-gray-800 dark:text-gray-400',
}

export default function Badge({ label, variant = 'default' }: BadgeProps) {
  return (
    <span className={`inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium ${VARIANTS[variant]}`}>
      {label}
    </span>
  )
}
```

---

### NewsletterSignup.tsx

```tsx
// components/ui/NewsletterSignup.tsx

export default function NewsletterSignup() {
  return (
    <div className="bg-indigo-50 dark:bg-indigo-900/20 border border-indigo-200 dark:border-indigo-800 rounded-xl p-8 my-12">
      <h2 className="text-xl font-semibold text-gray-900 dark:text-white mb-2">
        Stay current with AI
      </h2>
      <p className="text-sm text-gray-600 dark:text-gray-400 mb-6">
        Weekly tutorials, governance updates, and tool reviews. No spam.
      </p>
      <form className="flex gap-3 max-w-md">
        <input
          type="email"
          placeholder="you@company.com"
          className="flex-1 px-4 py-2 text-sm border border-gray-300 dark:border-gray-700 rounded-lg bg-white dark:bg-gray-900 text-gray-900 dark:text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-indigo-500"
        />
        <button
          type="submit"
          className="px-4 py-2 text-sm font-medium bg-indigo-600 hover:bg-indigo-700 text-white rounded-lg transition-colors"
        >
          Subscribe
        </button>
      </form>
    </div>
  )
}
```

---

## Component Rules

1. All components are server components by default — add `'use client'` only when using hooks or browser events
2. Always type props with a named interface above the component
3. Use Tailwind `dark:` variants for every color class
4. Never use `any` type
5. Export as default from each file
6. Keep components under 100 lines — split if longer
