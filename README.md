# TaskHive Hiring Test

**Build a freelancer marketplace where AI agents are the workers.**

You have 7 days to build TaskHive — a web platform where humans post tasks and AI agents browse, claim, and deliver work for credits. The architecture is prescribed: the **Trinity Architecture** (Skill, Tools, Software). The implementation is yours.

---

## The Core Loop

```
┌──────────┐       ┌──────────┐       ┌──────────┐       ┌──────────┐       ┌──────────┐
│  Human   │──────→│  Agent   │──────→│  Agent   │──────→│  Agent   │──────→│  Human   │
│  posts   │       │  browses │       │  claims  │       │ delivers │       │ accepts  │
│  task    │       │  tasks   │       │  task    │       │  work    │       │  work    │
│ (Web UI) │       │  (API)   │       │  (API)   │       │  (API)   │       │ (Web UI) │
└──────────┘       └──────────┘       └──────────┘       └──────────┘       └──────────┘
     │                                                                            │
     ▼                                                                            ▼
  Task with                                                                Agent operator
  budget                                                                   earns reputation
  (promise)                                                                credits. Payment
                                                                           happens off-platform.
```

That's it. Make this work end-to-end, and you've passed Tier 1.

---

## Rules

1. **Use an agentic tool.** Claude Code, Cursor IDE, Claude Cowork, OpenClaw, Companion, or any agentic desktop/IDE of your choice. This test evaluates how effectively you build with AI assistance, not how fast you type.

2. **7-day deadline.** Late submissions accepted with penalty (see `SUBMISSION.md`).

3. **Solo work.** You may use any AI tools, libraries, or frameworks. You may not collaborate with other humans on the implementation.

4. **Recommended stack:** Next.js + PostgreSQL + TypeScript + Drizzle ORM. You can deviate — explain why in `DECISIONS.md`.

5. **No starter code.** You scaffold from scratch. How you structure the initial project is part of the evaluation.

---

## What We're Testing

| We ARE testing | We are NOT testing |
|----------------|--------------------|
| Agent-first API design | UI design or CSS skills |
| Trinity Architecture understanding | DevOps or deployment |
| Core loop correctness | Real payment integration |
| Error message quality | Mobile responsiveness |
| Architectural decision-making | Speed of typing |
| Skill file accuracy | Number of features |

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
| 1 | Read all docs. Scaffold project. Database schema. Auth (human + agent). |
| 2 | Task CRUD (web UI). Agent registration. API key generation. |
| 3 | Agent API: browse tasks, claim tasks, deliver work. |
| 4 | Core loop end-to-end. Credit ledger. Reputation tracking. |
| 5 | Skill files. Error message polish. Cursor pagination. |
| 6 | Tier 2 features: bulk ops, rate limiting, agent profile endpoints. |
| 7 | Testing. DECISIONS.md. README. Pre-submission checklist. |

---

## Scoring Summary

| Category | Weight |
|----------|--------|
| Core Loop Works | 30% |
| Agent API Quality | 25% |
| Trinity Architecture | 20% |
| Code Quality | 15% |
| Documentation | 10% |

All valid submissions receive **$20 compensation**. Top candidates are invited to further rounds. Full rubric in `EVALUATION.md`.

---

## FAQ

**Can I use a different database?**
Yes. SQLite, MySQL, MongoDB — your choice. Explain why in DECISIONS.md. PostgreSQL is recommended because it's what we know well and can evaluate quickly.

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

---

## Getting Started

```bash
# 1. Read ARCHITECTURE.md first
# 2. Then scaffold your project:
mkdir taskhive && cd taskhive
npx create-next-app@latest . --typescript --tailwind --app --eslint
npm install drizzle-orm postgres
npm install -D drizzle-kit

# 3. Set up your database and start building
```

Good luck. Build something an agent would love to use.
