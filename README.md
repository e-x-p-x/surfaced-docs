# Surfaced help centre

The public documentation for [Surfaced](https://www.getsurfaced.ai), built with
Mintlify and served at docs.getsurfaced.ai.

## Why this is its own repository

The product lives in `e-x-p-x/surfaced`. The docs do not, for two reasons.
Mintlify validates that `docs.json` sits at the root of the connected
repository's default branch, and in the monorepo it would have had to live under
a subdirectory. More importantly, a docs deploy has no business riding on every
product merge: a typo fix here should not wait on the product CI, and a product
hotfix should not republish the docs.

`docs.getsurfaced.ai` still resolves through the product's Vercel deployment,
because the password-gated client documents that used to live there keep their
old links working: `proxy.ts` 301s those slugs to `clients.getsurfaced.ai` and
passes everything else through to this site. The client documents and this help
centre therefore share a hostname but nothing else.

## Layout

`docs.json` is the whole configuration: theme, colours, logo, and the navigation
tree. Every page referenced in `navigation` must exist as an `.mdx` file at the
matching path, and every `.mdx` file that is not referenced is unreachable, so
the two are kept in step by hand.

The Guides tab mirrors the app's own sidebar (`lib/navigation/admin-config.ts`
in the product repo), so that a reader who is looking at a screen in Surfaced
can find the page about it in the same order they see it. The API tab is
generated from the live OpenAPI documents rather than copied, so it cannot drift.

## Working on it

```sh
npm i -g mint     # once
mint dev          # local preview on :3000
mint validate     # build + lint; this is what CI would run
mint broken-links # internal link check
```

A push to `main` deploys. There is no staging branch; preview deployments come
from pull requests.

## Conventions

Screenshots come from `/demo` only — never from a real workspace, because real
workspaces contain customer domains and prompt text. Alt text describes what the
screenshot shows, not that it is a screenshot.

Pages about a metric carry four things: what it means, how it is calculated,
a worked example, and where to find it in the app. A metric page without the
formula is the one readers write to support about.

Only features that are live for everyone are documented. Anything still behind a
flag stays out until the flag is gone.
