# Submission

How to submit your TaskHive implementation for evaluation.

---

## Submission Method

1. **Deploy your app to Vercel** (or equivalent hosting). You must have a working live URL.
2. Push your code to a **private GitHub repository**
3. Invite both evaluators as collaborators:
   - **Andrew** — GitHub: `Drew-Source`
   - **Ibrahim** — GitHub: `ibbybuilds`
4. Email the following to both evaluators (email provided separately):
   - GitHub repo link
   - **Live deployment URL**

**Deadline:** 7 days from when you receive the test. Late submissions accepted with penalty.

---

## Deployment Requirements

Your app must be accessible at a public URL. We recommend:

### App Hosting
- **Vercel** (recommended) — `npm install -g vercel && vercel deploy`. Free tier is sufficient.
- Alternatives: Netlify, Railway, Render. Any hosting that serves your app publicly.

### Database Hosting
Vercel doesn't include a database. Use a free hosted PostgreSQL:
- **Neon** (recommended) — Free tier, one-click PostgreSQL, native Vercel integration
- **Supabase** — Free tier, PostgreSQL with extras
- Any hosted PostgreSQL that your deployed app can connect to

### Why Deployment Is Required

We evaluate against your live URL. This isn't a DevOps test — it's a "does your product actually work" test. Deploying a Next.js app to Vercel is one command. If your app only works on localhost, we can't fully evaluate it.

---

## Required Files

Your repository must include:

| File | Purpose |
|------|---------|
| `README.md` | Setup instructions that actually work (local dev + deployed URL) |
| `DECISIONS.md` | Architectural decisions with reasoning |
| `skills/` (3+ files) | Skill files for your implemented endpoints |
| `.env.example` | All environment variables with placeholder values |
| `package.json` | Dependencies and scripts |

---

## Pre-Submission Checklist

Before submitting, verify each item against your **live deployment URL**:

### Deployment
- [ ] Live URL is accessible (no auth wall on public pages)
- [ ] API endpoints respond at `<your-url>/api/v1/tasks`
- [ ] Database is connected and seeded (categories exist)

### Core Loop (test against your live URL)
- [ ] Can register a new user and see 500 credit bonus
- [ ] Can create a task with budget (task appears with status `open`)
- [ ] `curl GET <your-url>/api/v1/tasks` returns tasks with correct envelope
- [ ] `curl POST <your-url>/api/v1/tasks/:id/claims` creates a claim
- [ ] Can accept claim in web UI
- [ ] `curl POST <your-url>/api/v1/tasks/:id/deliverables` submits deliverable
- [ ] Can accept deliverable in web UI → credits flow correctly

### Files
- [ ] At least 3 Skill files exist and match actual API behavior
- [ ] `.env.example` exists with all required variables
- [ ] `DECISIONS.md` explains your key choices
- [ ] No secrets committed (API keys, passwords, .env files)

---

## Late Penalty

| Days Late | Penalty |
|-----------|---------|
| 1 day | -10% |
| 2 days | -20% |
| 3 days | -30% |
| 4+ days | Not accepted |

A submission scoring 90% that's 1 day late becomes 81%.

---

## Partial Submissions

We accept partial submissions. A working Tier 1 implementation with good Skill files will score higher than a broken Tier 3 attempt. Ship what works.

**Important:** Even partial submissions must be deployed. A half-finished app on a live URL beats a complete app that only runs on your machine.

---

## Questions During the Test

- **Clarification questions** are welcome — email the evaluators
- **We won't debug your code** — but we'll clarify requirements if they're ambiguous
- **Stack questions** — "Can I use X instead of Y?" → Yes, if you explain why in DECISIONS.md
- **Deployment questions** — If you've never deployed to Vercel, ask. We'd rather answer a deployment question than receive a localhost-only submission.
- **Need AI tooling?** — We can provide a Claude Code account. Email **andrea@blackcode.ch** with subject `TaskHive Test — Claude Code Access Request — [Your Full Name]`. See `README.md` for details.

---

## What Happens After

1. We test your live deployment within 48 hours of submission
2. We follow the evaluation sequence in `EVALUATION.md` against your live URL
3. We review your source code, Skill files, and documentation
4. You receive a score breakdown within 5 business days
5. Top candidates are invited to further rounds with additional compensation
6. Top candidates are invited to a technical discussion
