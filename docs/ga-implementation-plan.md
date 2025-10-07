# GA & Site Enhancements Implementation Plan

## High-Level Perspective
Our goals are to: (1) verify `https://qlimber.github.io` in Google Search Console via Google Analytics, (2) instrument the site with GA4 while staying GDPR compliant, and (3) improve post readability by surfacing titles on individual articles. Work spans both the Google Analytics admin console and the Jekyll codebase. This checklist will track implementation progress and ties back to the background research in `docs/google-analytics-research.md`.

## Workstreams & TODOs

### Google Analytics Console
- [ ] Confirm `QLiMBer Projects` GA account has Editor access for your user.
- [ ] Record the `G-` Measurement ID for the `qlimber.github.io` web data stream.
- [ ] Set data retention to 14 months (Admin → Data settings → Data retention).
- [ ] Review enhanced measurement settings; disable anything that conflicts with cookie consent strategy if needed.

### Site Compliance Content
- [ ] Draft a privacy policy page (`privacy-policy.md`) covering GA usage, cookies, and link to Google’s partner privacy policy.
- [ ] Add footer/header navigation link to the privacy policy so it is easily discoverable.

### Cookie Consent & Analytics Injection
- [ ] Choose a consent banner library suitable for static sites (document rationale).
- [ ] Integrate the consent banner assets (CSS/JS) into the Jekyll site.
- [ ] Wrap GA initialisation so it fires only after explicit consent.
- [ ] Include IP anonymisation in the GA config call (`anonymize_ip: true`).

### Layout Updates
- [ ] Create a custom `_layouts/default.html` (or include) to inject shared `<head>` markup.
- [ ] Render `<h1>{{ page.title }}</h1>` (with suitable styling hook) at the top of `_layouts/post.html`.
- [ ] Add meta title/description tags if needed to complement the new heading.

### Verification & QA
- [ ] Deploy changes to GitHub Pages (publish branch and wait for build).
- [ ] Validate locally with `bundle exec jekyll serve`, confirming:
  - [ ] Post pages display titles.
  - [ ] GA snippet only loads after consent.
- [ ] Use browser devtools to confirm GA requests anonymise IP and respect consent state.
- [ ] Re-run Google Search Console verification using the GA method.
- [ ] Smoke-test GA real-time dashboard to ensure page views register after consent.

### Documentation & Housekeeping
- [ ] Update `README.md` (or site docs) summarising analytics/compliance setup.
- [ ] Add changelog entry once the feature set is confirmed.
