# Xelacal Fork Design — 2026-04-20

## What this is

`xelacal` is a public fork of `calcom/cal.diy` (MIT) hosted at
`github.com/xelasoft-inc/xelacal`, rebranded as Xelasoft's in-house scheduling
tool. Deployed on Railway.

## Goals

- Self-hosted scheduling with Google Calendar + Meet.
- Single user (me) taking bookings under multiple client brand identities.
- Embeddable iframe with neutral chrome so it blends into any host site.
- Keep the ability to merge upstream (`calcom/cal.diy`) improvements forever.

## Non-goals

- Teams, Organizations, Workflows, SSO — removed in cal.diy and not re-added.
- Per-event-type logos — schema change rejected; event color is sufficient.
- Per-customer instance sharding — one deployment serves all brand embeds.

## Decisions

### Multi-brand approach
Single Railway instance. Embed iframe chrome stays neutral (no Xelasoft marks
inside the embed). Each event type uses the existing `eventTypeColor` field
to theme its booking flow. Admin-side color picker already ships in cal.diy.

### Branding identity
User-facing product brand = **Xelasoft** (uses `/Users/sashkode/Projects/xelasoft/brand/` assets).
"Xelacal" is the repo / internal codename only — never shown to users.

### Branding strip tiers
- **T1 (replace):** UI strings "Cal.com"/"Cal", favicons, apple-touch-icon,
  email template logos + "from name", HTML `<title>`, meta tags, PWA manifest,
  OG image defaults, "Powered by" footer.
- **T2 (replace):** Loading screens, skeleton logos, OG image generator output.
- **T3 (leave):** Code identifiers (`calEvent`, `calUsername`), internal comments,
  LICENSE (preserve MIT + calcom copyright, add Xelasoft line), README
  (update to note fork).

Rationale for T3: preserving code identifiers keeps future `git merge
upstream/main` tractable. Renaming variables would guarantee endless conflicts.

### Fork strategy
Real public GitHub fork (not a re-origined clone), giving clean `upstream`
tracking and preserving MIT attribution naturally. Fork command used:
`gh repo fork calcom/cal.diy --org xelasoft-inc --fork-name xelacal --clone`.

### Asset mapping
| Target | Source |
|---|---|
| `favicon.ico`, `favicon-32x32.png` | `xelasoft-icon-32.png` |
| `apple-touch-icon.png` (180) | `xelasoft-icon-180.png` |
| PWA icons (512, maskable) | `xelasoft-icon-512.png`, `xelasoft-icon-2000.png` |
| Nav/header logo | `xelasoft-lockup-horizontal.svg` |
| Email header logo | `xelasoft-lockup-horizontal-h128.png` |
| OG image (1200x630) | generated: icon centered on brand background |
| Loading / empty states | `xelasoft-icon.svg` |

### Hosting
- **Platform:** Railway
- **Services:** Next.js app + Postgres plugin + Redis plugin
- **SMTP:** Gmail with app password (admin emails only — attendee emails ride
  on Google Calendar invites).
- **OAuth:** Google Cloud OAuth 2.0 Web Client; redirect URI set to the Railway
  domain after first deploy.

## Upstream sync protocol

Periodic `git fetch upstream && git merge upstream/main`. Expected conflict
zones: branding asset files, email templates, meta tags. Code identifier files
should merge cleanly because T3 leaves them alone.
