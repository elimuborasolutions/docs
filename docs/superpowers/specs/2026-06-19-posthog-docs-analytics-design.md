# PostHog Analytics for the Elimu Bora Help Center (Mintlify)

**Date:** 2026-06-19
**Status:** Approved design, pending implementation plan

## Goal

Add PostHog analytics to the Mintlify help-center docs, feeding the same
PostHog project already used by the marketing website and the ERP app, so docs
traffic is visible alongside the rest of the product.

## Key constraint: hosted Mintlify, not a code-instrumented app

The marketing website (Astro) and the ERP (Laravel/Vite) both call
`posthog.init()` directly and hand-instrument custom events. **That is not
possible here.** The docs run on hosted Mintlify — there is no access to the
React theme, no `posthog-js` dependency, and no place to write `init()` or
`capture()` calls. The only available mechanism is Mintlify's native
`integrations.posthog` block in `docs.json`.

### What the docs get
- Automatic pageview tracking
- Autocapture (link/button clicks, navigation)
- Session replay

### What the docs do NOT get (and cannot)
- Custom funnel events (e.g. `contact_form_submitted`, `pricing_plan_cta_clicked`)
- `identify()` by email
- Any hand-instrumented capture

So "just like the website" is not literally achievable; the docs receive the
autocapture layer only.

## Shared project decision

Data goes to the **existing** PostHog project (156223) on **EU cloud**, using
the shared project token `phc_qpX7FwgEJa6osP32UU2YEpCp6hYGcwbzVBUdtdnhgPz6`.
This unifies docs traffic with website and ERP data; an anonymous user can be
followed across properties. Docs traffic is separated in PostHog by filtering
on `$host` / `$current_url`.

## The change

A single new top-level key in `docs.json`:

```json
"integrations": {
  "posthog": {
    "apiKey": "phc_qpX7FwgEJa6osP32UU2YEpCp6hYGcwbzVBUdtdnhgPz6",
    "apiHost": "https://b.elimuboraerp.com",
    "sessionRecording": true
  }
}
```

### Field rationale
- **`apiKey`** — shared project token (same as website and ERP).
- **`apiHost`** — set to the self-hosted reverse proxy `https://b.elimuboraerp.com`,
  matching the ERP app. This routes to PostHog EU cloud and resists adblockers.
  It must be set explicitly: Mintlify's default proxy (`ph.mintlify.com`) routes
  to **US** ingestion and would silently drop data into the wrong region.
  Verified live during design: the proxy serves PostHog assets
  (`/static/array.js` → HTTP 200, `application/javascript`,
  `access-control-allow-origin: *`), so cross-origin requests from the docs
  domain succeed.
- **`sessionRecording`** — `true` to enable replay.

## No PostHog dashboard changes required

"Authorized domains for recordings" is **deprecated**. When the list is empty
(it is), PostHog records on all domains by default — which is why the website
and ERP already record sessions without any domain configuration. There is
**nothing to add** in PostHog for the docs. Replay works out of the box once the
integration is enabled.

## Optional follow-up (not required)

Build a docs-scoped insight/dashboard filtered on `$host` to separate help-center
traffic from marketing and app traffic within the shared project.

## Verification

Config-only change. Verify by running `mint dev` (or on the deployed docs) and
confirming a `$pageview` event for the docs host appears in the PostHog
project's live events / activity feed, and that a session replay is captured.