# Daily Delta

**A daily increment of code, books, and ideas.**

Live site: **https://george-102.github.io/daily-delta/**

---

## What this is

I'm Christopher Chang — a computer science student, a programmer, and a writer. I'm also drawn to philosophy, economics, and building things as an entrepreneur. Those interests look scattered, but they share one thread: I want to understand how systems work, whether the system is a compiler, a market, or an argument.

Daily Delta is where I record what I learn each day. The name comes from a simple habit: a small, deliberate increment every day compounds into something large. Most entries are about technology and books, because those are the two places where I learn fastest. Some posts wander into philosophy or economics when a reading pushes me there.

I write in public for three reasons: writing forces me to make my understanding explicit, publishing keeps me honest about the gaps, and the archive lets me look back and measure how far the increments have added up. Writing in English is part of the exercise too.

## What you'll find here

- Notes on programming, computer science, and the tools I use
- Summaries and reflections on the books I'm reading
- Occasional essays on ideas from philosophy and economics
- Short entries for the days that produce only one useful thought

## Tech stack

| Layer          | Choice                                                                   |
| :------------- | :----------------------------------------------------------------------- |
| Framework      | [Astro](https://astro.build/) (static site, zero client JS by default)   |
| Language       | TypeScript                                                               |
| Styling        | [Tailwind CSS](https://tailwindcss.com/) v4                              |
| Search         | [Pagefind](https://pagefind.app/) (static index built at build time)     |
| Content        | Markdown + MDX, with Shiki syntax highlighting and filename labels       |
| Sharing images | Dynamic OG images via [Satori](https://github.com/vercel/satori) + Sharp |
| Feeds & SEO    | RSS, sitemap, `robots.txt`, canonical URLs, per-post metadata            |
| Deployment     | GitHub Actions → GitHub Pages                                            |

The theme is [AstroPaper](https://github.com/satnaing/astro-paper) by Sat Naing, customized for this site.

## Project structure

```bash
/
├── public/
│   ├── favicon.png                     # site icon
│   ├── google5805ff1a5c362673.html     # Search Console verification
│   └── pagefind/                       # generated at build time, git-ignored
├── src/
│   ├── assets/icons/                   # UI and social icons (SVG)
│   ├── components/                     # Header, Footer, Card, Pagination, ...
│   ├── content/
│   │   ├── pages/about.md              # the About page
│   │   └── posts/                      # every blog post lives here
│   ├── custom/                         # Head / BodyEnd / PostEnd injection points
│   ├── i18n/                           # locale strings (en)
│   ├── layouts/                        # Layout.astro, PostLayout.astro
│   ├── pages/                          # routes: index, posts, tags, archives, rss, og
│   ├── styles/                         # global / theme / typography CSS
│   ├── utils/                          # post sorting, slugs, TOC, filters
│   ├── config.ts                       # runtime config derived from user config
│   └── content.config.ts               # content collections + frontmatter schema
├── astro-paper.config.ts               # site identity, socials, feature flags
├── astro.config.ts                     # base path, integrations, i18n
├── copy-pagefind.mjs                   # copies the search index for local dev
└── docs/deployment-plan.md             # deployment notes and daily writing loop
```

## Running locally

Requires **Node ≥ 22.23.4** and **pnpm**.

```bash
pnpm install
pnpm dev
```

Then open **http://localhost:4321/daily-delta/** — the `/daily-delta` segment is required, because `base` is set to the repository name.

| Command             | Action                                                                  |
| :------------------ | :---------------------------------------------------------------------- |
| `pnpm install`      | Install dependencies                                                    |
| `pnpm dev`          | Start the dev server at `localhost:4321/daily-delta/`                   |
| `pnpm build`        | Type-check, build, run Pagefind indexing, copy the index into `public/` |
| `pnpm preview`      | Preview the production build locally                                    |
| `pnpm sync`         | Generate TypeScript types for Astro modules                             |
| `pnpm format`       | Format with Prettier                                                    |
| `pnpm format:check` | Verify formatting without writing changes                               |
| `pnpm lint`         | Lint with ESLint                                                        |

If `pnpm build` fails with `RangeError: Offset is outside the bounds of the DataView` on Windows, a system proxy is intercepting the local server that Astro starts while generating OG images. Clear the proxy variables for that shell, or bypass `localhost` in the proxy settings.

## Writing a post

Posts are plain Markdown or MDX files in `src/content/posts/`. **The filename becomes the URL**, and subdirectories are folded into the path:

```
src/content/posts/my-first-note.md    →  /daily-delta/posts/my-first-note/
src/content/posts/notes/day-12.md     →  /daily-delta/posts/notes/day-12/
```

Keep filenames lowercase ASCII with hyphens: the slug is taken from the filename verbatim, with no transliteration, so non-ASCII names turn into percent-encoded URLs.

Required frontmatter:

```yaml
---
title: "Post title"
description: "One-line summary, used on cards and in SEO metadata"
pubDatetime: 2026-11-23T20:00:00+08:00
tags: ["code", "reading"]
---
```

Optional fields:

| Field                  | Effect                                                        |
| :--------------------- | :------------------------------------------------------------ |
| `featured: true`       | Pins the post to the featured section on the home page        |
| `draft: true`          | Never published — hidden even in `pnpm dev`                   |
| `modDatetime`          | Shows an "updated on" date; does not change sort order        |
| `ogImage: ./cover.png` | Custom share image; otherwise the generated `/og.png` is used |
| `hideToc: true`        | Hides the table of contents                                   |
| `hideEditPost: true`   | Hides the "edit this page" link                               |
| `canonicalURL`         | Original source URL, for republished writing                  |
| `timezone`             | Overrides the site default (`Asia/Shanghai`) for this post    |

Two behaviors worth knowing:

- **Drafts**: `draft: true` is invisible everywhere. To preview a draft locally, prefix the filename with `_` (e.g. `_draft-note.md`) — the content loader ignores anything starting with `_`.
- **Scheduled posts**: a future `pubDatetime` is visible in `pnpm dev` but hidden in the production build until that calendar day (evaluated in `Asia/Shanghai`). This is how posts get queued in advance.

## Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`, which installs dependencies, runs `pnpm build`, and deploys `dist/` to GitHub Pages. No manual build step is needed for normal writing.

Keep these three values in sync, or the site will build fine but break on the live URL:

| Setting         | File                    | Current value                               |
| :-------------- | :---------------------- | :------------------------------------------ |
| Repository name | GitHub                  | `daily-delta`                               |
| `base`          | `astro.config.ts`       | `/daily-delta`                              |
| `site.url`      | `astro-paper.config.ts` | `https://george-102.github.io/daily-delta/` |

Site identity — title, description, author, timezone, socials, per-page counts, feature flags — lives entirely in `astro-paper.config.ts`.

## Contact

- Email: [su2489544776@outlook.com](mailto:su2489544776@outlook.com)
- GitHub: [George-102](https://github.com/George-102)
- About page: [george-102.github.io/daily-delta/about](https://george-102.github.io/daily-delta/about/)

## Credits & license

Built on the [AstroPaper](https://github.com/satnaing/astro-paper) theme by [Sat Naing](https://satnaing.dev), released under the MIT License. See [LICENSE](LICENSE) for the original copyright notice.

All writing on this site is my own.
