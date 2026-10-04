# ManxHub

This repository is a **planning repository**, not the source code for the live
ManxHub website or an iOS application. It currently contains the product
requirements for a proposed Isle of Man discovery app.

## Current status

- The public directory is reachable at <https://manxhub.com> and is fronted by
  Cloudflare. Its application source and origin-host configuration are not in
  this repository.
- This repository has no app source, dependency manifest, build configuration,
  automated tests, CI workflow, release, or deployment configuration.
- No GitHub deployment or GitHub Pages site is configured for this repository.
- The proposed iOS app is not buildable from this repository.

Do not treat roadmap checkboxes in the PRD as proof of delivery. A feature is
complete only when its implementation, tests, and release evidence can be
linked.

## Documents

- [Product requirements](IOM_DISCOVERY_APP_PRD.md)
- [Technical and commercial audit](AUDIT.md)

## Before implementation

1. Decide whether the existing website or a native iOS app is the primary
   product.
2. Fix and measure the existing directory before funding another client: search,
   category, and business-detail routes must return real results.
3. Validate data ownership, data freshness, and willingness to pay with actual
   users and businesses.
4. Add application source with a reproducible local build, CI, tests, secret
   handling, and deployment documentation.

## Security

Never commit credentials. Client applications may contain only public client
identifiers or explicitly publishable keys. Supabase service-role keys, payment
secrets, signing keys, and admin credentials belong in a managed secret store
and must only be used by trusted server-side code.
