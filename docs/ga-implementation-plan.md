# GA & Site Enhancements Implementation Plan

## High-Level Perspective
Our goals are to: (1) verify `https://qlimber.github.io` in Google Search Console via Google Analytics, (2) instrument the site with GA4 while staying GDPR compliant, and (3) improve post readability by surfacing titles on individual articles. Work spans both the Google Analytics admin console and the Jekyll codebase. This checklist will track implementation progress and ties back to the background research in `docs/google-analytics-research.md`.

## Workstreams & TODOs

### Google Analytics Console
- [ ] Confirm `QLiMBer Projects` GA account has Editor access for your user.
- [x] Record the `G-` Measurement ID for the `qlimber.github.io` web data stream (set in `_config.yml`).
- [ ] Set data retention to 14 months (Admin → Data settings → Data retention).
- [x] Review enhanced measurement settings; disable anything that conflicts with cookie consent strategy if needed (kept Page views, Scrolls, Outbound clicks only).

### Site Compliance Content
- [x] Draft a privacy policy page (`privacy-policy.md`) covering GA usage, cookies, and link to Google’s partner privacy policy.
- [x] Add footer/header navigation link to the privacy policy so it is easily discoverable.

### Cookie Consent & Analytics Injection
- [x] Implement a lightweight consent banner tailored for static hosting and note the rationale in docs.
- [x] Integrate the consent banner assets (CSS/JS) into the Jekyll site.
- [x] Wrap GA initialisation so it fires only after explicit consent.
- [x] Include IP anonymisation in the GA config call (`anonymize_ip: true`).

### Layout Updates
- [x] Create a custom `_layouts/default.html` (or include) to inject shared `<head>` markup.
- [x] Render `<h1>{{ page.title }}</h1>` (with suitable styling hook) at the top of `_layouts/post.html`.
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
- [x] Update `README.md` (or site docs) summarising analytics/compliance setup.
- [ ] Add changelog entry once the feature set is confirmed.
