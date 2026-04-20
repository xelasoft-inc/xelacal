# Xelacal Branding Audit — 2026-04-20

Read-only audit produced before the rebrand execution pass. Lists every
user-visible Cal/Cal.com/Cal.diy touchpoint in the codebase with proposed
replacements. Paths are relative to `/Users/sashkode/repos/xelacal/` unless
absolute.

Tier legend:
- **T1**: replace with Xelasoft equivalent
- **T2**: replace with Xelasoft equivalent (lower priority)
- **T3**: LEAVE unchanged (code identifier, test, license, etc.) — preserves
  upstream merge compatibility.

## 1. Assets to replace (public/ + emails/)

All icon assets are served through `/api/logo?type=...` (`apps/web/app/api/logo/route.ts`), which falls back to the `public/*` files listed below when no team has a custom logo. Replacing only the public files covers the self-hosted default path.

| Current asset | Role | Referenced from | Replace with |
|---|---|---|---|
| `apps/web/public/favicon.ico` | favicon (legacy) | implicit `/favicon.ico` | rasterized from `xelasoft-icon.svg` |
| `apps/web/public/favicon-16x16.png` | favicon 16 | `packages/lib/constants.ts` FAVICON_16 | `xelasoft-icon-32.png` downscaled to 16 |
| `apps/web/public/favicon-32x32.png` | favicon 32 | FAVICON_32 | `xelasoft-icon-32.png` |
| `apps/web/public/apple-touch-icon.png` | iOS home-screen 180 | APPLE_TOUCH_ICON | `xelasoft-icon-180.png` |
| `apps/web/public/android-chrome-192x192.png` | PWA 192 | manifest + /api/logo | resize `xelasoft-icon-512.png` to 192 |
| `apps/web/public/android-chrome-256x256.png` | PWA 256 | ANDROID_CHROME_ICON_256 | resize 512 to 256 |
| `apps/web/public/android-chrome-384x384.png` | PWA 384 | manifest | resize 512 to 384 |
| `apps/web/public/android-chrome-512x512.png` | PWA 512 | app.json logo | `xelasoft-icon-512.png` |
| `apps/web/public/mstile-{70,150,310x150,310x310}.png` | Windows tile | browserconfig.xml | resize `xelasoft-icon-mono-black.png` |
| `apps/web/public/safari-pinned-tab.svg` | Safari mask | app/layout.tsx:51, pages/_document.tsx:72 | `xelasoft-icon-mono-black.svg` |
| `apps/web/public/calcom-logo-white-word.svg` | default LOGO | constants.ts:101 LOGO → OgImages | `xelasoft-lockup-horizontal-light.svg` |
| `apps/web/public/cal-logo-word-black.svg` | LOGO_DARK | constants.ts:102 | `xelasoft-lockup-horizontal.svg` |
| `apps/web/public/cal-logo-word-dark.svg` | video splash | videos-single-view.tsx:195 | `xelasoft-lockup-horizontal.svg` |
| `apps/web/public/cal-logo-word.svg` | variant | unreferenced | delete or replace |
| `apps/web/public/cal-com-icon-white.svg` | LOGO_ICON | constants.ts:103 | `xelasoft-icon-light.svg` |
| `apps/web/public/cal-com-icon.svg` | OAuth consent | modules/auth/oauth2/authorize-view.tsx:158 | `xelasoft-icon.svg` |
| `apps/web/public/calcom-white.svg` | wordmark variant | Logo.tsx | Xelasoft wordmark |
| `apps/web/public/og-image.png` | default social OG | constants.ts:117 SEO_IMG_DEFAULT | regenerate with Xelasoft lockup |
| `apps/web/public/video-og-image.png` | Cal Video OG | constants.ts:123 | regenerate |
| `apps/web/public/social-bg-dark-lines.jpg` | OG dark bg | OgImages.tsx:167 | neutral Xelasoft bg |
| `apps/web/public/social-bg-light-lines.jpg` | OG light bg | OgImages.tsx | neutral Xelasoft bg |
| `apps/web/public/emails/logo.png` | email header | EmailBodyLogo.tsx:9, confirm-email.html | rasterize `xelasoft-lockup-horizontal-h128.png` |
| `apps/web/public/emails/CalLogo@2x.png` | email masthead retina | email templates | Xelasoft lockup retina |
| `apps/web/public/emails/calendar-email-hero.png` | invite email hero | TeamInviteEmail, OrgAutoInviteEmail, OrganizationCreationEmail | regenerate Xelasoft-branded hero |
| `apps/web/public/continue-with-calcom-*.svg` (8 variants) | "Sign in with Cal" buttons | external sites | **AMBIGUOUS** — can delete (we aren't a SaaS identity provider) |
| `apps/web/public/product-cards/*.svg`, `g2.svg`, `producthunt.svg`, `google-reviews.svg`, `trustpilot*.svg` | review badges | signup-view.tsx:786-833 (wrapped in IS_CALCOM) | hidden on self-hosted; delete for tidiness |

Source assets directories:
- SVG: `/Users/sashkode/Projects/xelasoft/brand/svg/`
- PNG (pre-rasterized at 32/180/512/2000): `/Users/sashkode/Projects/xelasoft/brand/png/`

## 2. Text string replacements

`APP_NAME` in `packages/lib/constants.ts` is threaded through i18n as `{{appName}}` — flipping it covers most of the UI automatically. Still change hard-coded defaults.

### Config & top-level
| File | Line | Current | Proposed |
|---|---|---|---|
| `app.json` | 2 | `"name": "Cal.diy"` | `"xelacal"` |
| `app.json` | 3 | `"description": "Open Source Scheduling"` | `"Xelasoft scheduling"` |
| `app.json` | 4 | `"repository": "https://github.com/calcom/cal.diy"` | `https://github.com/xelasoft-inc/xelacal` |
| `app.json` | 5 | `"logo": "https://cal.com/android-chrome-512x512.png"` | placeholder Xelasoft logo URL |
| `README.md` | top matter | "Cal.diy" hero, cal.com links | Xelasoft intro; retain upstream MIT attribution |

### Constants (single source for most labels)
| File:line | Current | Proposed |
|---|---|---|
| `packages/lib/constants.ts:37` | `WEBSITE_URL ... "https://cal.com"` | Xelasoft website (placeholder OK) |
| `:38` | `APP_NAME = ... \|\| "Cal.diy"` | `"Xelasoft"` |
| `:39` | `SUPPORT_MAIL_ADDRESS = ... \|\| "help@cal.com"` | internal Xelasoft support |
| `:40` | `COMPANY_NAME = ... \|\| "Cal.com, Inc."` | `"Xelasoft, Inc."` |
| `:41` | `SENDER_ID = ... \|\| "Cal"` | `"Xelasoft"` |
| `:42` | `SENDER_NAME = ... \|\| "Cal.diy"` | `"Xelasoft"` |
| `:111-116` | `ROADMAP`, `DESKTOP_APP_LINK`, `JOIN_COMMUNITY`, `POWERED_BY_URL`, `DOCS_URL`, `DEVELOPER_DOCS` cal.com URLs | empty strings or internal URLs |

### Hard-coded product strings
| File:line | Current | Proposed |
|---|---|---|
| `apps/web/modules/auth/login-view.tsx:186` | `<h1>Cal.diy</h1>` | `{APP_NAME}` |
| `apps/web/modules/auth/verify-email-view.tsx:31` | Yahoo search `p=Cal.diy` | `p=Xelasoft` |
| `apps/web/pages/router/index.tsx:20` | `<title>Cal.diy Forms</title>` | `{APP_NAME} Forms` |
| `apps/web/app/(use-page-wrapper)/refer/page.tsx:11,13` | "Cal.diy referral program…" | "Xelasoft referral program…" |
| `apps/web/public/service-worker.js:16` | `"New Cal.diy Notification"` | `"New Xelasoft Notification"` |
| `apps/web/public/service-worker.js:17` | `https://cal.com/api/logo?type=icon` | relative `/api/logo?type=icon` |
| `apps/web/components/apps/alby/Setup.tsx:105` | `description: "Cal.diy"` | use APP_NAME |
| `apps/web/components/apps/paypal/Setup.tsx:88` | "Here in Cal.diy we offer Paypal..." | use APP_NAME |
| `apps/web/modules/signup-view.tsx:789,797,805,838,843` | IS_CALCOM block alts | safe to delete whole block or rewrite alts |
| `apps/web/modules/embed/components/Embed.tsx:680-691` | "powered by Cal.diy" banner in generated Email Embed snippet | **REMOVE entire footer `<div>`** — embed must stay brand-neutral |
| `packages/ui/components/logo/Logo.tsx:20,24,25` | `alt="Cal.diy" title="Cal.diy"` | `{APP_NAME}` |
| `packages/features/auth/lib/next-auth-options.ts:292` | `name: "Cal.diy"` CredentialsProvider | APP_NAME |
| `packages/prisma/zod-utils.ts:545` | `fontSrc default "https://cal.com/cal.ttf"` | internal URL or remove default |
| `packages/lib/__mocks__/*` | mocks | **T3 LEAVE** |

### App-store publisher strings (AMBIGUOUS — in-app catalog)
Bulk replace `"Cal.diy"` / `"Cal.com, Inc."` → `"Xelasoft"` in `packages/app-store/*/{config.json,_metadata.ts,DESCRIPTION.md}` — `publisher` fields are user-visible in the internal app-store UI.

### T3 LEAVE
- Comments, tests, Playwright fixtures, package.json `description`
- i18n keys (`disable_cal_branding`, `removes_cal_branding` — key names are stable; values use `{{appName}}`)
- iCal PRODID `-//Cal.diy//`
- `@calcom/*` package names
- `CalEvent*`, `calUsername` identifiers
- `.cal.com` hostname detection in `IS_CALCOM`/`CAL_DOMAINS`
- LICENSE, CONTRIBUTING.md, CODE_OF_CONDUCT.md

## 3. Meta tags & HTML head

| File:line | Current | Proposed |
|---|---|---|
| `apps/web/app/layout.tsx:68` | `application-TileColor: "#ff0000"` | `#FFFFFF` or Xelasoft color |
| `:71` | `site/creator: "@calcom"` | remove or replace |
| `:51-52` | `url: "/safari-pinned-tab.svg", color: "#000000"` | keep URL, swap asset |
| `apps/web/pages/_document.tsx:72-75` | mask-icon color, TileColor `#ff0000`, theme-color | Xelasoft colors |
| `packages/lib/next-seo.config.ts:36-37` | `handle/site: "@calcom"` | remove or replace |
| `apps/web/public/browserconfig.xml:6` | `<TileColor>#ff0000</TileColor>` | Xelasoft color |

## 4. PWA manifest

Only one: `apps/web/public/site.webmanifest`

| Line | Current | Proposed |
|---|---|---|
| 2 | `"name": "Cal.diy"` | `"Xelasoft"` |
| 3 | `"short_name": "Cal.diy"` | `"Xelasoft"` |
| 4 | `"description": "Scheduling infrastructure for absolutely everyone."` | `"Xelasoft internal scheduling."` |
| 7-15 | icons via `/api/logo?type=...` | leave dynamic; new PNGs picked up |

## 5. Email templates

| File:line | Element | Change |
|---|---|---|
| `packages/emails/src/components/EmailBodyLogo.tsx:9` | `image = ${WEBAPP_URL}/emails/logo.png` | keep path; swap PNG |
| `packages/emails/src/templates/TeamInviteEmail.tsx:46-47` | hero `calendar-email-hero.png` | swap PNG |
| `packages/emails/src/templates/OrgAutoInviteEmail.tsx:43-44` | same | same |
| `packages/emails/src/templates/OrganizationCreationEmail.tsx:27` | same | same |
| `packages/emails/templates/confirm-email.html:421` | `>Cal.diy<` masthead | `Xelasoft` |
| `:583,607,616` | "log in to Cal.diy", "Log into Cal.diy", "The Cal.diy Team" | Xelasoft variants |
| `:646` | `&copy; 2025 Cal.diy. All rights reserved.` | `&copy; 2026 Xelasoft. All rights reserved.` |
| `packages/emails/templates/oauth-client-approved-notification.test.ts:32,76` | test fixture | **T3 LEAVE** |

All `organizer-*`, `attendee-*`, etc. render via `V2BaseEmailHtml → EmailBodyLogo` and pull APP_NAME → no per-template edits needed once constants + assets update.

## 6. Embed iframe

| File | Finding | Action |
|---|---|---|
| `packages/embeds/embed-core/src/embed-iframe.ts:282,353,550` | Dev-facing console logs mention cal.com/Cal.diy | **T3 LEAVE** |
| `packages/embeds/embed-core/src/embed.ts:343-346` | Hard-coded `app.cal.com`/`cal.com` trusted origins | **FUNCTIONAL ISSUE** — will block Xelasoft-domain embeds. Must read from `WEBAPP_URL`/env. Flag to execution. |
| `packages/embeds/embed-core/src/styles.css:7` | `src: url("https://cal.com/cal.ttf")` | Point to `WEBAPP_URL/fonts/cal.ttf` or remove |
| `apps/web/modules/embed/components/Embed.tsx:680-691` | "powered by Cal.diy" footer in Email Embed snippet | **REMOVE entire `<div>`** |
| `apps/web/modules/bookings/components/Booker.tsx:555-565` | Already renders `{null}` under `!hideBranding` | **T3 LEAVE** — chrome already neutral |

## 7. OG image generator

Entry: `apps/web/app/api/social/og/image/route.tsx` (edge). Components: `packages/lib/OgImages.tsx`.

Three image types each using `LOGO` from constants.ts:
1. **meeting** — dark bg + LOGO wordmark. Replace SVG + bump `OG_ASSETS.meeting.id: "meeting-og-image-v1"` → `"v2"` (OgImages.tsx:55).
2. **app** — light bg + LOGO. Bump `OG_ASSETS.app.id` (OgImages.tsx:61).
3. **generic** — light bg + LOGO_DARK. Bump `OG_ASSETS.generic.id` (OgImages.tsx:67).

Fetches `/fonts/cal.ttf` (route.tsx:40). Decide: keep CalSans (name-only, no branded text) or swap to Xelasoft brand font. Soft-fails on fetch error so leaving is safe.

## 8. Effort estimate

| Tier | File count | Notes |
|---|---|---|
| T1 assets | ~22 in `apps/web/public/` + `public/emails/` | binary swaps from `/Users/sashkode/Projects/xelasoft/brand/*` |
| T1 config/strings | ~16 source files | concentrated in `constants.ts` + `public/` |
| T2 app-store publisher | ~25-30 files | bulk-replaceable |
| T3 LEAVE | everything else | |

Total: ~65-75 files. `constants.ts` + asset swap deliver most visible rebrand via `{{appName}}` i18n threading.
