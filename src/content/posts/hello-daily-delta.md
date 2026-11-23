---
title: "Hello, Daily Delta"
description: "A first test post that walks through the features this blog template gives me: headings, code blocks, callouts, tables, and tags."
pubDatetime: 2026-11-23T20:00:00+08:00
tags: ["meta", "writing"]
featured: true
---

This is the first post on Daily Delta. I am writing it mostly as a smoke test: if you can read it, then the Markdown pipeline, the styling, the table of contents, and the tag system are all wired up correctly.

The plan is simple. One small increment every day, written down in public, until the increments add up to something worth looking back on.

## Why write in public

Writing forces me to make my understanding explicit. A note I cannot explain clearly is a note I have not really learned yet. Publishing keeps me honest about that gap.

## What this post verifies

| Feature           | Where it shows up                                | Expected result                     |
| ----------------- | ------------------------------------------------ | ----------------------------------- |
| Table of contents | Right side of this page (or the floating button) | Links jump to headings              |
| Code highlighting | The block below                                  | Light and dark themes both readable |
| Callouts          | The note box below                               | Rendered with an icon and a border  |
| Tags              | `meta`, `writing`                                | Both tag pages list this post       |
| Featured section  | The home page                                    | This post appears first             |

## A small code sample

```ts file="daily-delta.ts"
type Increment = {
  date: string;
  minutes: number;
};

const log: Increment[] = [
  { date: "2026-11-22", minutes: 45 },
  { date: "2026-11-23", minutes: 30 }, // [!code highlight]
];

const total = log.reduce((sum, entry) => sum + entry.minutes, 0);

console.log(`Total focus time: ${total} minutes`); // [!code ++]
```

> [!note]
> The numbers above are placeholders. The real entries will live in the posts themselves, not in a code block.

## What comes next

- Publish a short note whenever a day produces one useful thought
- Keep longer essays for the ideas that refuse to stay short
- Never publish something I would be embarrassed to re-read in a year

If any part of this page looks broken, the problem is the site, not the idea. You can reach me through the [About page](https://george-102.github.io/daily-delta/about/).
