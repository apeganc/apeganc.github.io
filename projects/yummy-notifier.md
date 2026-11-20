---
layout: project
title: "YUMMY – Favorite Dish Notifier"
description: "YUMMY watches restaurants' published weekly menus and emails you on the days one of your favorite dishes is on."
---

# YUMMY – Favorite Dish Notifier

Live at [yummy-notifier.crunchy.si](https://yummy-notifier.crunchy.si/)

YUMMY watches restaurants' published weekly menus and emails you on the days one of your favorite dishes is on. You pick a restaurant, give an email address and up to ten dishes, confirm the address once and then hear from YUMMY only when there's something you want for lunch.

## Why I built it

I eat lunch at the General Hospital Celje restaurant regularly and there are some dishes I really like. I didn't want to check the menu every morning, so I built something that tells me when a favorite is on the menu. It started with that one restaurant and grew into a small platform.

## What it does

- Monitors five locations so far: the canteen at General Hospital Celje, Restavracija Angel and Restavracija Chuty’s in Ljubljana, Aera in Vienna and the IKEA restaurants in Dubai/Abu Dhabi.
- Matches dishes in the language the restaurant publishes in: Slovenian, German, Croatian or English. The interface is in English and Slovenian.
- Sends an alert on the morning of the day or the evening before, plus an optional weekly summary. Every alert asks "Was this notification useful?" with one-click yes/no links.
- No accounts: double opt-in signup, a private management link and one-click unsubscribe.
- New restaurants are added on request. You email me the link at [anka@crunchy.si](mailto:anka@crunchy.si), I check that YUMMY can read the menu and it goes on the list.

## How it works

The pipeline is **fetch → parse → store → match → notify**. The notifier reads only from the database, so a broken scrape can never block an alert.

- **Fuzzy matching.** Dish names are normalized, lemmatized and fuzzy-matched, so Slovenian inflection, synonyms and compound words still match. A specificity gate stops a dish as common as _solata_ (salad) from triggering an alert every day. While you type, the form suggests real dishes from the menu archive and warns about entries that are too common.
- **Many menu formats.** HTML, PDF text, OCR of PDFs and images, JavaScript-rendered pages and menus forwarded by email. Parsers match text patterns rather than CSS classes, so a site redesign doesn't break them. A page that is clearly wrong is rejected. A page that is only unusual, like a holiday or a short week, is stored and flagged, so subscribers don't lose a week of alerts.
- **Scheduling.** A timer in the app wakes every half hour. It fetches the menus that are due and sends the alerts whose time has come, following each restaurant's time zone, opening days and public holidays. A Postgres advisory lock makes sure only one process ever sends.
- **At-least-once sending.** Each alert claims a row under a unique index before it goes out. If something crashes mid-send, the worst case is a duplicate email, which is better than a missed lunch.
- **Cheap to run.** Autocomplete, match previews and the status page read in-memory snapshots instead of the database, so the serverless Postgres can stay asleep. An unchanged menu page costs a single indexed query.
- **Monitoring.** A public status page shows alert health, source health and the matcher's precision and recall. Daily checks and dead-man's switches report when something stops working.

## Built with

| Area            | Tools                                                                   |
| --------------- | ----------------------------------------------------------------------- |
| Language        | Python 3.14, managed with uv                                            |
| Web             | FastAPI, Jinja2 templates, HTMX                                         |
| Data            | PostgreSQL on Neon, SQLAlchemy 2, Alembic migrations                    |
| Menus           | BeautifulSoup and lxml, pdfplumber, Tesseract OCR                       |
| Matching        | LemmaGen lemmatizer, RapidFuzz, Lingua language detection               |
| Email           | Brevo, every message with a plain-text version                          |
| Spam protection | Cloudflare Turnstile and rate limiting                                  |
| Hosting         | Docker container on an Oracle Cloud Always Free Arm VM, behind Caddy    |
| Backups         | Daily database dump to Cloudflare R2                                    |
| Quality         | pytest (over 1,500 tests), Ruff, mypy in strict mode, GitHub Actions CI |

Everything runs on free tiers, so it costs €0 a month.

---

Back to [Anka Peganc](https://anka.crunchyapple.si/)
