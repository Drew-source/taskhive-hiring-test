# Submission

How to submit your TaskHive implementation for evaluation.

---

## Submission Method

1. Push your code to a **private GitHub repository**
2. Invite both evaluators as collaborators:
   - **Andrew** — GitHub: `Drew-Source`
   - **Ibrahim** — GitHub: `ibbybuilds`
3. Email the repo link to both evaluators (email provided separately)

**Deadline:** 7 days from when you receive the test. Late submissions accepted with penalty.

---

## Required Files

Your repository must include:

| File | Purpose |
|------|---------|
| `README.md` | Setup instructions that actually work |
| `DECISIONS.md` | Architectural decisions with reasoning |
| `skills/` (3+ files) | Skill files for your implemented endpoints |
| `.env.example` | All environment variables with placeholder values |
| `package.json` | Dependencies and scripts |

---

## Pre-Submission Checklist

Before submitting, verify each item:

- [ ] `git clone` + follow your README → application runs
- [ ] Can register a new user and see 500 credit bonus
- [ ] Can create a task with budget (task appears with status `open`)
- [ ] `curl GET /api/v1/tasks` returns tasks with correct envelope
- [ ] `curl POST /api/v1/tasks/:id/claims` creates a claim
- [ ] Can accept claim in web UI
- [ ] `curl POST /api/v1/tasks/:id/deliverables` submits deliverable
- [ ] Can accept deliverable in web UI → credits flow correctly
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

---

## Questions During the Test

- **Clarification questions** are welcome — email the evaluators
- **We won't debug your code** — but we'll clarify requirements if they're ambiguous
- **Stack questions** — "Can I use X instead of Y?" → Yes, if you explain why in DECISIONS.md

---

## What Happens After

1. We clone your repo within 48 hours of submission
2. We follow the evaluation sequence in `EVALUATION.md`
3. You receive a score breakdown within 5 business days
4. Top candidates are invited to further rounds with additional compensation
5. Top candidates are invited to a technical discussion
