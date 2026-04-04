# Content Categories — infinitekb.com

## Category Overview

| Category | URL | Content Type | Schema |
|---|---|---|---|
| Tutorials | `/tutorials` | Step-by-step how-to guides | `HowTo` |
| AI Governance | `/ai-governance` | Policy, ethics, compliance | `Article` |
| Vibe Coding | `/vibe-coding` | Prompt engineering, AI-assisted dev | `HowTo` / `Article` |
| News | `/news` | AI news roundups, product launches | `NewsArticle` |
| Tools | `/tools` | AI tool reviews and comparisons | `Article` |
| Glossary | `/glossary` | Term definitions | `DefinedTerm` |

---

## Category Definitions

### Tutorials `/tutorials`
Long-form how-to guides with numbered steps. Targets readers searching for
"how to" accomplish something specific with AI tools. Articles should be
actionable and reproducible — readers should be able to follow along and
get a result.

**Good titles:**
- "How to Set Up Claude Code on macOS"
- "How to Build a Cold Email Workflow with AI"
- "How to Use Claude Projects for Team Knowledge Management"

**Avoid:** Opinion pieces, news, abstract concepts without steps.

---

### AI Governance `/ai-governance`
Best practices for responsible AI use in organizations. Targets business
leaders, IT managers, and compliance teams searching for frameworks,
policies, and guidelines.

**Good titles:**
- "AI Acceptable Use Policy Template for SMBs"
- "How to Audit Your AI Tools for Data Privacy Compliance"
- "CMMC 2.0 and AI: What MSPs Need to Know"

**Avoid:** Purely technical tutorials, news without governance angle.

---

### Vibe Coding `/vibe-coding`
AI-assisted development workflows, prompt engineering, and best practices
for coding with AI tools. Targets developers using Claude Code, Cursor,
GitHub Copilot, and similar tools.

**Good titles:**
- "Vibe Coding Best Practices: How to Get Consistent Results"
- "Prompt Engineering Patterns for Next.js Development"
- "How to Use Claude Code Skills for Repeatable Workflows"

**Avoid:** General programming tutorials not involving AI assistance.

---

### News `/news`
Timely coverage of AI product launches, research papers, and industry
developments. Targets readers who want to stay current without reading
raw press releases or research papers.

**Good titles:**
- "Anthropic Releases Claude 4: What's New for Developers"
- "OpenAI vs Anthropic: Enterprise AI in 2025"
- "Week in AI: Top Stories for April 2025"

**Avoid:** Evergreen how-to content, opinion without news hook.

---

### Tools `/tools`
Reviews, comparisons, and guides for AI tools and platforms. Targets
readers at the decision stage — evaluating tools before committing.

**Good titles:**
- "Claude Code vs Cursor: Which AI Coding Tool is Right for You?"
- "Best AI Tools for MSPs in 2025"
- "Claude CoWork Review: Enterprise AI for Sales Teams"

**Avoid:** News, tutorials not tied to a specific tool evaluation.

---

### Glossary `/glossary`
Single-page definitions for AI terms and concepts. Targets readers
searching "what is [term]" queries. Each page targets one term with a
thorough definition, related terms, and practical examples.

**Good titles:**
- "What is Retrieval-Augmented Generation (RAG)?"
- "What is Model Context Protocol (MCP)?"
- "What is Vibe Coding?"

**Avoid:** Multi-term pages, opinion, news.

---

## Tag System

Tags are lowercase kebab-case. Use 2–5 tags per article.

### Tool Tags
`claude` · `claude-code` · `claude-cowork` · `cursor` · `chatgpt` ·
`github-copilot` · `anthropic` · `openai` · `n8n` · `zapier`

### Topic Tags
`prompt-engineering` · `vibe-coding` · `ai-governance` · `cybersecurity` ·
`cold-email` · `lead-generation` · `seo` · `automation` · `mcp` · `rag`

### Audience Tags
`beginners` · `developers` · `msp` · `enterprise` · `small-business`

### Format Tags
`tutorial` · `comparison` · `review` · `news` · `checklist` · `template`

---

## URL Conventions

| Pattern | Example |
|---|---|
| Category hub | `/tutorials` |
| Article | `/tutorials/how-to-use-claude-code` |
| Glossary term | `/glossary/retrieval-augmented-generation` |
| Tool comparison | `/tools/claude-code-vs-cursor` |
| News item | `/news/anthropic-claude-4-release` |

**Rules:**
- Always kebab-case
- No dates in URLs (content stays evergreen)
- No stop words (a, the, for, of) unless essential for meaning
- Max 5 words in slug where possible

---

## MDX Frontmatter Schema

Every article must include all required fields.

```mdx
---
title: "How to Use Claude Code for Vibe Coding"          # Required — full article title
description: "A step-by-step guide to setting up..."     # Required — 140-160 chars
category: "tutorials"                                     # Required — see Category Overview
slug: "how-to-use-claude-code"                           # Required — kebab-case, matches URL
author: "Aaron Wykoff"                                    # Required
publishedAt: "2025-04-03"                                # Required — YYYY-MM-DD
updatedAt: "2025-04-03"                                  # Required — YYYY-MM-DD
tags: ["claude", "vibe-coding", "claude-code"]           # Required — 2-5 tags
featured: false                                          # Required — true for homepage feature
schema: "HowTo"                                          # Required — see Category Overview
---
```

---

## Content Priorities

Build these first for maximum SEO impact:

### Phase 1 — Foundation (launch)
- [ ] Homepage
- [ ] `/tutorials` hub
- [ ] `/vibe-coding` hub
- [ ] `/ai-governance` hub
- [ ] 3 tutorial articles (Claude Code, Claude Projects, MCP setup)
- [ ] 5 glossary terms (RAG, MCP, vibe coding, prompt engineering, AI governance)

### Phase 2 — Growth
- [ ] `/news` hub + first 3 news articles
- [ ] `/tools` hub + Claude Code vs Cursor comparison
- [ ] 5 more tutorials targeting high-volume how-to queries
- [ ] 10 more glossary terms

### Phase 3 — Authority
- [ ] `/ai-governance` policy templates (downloadable)
- [ ] Weekly news roundup cadence
- [ ] Guest author system
- [ ] Newsletter integration
