# Google Analytics & Search Console Notes

## Goal
Outline how to verify `https://qlimber.github.io` in Google Search Console using Google Analytics (GA), plus layout considerations for exposing post titles and adding GA snippets site-wide. No implementation performed yet.

## Search Console Verification via Google Analytics
- **Prerequisites**
  - A GA property that covers `https://qlimber.github.io` (GA4 is fine). Create a web data stream and keep the `G-XXXX` Measurement ID handy.
  - You (QLiMBer Google account) must have the **Editor** role on that GA property; Search Console checks this during verification.
  - The published site must load the GA snippet (either `gtag.js` for GA4 or `analytics.js` for Universal Analytics) in the `<head>` of every page—especially the homepage `https://qlimber.github.io/` because that is what Search Console fetches for verification.

- **Implementation Steps (once we decide to proceed)**
  1. In GA, open *Admin → Data Streams → Web* and copy the `gtag.js` snippet provided for the site.
  2. Inject the snippet into the site’s `<head>` so it outputs on every page, then push/deploy the change to GitHub Pages and wait for the site to rebuild.
  3. Back in Search Console, add a **URL prefix** property for `https://qlimber.github.io/` (domain-level verification is not possible because we do not control `github.io` DNS).
  4. Choose **Google Analytics** as the verification method and click *Verify*. Search Console will fetch the homepage, detect the GA tag, confirm you have Editor permissions, and mark the property as verified.
  5. Keep the GA snippet in place; removing it will eventually cause Search Console to revoke verification.

- **Pros**
  - Unlocks GA traffic metrics and Search Console SEO tooling with a single snippet.
  - No need to upload a standalone HTML file or manage DNS records that are inaccessible for GitHub Pages subdomains.
  - Future redeploys keep working automatically, provided the GA snippet remains in the layout.

- **Cons / Considerations**
  - Adds an extra network request (~45–50 KB) and JavaScript execution to every page, slightly impacting load time.
  - Introduces privacy/compliance responsibilities (cookie consent, privacy policy updates) that may be required depending on jurisdiction.
  - Relies on third-party analytics cookies/scripts; if blocked (e.g., by an ad blocker), Search Console verification can fail. Usually rerunning verification after a redeploy fixes it.
  - Requires continued GA access—losing Editor rights or deleting the property breaks verification.

## Current Layout Findings
- `_layouts/post.html` wraps each article but does **not** render `page.title`; only `{{ content }}` appears. Posts still show titles on the home page because the theme’s index template handles it, but individual post pages lack a header.
- The site inherits `jekyll-theme-hacker`’s default layout with no local override, so there is currently no central place to inject analytics scripts. Adding a custom `_layouts/default.html` (based on the theme’s version) or using custom head includes would give us a hook for GA and other meta tags.

## Potential Implementation Approaches (for future work)
- **Expose Post Titles:** Update `_layouts/post.html` to include a heading, e.g. place `<h1>{{ page.title }}</h1>` before `{{ content }}`. We can style it as needed to match the Hacker theme.
- **Inject GA Site-Wide:**
  - Option A: Set `google_analytics: G-XXXXXXX` in `_config.yml` and add a small include that renders the GA4 snippet whenever that config value exists. (GitHub Pages’ built-in GA helper only supports legacy UA IDs, so for GA4 we would manage the snippet ourselves.)
  - Option B: Copy the theme’s `default.html` into `_layouts/default.html`, then add the GA `<script>` block just before `</head>` so every page picks it up.
  - Option C: Create `_includes/head-custom.html` and reference it from our custom layout; this keeps analytics, meta tags, and verification snippets in one place.

When we are ready to implement, we can iterate on the layout updates, run `bundle exec jekyll serve`, and confirm the rendered HTML includes the new title headings and GA tags before pushing live.

## Compliance To-Dos (pre-implementation)
- Publish a visible privacy policy page that discloses GA usage, links to Google’s partner privacy statement, and explains cookie usage.
- Add an EU-compliant cookie consent banner that blocks the GA snippet until the visitor opts in.
- Configure the GA snippet with IP anonymisation (`gtag('config', 'G-XXXX', { anonymize_ip: true })`).

## Consent Implementation Rationale (chosen approach)
- Instead of relying on a third-party banner library, we implemented a custom consent component in Liquid/JS. That keeps dependencies small, avoids extra CDN calls, and gives full control over when GA is initialised.
- Consent state lives in `localStorage` under `ga_consent_state`, with helpers to reopen the banner via “Cookie Preferences” in the site nav.
- GA only loads after the visitor presses “Accept”. The script sets `window['ga-disable-GAID'] = true` by default, flips it on acceptance, and calls `gtag('config', …, { anonymize_ip: true })`.
- On withdrawal we immediately run `gtag('consent', 'update', { analytics_storage: 'denied' })` and block future loads until re-consent.

## GA Configuration Notes
- Enhanced measurement currently keeps only **Page views**, **Scrolls**, and **Outbound clicks** enabled; all other automatic events are disabled. Update the privacy policy if this list changes.
- Measurement ID lives in `_config.yml` under `ga_measurement_id`. Deploys without that value present will render the banner but not load GA (fails safe).
