---
layout: project
title: "Clinical Studies Tracker"
description: "Every clinical study in one place: sponsor, protocol and study type, the sites each one recruits at and who is assigned to them."
---

# Clinical Studies Tracker (demo)

Live demo at [app-clinical.crunchy.si](https://app-clinical.crunchy.si/)

Every clinical study in one place: sponsor, protocol and study type, the sites each one recruits at and who is assigned to them.

> The live site is a demo. Anyone can read the sample studies and sign up and everything is reset to the sample data every night at 03:00 UTC. Nothing in it is real.

## The problem

Most clinical study systems are built by the sponsor, for the sponsor. Run studies for five sponsors and you end up with a portal for each one and a folder of spreadsheets holding it all together. I dealt with this myself while working in clinical trials.

## What it does

- Every study in one list, down to each recruiting site with its own status, enrollment and notes
- Filter by country, sponsor, protocol number, acronym or CRA (Clinical Research Associate) to see who is assigned to what
- Add, edit and delete studies as things change
- Every change is recorded: who made it, when and each field's value before and after
- Export to CSV

## Small on purpose

People in clinical trials already have to learn many systems, many of them with features they rarely use. This one does only what the job needs and it should be easy to use from the first minute.

It is a tracking and help tool, not a system of record. It holds study metadata only: no patient data and no confidential information.

## Technical highlights

- **Filters that can't trap you.** Each filter narrows the other four, so every value on offer has studies behind it. No filter narrows its own list, though, so a choice can always be widened again.
- **A strict Content-Security-Policy.** Enforced, with a per-request nonce and no `unsafe-inline` scripts.
- **Guards that fail closed.** The demo switches for public reads and open sign-up turn on only for the exact string `"true"`, so a typo closes a door rather than opening one. Without them, reads need an account and accounts are created by invitation.
- **An audit trail that can't be rewritten.** The data model itself refuses updates and deletes on audit entries, so append-only is enforced in code, not just promised.
- **A safe CSV export.** A note starting with `=` could turn into a live formula in Excel, so the export neutralizes it.
- **Rate limiting that works on serverless.** The counters live in MongoDB. Kept in a function's memory, they would reset with every new Vercel instance.
- **Tests with no test framework.** Node's built-in runner calls the real route handlers (a `Request` in, a `Response` out) against a throwaway MongoDB that the suite starts itself, so a test run can never touch a real database.

## Built with

| Area      | Tools                                                                   |
| --------- | ----------------------------------------------------------------------- |
| Framework | Next.js (App Router), React 19                                          |
| Styling   | Hand-written CSS with design tokens, IBM Plex Sans and Mono self-hosted |
| Data      | MongoDB Atlas via Mongoose                                              |
| Auth      | Better Auth: email and password, sessions in the same database          |
| Email     | Resend                                                                  |
| Hosting   | Vercel                                                                  |
| Tests     | `node --test`, ESLint                                                   |

---

Back to [Anka Peganc](https://anka.crunchyapple.si/)
