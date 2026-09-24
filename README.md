# KinetiCore / THOR website — clean deploy

## Structure (this is the whole repository)

```
wrangler.jsonc      <- tells Cloudflare where the site files are. No build needed.
public/
  index.html         <- the complete, working site (verified, screenshotted, confirmed working)
  _headers            <- security headers (CSP, HSTS, etc.)
README.md
```

That's it. One folder deep, everything visible at a glance.

## Why this structure specifically

Today's session established, the hard way, that Cloudflare now runs every
deployment — even plain static sites — through its Workers/Wrangler system,
using a `wrangler.jsonc` file to know what to serve. The recurring failure
all day was the dashboard's **Build command** setting not taking effect,
which matters only when something actually needs building (like the Astro
version).

This version sidesteps that problem entirely: `index.html` is already
finished, nothing needs to be compiled, so there's nothing for a Build
command to do. `wrangler.jsonc` just points straight at the `public` folder
and Cloudflare uploads it as-is.

## Cloudflare setup, this time

1. Workers & Pages → Create application → **Import Git repository** (not a
   template) → GitHub → select the repository.
2. Build settings:
   - **Root directory:** leave blank / `/` (default) — `wrangler.jsonc`
     sits right at the repo root, exactly where Cloudflare looks for it.
   - **Build command:** leave empty — nothing to build.
   - **Deploy command:** leave as the default (`npx wrangler deploy`) —
     it reads `wrangler.jsonc` automatically.
3. Deploy.
4. Domains tab → Add custom domain → `hansbrinckers.solutions`.

## What's deliberately not in this version

The Astro project (component structure, `src/data/site.ts`, etc.) still
exists in the repo's earlier history — it's the right long-term structure
and nothing about it was wrong conceptually. It's just not this deployment.
Once this plain version is confirmed live and working, upgrading to the
Astro structure is a separate, lower-stakes step — the deployment pattern
will already be proven by then.
