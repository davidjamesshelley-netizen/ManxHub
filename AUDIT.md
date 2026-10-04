# ManxHub audit

**Audit date:** 4 October 2026  
**Scope:** repository at `davidjamesshelley-netizen/ManxHub`, its only product
document, GitHub deployment metadata, and read-only checks of
<https://manxhub.com>. The production source and hosting account were not
available, so production findings cannot be fixed from this repository.

## Owner verdict

**PARK the proposed app, but preserve the directory data.** There may be a
useful local business-directory asset here, but this repository is a pitch
document, not a product: it contains no application code, reproducible build,
tests, deployment configuration, or evidence of users or revenue. The live
website is reachable but its core search, category, and business-detail routes
currently serve the same homepage, which makes the directory largely unusable
and undermines its 1,538-URL SEO footprint. Do not invest in native-app scope,
subscriptions, AR, or data licensing yet. First recover the website source,
repair the directory, verify ownership and freshness of the 1,511 claimed
records, and run a small paid-listing or sponsorship test. Resume investment
only if real usage and willingness to pay survive that validation.

## What ManxHub is

Two related things currently share the name:

1. **A live website:** ManxHub describes itself as an Isle of Man business
   directory and community portal with 1,511 records.
2. **A proposed native app:** the PRD proposes a SwiftUI/MapKit iOS app backed by
   Supabase, with offline discovery, TT Week and heritage features, followed by
   consumer subscriptions, business listings, sponsorship, and data licensing.

The repository contains only the second item's PRD. It does not contain the
live site's source or any iOS source.

## Deployment and build status

| Area | Evidence | Honest status |
|---|---|---|
| Repository build | No source tree, project file, package/dependency manifest, build script, or CI workflow | **No build exists.** This is not a passing build. |
| Tests | No unit, integration, UI, smoke, or deployment tests | **0 tests; coverage is not measurable.** |
| GitHub delivery | No Actions workflows, deployments, releases, tags, or GitHub Pages configuration | **Nothing in this repository is deployed.** |
| README/config | Neither existed before this audit; no hosting config exists | Repository purpose and ownership of production were undocumented. |
| Public website | `https://manxhub.com` responds over HTTPS and identifies Cloudflare as the edge server | **Live behind Cloudflare; origin host is unknown.** |
| Native app | No Xcode project or App Store/TestFlight evidence in the repository | **Not buildable or releasable from here.** |

The absence of a failed build must not be reported as “green.” There is no
build pipeline to fail.

## Findings

### Critical — the production directory's core routes are non-functional

Read-only probes returned byte-for-byte identical homepage HTML for:

- `/business/`
- `/services/`
- `/search?q=plumber`
- `/business/tesco-douglas/`

Each returned HTTP 200, the homepage title and H1, and a canonical URL pointing
to `/`. A soft-200 fallback hides outages from uptime checks while users and
search engines receive no directory, result, or profile content. This blocks
the core user value and any credible monetisation.

**Fix in the production repository:** restore route/data handling; return unique
server-rendered content and self-referencing canonicals for valid pages; return
404/410 for invalid pages; add smoke tests for homepage, search, category,
profile, privacy, and terms routes.

### High — SEO is actively being damaged

- The primary sitemap advertises 1,538 URLs, while sampled directory URLs serve
  duplicate homepage content.
- `sitemap-businesses.xml` and `sitemap-categories.xml` return HTTP 200 with
  `text/html` homepage content instead of XML.
- `www.manxhub.com` serves duplicate content instead of redirecting to the
  canonical apex host.
- The homepage itself has a title, meta description, canonical, `lang="en"`,
  one H1, Open Graph title, robots file, and a primary XML sitemap. Those basics
  do not offset the route failures.

**Fix in the production repository:** repair or remove invalid sitemap
references, redirect `www` to the apex host, emit only canonical indexable URLs,
and validate sitemaps in CI and a post-deploy smoke test.

### High — commercial claims are forecasts without validation

The PRD forecasts £56,784 in year-one revenue and assumes 15,630 year-one
installs, 10% consumer premium conversion, 600 TT passes, and 98 paying
businesses across Premium and Pro by month 12. The repository contains no user
research, analytics export, signed partner/customer evidence, payment code,
pricing experiment, acquisition baseline, or revenue record. The live site's
core flows are currently broken.

The directory can become useful before it becomes an app. The cheapest
commercial proof is:

1. restore search and profile pages;
2. measure search success, profile views, outbound calls/directions, returning
   users, and data-correction rate;
3. manually sell a clearly labelled featured listing to a small cohort;
4. record churn, support cost, and attributable leads for at least one renewal
   cycle.

Until then, MRR is best treated as **unproven and potentially £0**, not as a
forecast with investment weight.

### High — confidential planning material is in a public repository

The PRD labels itself “Confidential — Investor & Development Use,” but the
GitHub repository is public. It exposes detailed strategy, pricing, forecasts,
roadmap, named distribution plans, architecture, and personal competitive
claims. This is not a credential leak, but it is an information-classification
failure.

**Owner action:** either make the repository private and review access, or
explicitly declassify and redact the document. A pull request cannot safely
choose that business decision.

### High — proposed backend security is incomplete

The PRD names Supabase tables, authentication, storage, an admin page, claims,
reviews, sponsorship, and payment webhooks but specifies no:

- row-level security policies or deny-by-default grants;
- separation of public client keys from service-role credentials;
- owner/admin authorization model;
- signed webhook verification and replay protection;
- abuse controls for reviews, claims, uploads, search, or notifications;
- audit log, backup/restore test, retention schedule, or deletion workflow;
- validation preventing users from changing listing tier, ownership, aggregate
  ratings, or sponsorship state.

The architecture section now includes a required security baseline, but it
still needs implementation and tests once a backend exists.

### Medium — no exposed credentials found, with limited assurance

A pattern-based scan of the complete tracked history (one commit, three Git
objects before this audit) found no private keys, common tokens, JWTs, Stripe
secret keys, or obvious secret assignments. No secret values were printed.
There are no dependency files or environment files in the repository.

This does **not** audit the unavailable production source, hosting settings,
Supabase project, CI variables, or historical data outside this Git repository.

### Medium — production security headers and privacy posture need work

The live homepage sends `X-Content-Type-Options` and a useful referrer policy,
but no observed HSTS, Content Security Policy, Permissions Policy, or
clickjacking protection. Cloudflare's analytics beacon is loaded. The privacy
policy broadly claims collection of accounts, payments, precise location,
unique device identifiers, and app data even though those capabilities cannot
be verified from this repository; it gives no concrete retention periods or
service-provider list.

**Fix in the production repository and legal review:** add an enforcing CSP,
HSTS after confirming all subdomains support HTTPS, `frame-ancestors`, and a
minimal Permissions Policy. Make the privacy notice match actual processing,
name processors and lawful bases, define retention periods, and document
consent/rights handling under Isle of Man law. Do not rely on the PRD's former
claim that all public business information is automatically outside data
protection law; sole-trader and contact data can be personal data.

### Medium — accessibility is unverified and under-specified

There is no app UI source or accessibility test suite. On the live homepage,
the search text input relies on placeholder text and has no programmatic label.
Static markup checks found language, heading, viewport, and visible button text,
but cannot establish keyboard behavior, focus visibility, contrast, zoom,
screen-reader output, error announcements, or mobile reflow.

The PRD now includes a minimum accessibility gate: VoiceOver labels and order,
Dynamic Type, contrast, reduced motion, touch targets, non-colour cues, and
keyboard/switch access. Production should target WCAG 2.2 AA where applicable
and include automated checks plus manual keyboard, VoiceOver, zoom, and
contrast testing.

### Medium — several planned technical choices were unsafe or inaccurate

- Arbitrarily caching Apple map tiles is not a supported offline strategy.
  Offline maps require a provider and licence that explicitly permit it.
- A 14-day TT pass should be designed and reviewed as fixed-duration access,
  not assumed to be a StoreKit consumable.
- Using third-party business photos or geocoding results requires explicit
  rights and compliance with provider storage/display terms.
- Selling mobility or location-derived data is high-risk even when described as
  “anonymised”; small-island datasets can be re-identifiable.
- A browser admin page built directly around elevated Supabase access requires
  strong authentication, authorization, CSRF protection, audit logging, and
  server-side secret isolation.

These items have been corrected or qualified in the PRD where a documentation
change was safe.

### Dependencies and staleness

There are **no declared dependencies**, so no package can be checked for CVEs or
staleness. The PRD merely proposes Apple frameworks, Supabase, one of two image
libraries, TelemetryDeck, Sentry, weather/tide providers, Google geocoding, and
Stripe. Versions are not pinned and no software bill of materials exists.

This is not a clean dependency audit; it is an absent dependency inventory.
When source is recovered or added, pin direct dependencies, enable automated
updates, run native/package vulnerability checks, and produce an SBOM in CI.

## Safe fixes made in this pull request

- Added a README that accurately identifies this as a planning repository and
  separates it from the live website.
- Replaced unsubstantiated completed roadmap marks with an evidence-based
  delivery rule and unchecked actions.
- Added security, privacy, accessibility, test, CI, and deployment gates to the
  PRD.
- Corrected offline-map, fixed-duration purchase, and third-party-data guidance.
- Added this audit and concrete stop/go criteria.

The production route, sitemap, header, and form-label defects cannot be changed
here because the production source is absent.

## Resume-investment gate

Resume app investment only when all of these are evidenced:

- production source and deployment ownership are identified;
- search, categories, profiles, sitemaps, and canonical routing pass automated
  production smoke tests;
- a representative sample of business records passes ownership, freshness,
  contact, and coordinate checks;
- analytics show repeat use and successful discovery rather than raw pageviews;
- at least one paid B2B offer converts and renews without misleading placement;
- a narrow MVP has a reproducible build, CI, tests, RLS/authorization tests,
  privacy review, and accessibility acceptance criteria;
- the owner can explain why native iOS creates more value than improving the
  already-indexed web directory.
