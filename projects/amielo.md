---
layout: project
title: "Amielo"
description: "Amielo is a web app for creating personalized digital gift invitations. You set up the surprise online and the gift itself happens in real life."
---

# Amielo

Live at [amielo.app](https://amielo.app/)

Amielo is a web app for creating personalized digital gift invitations. You set up the surprise online and the gift itself happens in real life.

## How it works

1. The giver builds the invitation in six steps: a gift (a birthday, romantic or kids' breakfast, or movie night), customized menu, a photo or illustration, a written or recorded voice message and an animated theme, with a live preview all the way through.
2. Creating and previewing is free and there's no account. The giver pays once, when they send the gift.
3. The recipient opens a private link, picks what they'd like from the menu and gets a confirmation card with a calendar invite.
4. The giver gets a chef's board with the choices and a grouped grocery list.
5. The giver makes it happen.

## The idea

Amielo sits between a greeting card and a real-world gesture. The digital part sets up the surprise and makes it feel personal and the experience itself happens offline.

## Technical highlights

- **No accounts.** Private links are the credentials. Their secret tokens are stored only as hashes and showing or emailing a link creates a new one.
- **Privacy by design.** Every service runs in an EU region. Personal data is deleted on a schedule and right away on request. Photos are re-encoded on the server to strip their metadata. Recipient pages, children's pages and the chef's board load no analytics and no third-party scripts, not even external fonts.
- **A job runner for everything timed.** Every email and every scheduled step is a job with a dedupe key, picked up by a cron every minute. The handlers are idempotent and re-read the current state before acting.
- **Time zones done carefully.** Times are stored in UTC together with the IANA time zone they belong to. All date logic goes through one module built on Temporal.
- **Money in minor units.** Prices are set per market and tax is left entirely to the Merchant of Record.
- **The memory card is drawn on the server.** It is rendered with Satori and resvg, emoji included, without any network calls.
- **Validation at every boundary.** Zod checks server actions, webhooks, environment variables and content files.
- **Content as data.** Menu items, gifts and themes live in YAML, which is validated and compiled to TypeScript at build time.

## Built with

| Area      | Tools                                                           |
| --------- | --------------------------------------------------------------- |
| Framework | Next.js 16 (App Router), React 19, TypeScript in strict mode    |
| Styling   | Tailwind CSS 4                                                  |
| Data      | PostgreSQL on Neon (Frankfurt), Drizzle ORM                     |
| Files     | Cloudflare R2 (EU), images processed with sharp                 |
| Email     | Resend and React Email                                          |
| Payments  | Stripe Managed Payments as the Merchant of Record               |
| Hosting   | Vercel (Frankfurt) with Vercel Cron                             |
| Also      | Upstash rate limiting, Sentry (EU), Plausible analytics         |
| Tests     | Vitest, Playwright with axe accessibility checks, Lighthouse CI |

## Status

Built and live at amielo.app. Gift creation opens once payments are connected.

---

Back to [Anka Peganc](https://anka.crunchyapple.si/)
