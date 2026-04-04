# SEO Standards — infinitekb.com

## Core Principles

- Every page must have a unique `title`, `description`, and canonical URL
- All article pages include JSON-LD structured data matching their `schema` frontmatter field
- Open Graph and Twitter Card tags on every page
- Sitemap auto-generated via `app/sitemap.ts`
- `trailingSlash: true` in next.config.ts ensures clean URLs for Site5

---

## generateMetadata() — Base Pattern

Use this in every `page.tsx`. Import and call the helper from `lib/metadata.ts`.

```ts
// lib/metadata.ts
import type { Metadata } from 'next'

const BASE_URL = 'https://infinitekb.com'
const SITE_NAME = 'Infinite Knowledge Base'
const DEFAULT_DESCRIPTION = 'AI tutorials, how-tos, and best practices for managing AI, vibe coding, and AI governance.'

export function generatePageMetadata({
  title,
  description = DEFAULT_DESCRIPTION,
  path = '',
  image = '/og-default.png',
}: {
  title: string
  description?: string
  path?: string
  image?: string
}): Metadata {
  const url = `${BASE_URL}${path}`

  return {
    title: `${title} | ${SITE_NAME}`,
    description,
    metadataBase: new URL(BASE_URL),
    alternates: {
      canonical: url,
    },
    openGraph: {
      title,
      description,
      url,
      siteName: SITE_NAME,
      images: [{ url: image, width: 1200, height: 630, alt: title }],
      type: 'website',
    },
    twitter: {
      card: 'summary_large_image',
      title,
      description,
      images: [image],
    },
  }
}
```

### Usage in a category hub page

```ts
// app/tutorials/page.tsx
import { generatePageMetadata } from '@/lib/metadata'

export const metadata = generatePageMetadata({
  title: 'AI Tutorials',
  description: 'Step-by-step guides for Claude, vibe coding, prompt engineering, and AI best practices.',
  path: '/tutorials',
})
```

### Usage in a dynamic article page

```ts
// app/tutorials/[slug]/page.tsx
import { generatePageMetadata } from '@/lib/metadata'
import { getArticleBySlug } from '@/lib/mdx'

export async function generateMetadata({ params }: { params: { slug: string } }) {
  const article = await getArticleBySlug('tutorials', params.slug)
  return generatePageMetadata({
    title: article.title,
    description: article.description,
    path: `/tutorials/${article.slug}`,
  })
}
```

---

## JSON-LD Schema Generators

Add to `lib/schema.ts`. Include the output in a `<script>` tag in the page component.

### HowTo Schema

Use for tutorial articles with numbered steps.

```ts
export function howToSchema({
  title,
  description,
  url,
  steps,
}: {
  title: string
  description: string
  url: string
  steps: { name: string; text: string }[]
}) {
  return {
    '@context': 'https://schema.org',
    '@type': 'HowTo',
    name: title,
    description,
    url,
    step: steps.map((step, i) => ({
      '@type': 'HowToStep',
      position: i + 1,
      name: step.name,
      text: step.text,
    })),
  }
}
```

### Article Schema

Use for news and general articles.

```ts
export function articleSchema({
  title,
  description,
  url,
  author,
  publishedAt,
  updatedAt,
  type = 'Article',
}: {
  title: string
  description: string
  url: string
  author: string
  publishedAt: string
  updatedAt: string
  type?: 'Article' | 'NewsArticle'
}) {
  return {
    '@context': 'https://schema.org',
    '@type': type,
    headline: title,
    description,
    url,
    author: {
      '@type': 'Person',
      name: author,
    },
    publisher: {
      '@type': 'Organization',
      name: 'Infinite Knowledge Base',
      url: 'https://infinitekb.com',
    },
    datePublished: publishedAt,
    dateModified: updatedAt,
  }
}
```

### FAQPage Schema

Use for articles with a Q&A section.

```ts
export function faqSchema(faqs: { question: string; answer: string }[]) {
  return {
    '@context': 'https://schema.org',
    '@type': 'FAQPage',
    mainEntity: faqs.map((faq) => ({
      '@type': 'Question',
      name: faq.question,
      acceptedAnswer: {
        '@type': 'Answer',
        text: faq.answer,
      },
    })),
  }
}
```

### DefinedTerm Schema

Use for glossary pages.

```ts
export function definedTermSchema({
  term,
  description,
  url,
}: {
  term: string
  description: string
  url: string
}) {
  return {
    '@context': 'https://schema.org',
    '@type': 'DefinedTerm',
    name: term,
    description,
    url,
    inDefinedTermSet: {
      '@type': 'DefinedTermSet',
      name: 'Infinite Knowledge Base Glossary',
      url: 'https://infinitekb.com/glossary',
    },
  }
}
```

### Injecting Schema into a Page

```tsx
// In any page.tsx or layout.tsx
import { articleSchema } from '@/lib/schema'

export default function ArticlePage({ article }) {
  const schema = articleSchema({
    title: article.title,
    description: article.description,
    url: `https://infinitekb.com/tutorials/${article.slug}`,
    author: article.author,
    publishedAt: article.publishedAt,
    updatedAt: article.updatedAt,
  })

  return (
    <>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(schema) }}
      />
      {/* page content */}
    </>
  )
}
```

---

## Sitemap — app/sitemap.ts

```ts
import { MetadataRoute } from 'next'
import { getAllArticles } from '@/lib/mdx'

const BASE_URL = 'https://infinitekb.com'

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const articles = await getAllArticles()

  const articleUrls = articles.map((article) => ({
    url: `${BASE_URL}/${article.category}/${article.slug}`,
    lastModified: new Date(article.updatedAt),
    changeFrequency: 'weekly' as const,
    priority: 0.8,
  }))

  const staticPages = [
    { url: BASE_URL, priority: 1.0 },
    { url: `${BASE_URL}/tutorials`, priority: 0.9 },
    { url: `${BASE_URL}/ai-governance`, priority: 0.9 },
    { url: `${BASE_URL}/vibe-coding`, priority: 0.9 },
    { url: `${BASE_URL}/news`, priority: 0.9 },
    { url: `${BASE_URL}/tools`, priority: 0.8 },
    { url: `${BASE_URL}/glossary`, priority: 0.7 },
  ].map((page) => ({
    ...page,
    lastModified: new Date(),
    changeFrequency: 'weekly' as const,
  }))

  return [...staticPages, ...articleUrls]
}
```

---

## Title Tag Formulas

| Page type | Formula | Example |
|---|---|---|
| Homepage | `Site name \| Tagline` | `Infinite Knowledge Base \| AI Tutorials & News` |
| Category hub | `Category \| Site name` | `AI Tutorials \| Infinite Knowledge Base` |
| Article | `Article title \| Site name` | `How to Use Claude Code \| Infinite Knowledge Base` |
| Glossary term | `Term definition \| Glossary \| Site name` | `What is RAG \| Glossary \| Infinite Knowledge Base` |

---

## Description Guidelines

- 140–160 characters
- Include the primary keyword naturally in the first sentence
- End with a value statement or call to action
- Never duplicate descriptions across pages

---

## Priority Keywords by Category

| Category | Primary keywords |
|---|---|
| tutorials | how to use Claude, Claude Code tutorial, AI workflow guide |
| ai-governance | AI governance framework, AI policy best practices, responsible AI |
| vibe-coding | vibe coding, prompt engineering, AI-assisted development |
| news | AI news, Anthropic news, Claude updates |
| tools | Claude vs Cursor, best AI coding tools, AI tool reviews |
| glossary | what is RAG, what is MCP, AI glossary |
