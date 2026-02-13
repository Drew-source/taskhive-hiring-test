# TaskHive Hiring Test

**Build a freelancer marketplace where AI agents are the workers.**

You have 7 days to build TaskHive — a web platform where humans post tasks and AI agents browse, claim, and deliver work for credits. The architecture is prescribed: the **Trinity Architecture** (Skill, Tools, Software). The implementation is yours.

---

## About the Role

This test reflects the real work you'll do. Our stack includes:

- **Meta** — React Native / Expo mobile application
- **AIOS Desktop** — Electron desktop app
- **AIOS Backend** — Python / LangGraph / Aegra agent infrastructure

TaskHive tests your ability to build agent-consumable software — the same skill you'll use daily. The **Bonus Tier** (Reviewer Agent) reflects real work on our agent infrastructure: building a LangGraph-powered bot that automatically evaluates deliverables submitted on the platform. See `REQUIREMENTS.md` for details.

---

## The Core Loop

```
┌──────────┐       ┌──────────┐       ┌──────────┐       ┌──────────┐       ┌──────────┐
│  Someone │──────→│  Agent   │──────→│  Agent   │──────→│  Agent   │──────→│  Poster  │
│  posts   │       │  browses │       │  claims  │       │ delivers │       │ accepts  │
│  task    │       │  tasks   │       │  task    │       │  work    │       │  work    │
│(UI / API)│       │  (API)   │       │  (API)   │       │  (API)   │       │(UI / API)│
└──────────┘       └──────────┘       └──────────┘       └──────────┘       └──────────┘
     │                                                                            │
     ▼                                                                            ▼
  Task with                                                                Agent operator
  budget                                                                   earns reputation
  (promise)                                                                credits. Payment
                                                                           happens off-platform.
```

Humans post tasks through the web UI. Agents can also post tasks via the API — an agent that needs help from other agents is the vision, not an edge case.

That's it. Make this work end-to-end, and you've passed Tier 1.

---

## Rules

1. **Use an agentic tool.** Claude Code, Cursor IDE, Claude Cowork, OpenClaw, Companion, or any agentic desktop/IDE of your choice. This test evaluates how effectively you build with AI assistance, not how fast you type.

2. **7-day deadline.** Late submissions accepted with penalty (see `SUBMISSION.md`).

3. **Solo work.** You may use any AI tools, libraries, or frameworks. You may not collaborate with other humans on the implementation.

4. **Recommended stack:** Next.js + PostgreSQL + TypeScript + Drizzle ORM. You can deviate — explain why in `DECISIONS.md`.

5. **No starter code.** You scaffold from scratch. How you structure the initial project is part of the evaluation.

6. **Deploy to Vercel.** Your submission must include a live URL. We test against the deployed version, not localhost. Free tier is fine — Vercel for the app, Neon or Supabase for PostgreSQL. See `SUBMISSION.md` for details.

---

## AI Tooling Access

We want you to have the best tools available. If you don't have access to an agentic coding tool, we can provide you with a **Claude Code account** for the duration of the test.

To request access, email **andrea@blackcode.ch** with the subject line:

```
TaskHive Test — Claude Code Access Request — [Your Full Name]
```

Include in the body:
- Your full name
- Your GitHub username
- When you received the test (start date)

We'll set up access within 24 hours. This is optional — if you already have Cursor Pro, Claude Code, or another agentic tool, use what you're comfortable with.

---

## What We're Testing

| We ARE testing | We are NOT testing |
|----------------|--------------------|
| Agent-first API design | CSS artistry or pixel-perfection |
| Trinity Architecture understanding | Complex DevOps or CI/CD |
| Core loop correctness | Real payment integration |
| Error message quality | Mobile responsiveness |
| Architectural decision-making | Speed of typing |
| Skill file accuracy | Number of features |
| UI discoverability (can an agent navigate it?) | UI design trends or aesthetics |
| Deployment competence (it works live) | Infrastructure scaling |

---

## Reading Order

Read these documents in order. Each builds on the previous:

| # | File | What you'll learn |
|---|------|-------------------|
| 1 | `ARCHITECTURE.md` | The Trinity Architecture (Skill → Tools → Software) and why it exists |
| 2 | `specs/data-model.md` | All 9 entities, relationships, status enums, integer ID requirement |
| 3 | `specs/core-loop.md` | The 5-step lifecycle, state machine, what happens at each step |
| 4 | `specs/credit-system.md` | Reputation credits, promise model, ledger basics |
| 5 | `specs/auth-flows.md` | Human session auth + Agent API key auth, middleware routing |
| 6 | `API-CONTRACT.md` | Response envelope, endpoint table, 3 fully-specified endpoints |
| 7 | `examples/skill-example.md` | Gold-standard Skill file — match this quality |
| 8 | `examples/agent-session-example.md` | Full agent lifecycle with curl examples |
| 9 | `examples/error-examples.json` | Good vs bad error responses |
| 10 | `REQUIREMENTS.md` | Tiered requirements (must/should/nice) with checklists |
| 11 | `EVALUATION.md` | Exact scoring rubric and how we test |
| 12 | `SUBMISSION.md` | How to submit, checklist, late policy |

---

## Timeline Suggestion

This is a suggestion, not a requirement. Adjust to your workflow.

| Day | Focus |
|-----|-------|
| 1 | Read all docs. Scaffold project. Database schema (Neon/Supabase). Auth (human + agent). |
| 2 | Task CRUD (web UI). Agent registration. API key generation. |
| 3 | Agent API: browse tasks, claim tasks, deliver work. |
| 4 | Core loop end-to-end. Credit ledger. Reputation tracking. |
| 5 | Skill files. Error message polish. Cursor pagination. |
| 6 | Tier 2 features: bulk ops, rate limiting, agent profile endpoints. |
| 7 | Deploy to Vercel. Testing. DECISIONS.md. README. Pre-submission checklist. |

---

## Scoring Summary

| Category | Weight |
|----------|--------|
| Core Loop Works | 30% |
| Agent API Quality | 25% |
| Trinity Architecture | 20% |
| Code Quality | 15% |
| Documentation | 10% |

We test against your **live deployment URL** — not localhost. Bonus points available for building a Reviewer Agent that auto-evaluates deliverables (see `REQUIREMENTS.md`). Full rubric in `EVALUATION.md`.

All valid submissions receive **$20 compensation**. Top candidates are invited to further rounds.

---

## FAQ

**Can I use a different database?**
Yes. MySQL, MongoDB — your choice. Explain why in DECISIONS.md. PostgreSQL is recommended because it's what we know well, and Neon/Supabase give you free hosted PostgreSQL that works with Vercel out of the box. SQLite won't work on Vercel's serverless architecture.

**Can I use a different language?**
Yes, but TypeScript is strongly recommended. If you use Go, Rust, Python, etc., the code quality rubric adapts, but you lose the type-safety signal that TypeScript provides.

**Can I add extra features beyond the requirements?**
You can, but they won't earn extra points unless they're in the Tier 2/3 requirements list or the bonus criteria. Focus on doing the required things well rather than adding unrequested features.

**What if I can't finish everything?**
Submit what you have. A working Tier 1 with good Skill files beats a broken attempt at Tier 3. We evaluate what works, not what's attempted.

**Can I use an ORM other than Drizzle?**
Yes — Prisma, Kysely, TypeORM, raw SQL. Explain your choice in DECISIONS.md.

**What about testing (unit tests, E2E)?**
Not required but appreciated. The demo bot bonus (+3 points) is the closest thing to a testing requirement.

**Where do Skill files go?**
Create a `skills/` directory in your project root. One markdown file per endpoint.

**Do I really need to deploy?**
Yes. We test against your live URL, not localhost. Vercel's free tier handles this easily. If your app works locally but not deployed, that's a signal.

**What if Vercel cold starts cause slow responses?**
We account for reasonable cold start times during evaluation. Don't worry about serverless cold starts — they won't affect your score.

**What's the Reviewer Agent bonus?**
An optional bonus where you build a LangGraph-powered agent that automatically evaluates deliverables submitted on your platform. It's the highest-value bonus and reflects real work on our stack. See `REQUIREMENTS.md` for details.

---

## Getting Started

```bash
# 1. Read ARCHITECTURE.md first
# 2. Then scaffold your project:
mkdir taskhive && cd taskhive
npx create-next-app@latest . --typescript --tailwind --app --eslint
npm install drizzle-orm postgres
npm install -D drizzle-kit

# 3. Set up your database (free tier):
#    - Neon: https://neon.tech (recommended, one-click PostgreSQL)
#    - Supabase: https://supabase.com (PostgreSQL + extras)

# 4. Build locally, then deploy:
npm install -g vercel
vercel deploy

# 5. Submit your live URL + GitHub repo
```

Good luck. Build something an agent would love to use.
