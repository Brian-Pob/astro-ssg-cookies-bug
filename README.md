# Demonstrating bug with hybrid SSG output.

## Expected behavior

I should not be able to use Astro cookies when running in hybrid mode **without** `export const prerender = false;` on a dev server. I expected some type of warning or perhaps an error.

## Observed behavior

I am able to use Astro cookies when running in hybrid mode **without** `export const prerender = false;`.

## Steps to reproduce

1. `npm run dev`
1. Visit dev server link in browser
1. Observe the site cookies
1. Refresh the page
1. The cookies related code within `Footer.astro` works despite a lack of `export const prerender = false;`

## Info

| | |
|-------|--------|
|Astro | v4.4.5 |
|Node                    | v18.17.1|
|System                  | macOS (arm64)|
|Package Manager         | bun|
|Output                  | hybrid|
|Adapter                 | @astrojs/vercel/serverless|
|Integrations            | @astrojs/mdx, @astrojs/sitemap|

---

Contents of [astro.config.mjs](astro.config.mjs)
```js
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';
import sitemap from '@astrojs/sitemap';

import vercel from "@astrojs/vercel/serverless";

// https://astro.build/config
export default defineConfig({
  site: 'https://example.com',
  integrations: [mdx(), sitemap()],
  output: "hybrid",
  adapter: vercel()
});
```

Contents of [Footer.astro](src/components/Footer.astro) frontmatter
```js
const today = new Date()

let counter = 0

if (Astro.cookies.has("counter")) {
  const cookie = Astro.cookies.get("counter")
  counter = cookie.number() + 1
}

Astro.cookies.set("counter",counter)
```
