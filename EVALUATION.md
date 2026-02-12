# Evaluation

This document explains exactly how your submission will be scored. No hidden criteria — everything is transparent.

**Evaluators:** Andrew & Ibrahim

---

## Scoring Breakdown

| Category | Weight | What We Evaluate |
|----------|--------|------------------|
| Core Loop Works | 30% | End-to-end: post → claim → deliver → accept. Credits flow correctly. |
| Agent API Quality | 25% | Speed, error messages, bulk ops, pagination, idempotency |
| Trinity Architecture | 20% | Skill files exist, are accurate, are micro-verbose, match implementation |
| Code Quality | 15% | TypeScript strictness, organization, naming, separation of concerns |
| Documentation | 10% | Setup instructions work, DECISIONS.md explains choices |

**Total: 100 points**

---

## How We Test

We follow this exact sequence when evaluating your submission:

### 1. Clone and Setup (Documentation — 10%)

```bash
git clone <your-repo>
cd <your-repo>
# Follow YOUR setup instructions exactly
```

- Does `npm install` (or equivalent) succeed?
- Does the database setup work as documented?
- Does `npm run dev` (or equivalent) start the application?
- Is there a `.env.example` with all required variables?
- Does `DECISIONS.md` exist and explain architectural choices?

**If setup fails, we stop here. Broken setup = instant DQ.**

### 2. Core Loop Test (Core Loop — 30%)

We test the full lifecycle manually:

1. Register a new user → verify 500 credit bonus
2. Create a task (budget: 200 credits) → verify task created with status `open`
3. Register an agent → get API key
4. `curl GET /api/v1/tasks` → verify task appears
5. `curl POST /api/v1/tasks/:id/claims` → verify claim created
6. Accept claim in web UI → verify task status updates
7. `curl POST /api/v1/tasks/:id/deliverables` → verify deliverable created
8. Accept deliverable in web UI → verify:
   - Task status → completed
   - Agent operator receives reputation credits (budget minus 10% fee)
   - Ledger entry exists with correct `balance_after`

### 3. Agent API Deep Dive (Agent API Quality — 25%)

We test the API thoroughly:

- **Error quality:** Hit every error case. Are suggestions actionable?
- **Status codes:** Correct HTTP codes for each scenario?
- **Response envelope:** Consistent `ok`/`data`/`error`/`meta` shape?
- **Pagination:** Cursor-based? Deterministic? No duplicates across pages?
- **Rate limiting:** Does 429 fire at 100+ req/min? Are headers present?
- **Bulk operations:** Does `POST /tasks/bulk/claims` work? Partial success?
- **Idempotency:** Does `Idempotency-Key` header prevent duplicates?
- **Integer IDs:** Are all API IDs integers?
- **Edge cases:** Claim already-claimed task? Deliver without claiming? Empty content?

### 4. Trinity Audit (Trinity Architecture — 20%)

We read your Skill files, then verify against the API:

- Do at least 3 Skill files exist?
- Does each Skill file match the quality of `examples/skill-example.md`?
- For each Skill file, we curl the endpoint and compare:
  - Do parameters match what the Skill describes?
  - Does the response shape match?
  - Do error codes and suggestions match?
  - Is the latency within the stated target?
- **Binding violations = point deductions.** A Skill file that doesn't match the API is worse than no Skill file.

### 5. Code Review (Code Quality — 15%)

We read your source code:

- **TypeScript:** Is `strict: true` in tsconfig? No `any` types?
- **Organization:** Logical file structure? Related code grouped together?
- **Naming:** Consistent conventions? Self-documenting names?
- **Separation of concerns:** Business logic separated from route handlers? Database access abstracted?
- **Validation:** Input validation on all endpoints? Zod or equivalent?
- **Error handling:** Consistent error creation? No swallowed errors?
- **Security:** API keys properly hashed? No sensitive data in responses? SQL injection protection?

---

## Instant Disqualification

Any of these result in automatic failure (0 points):

- **Plagiarism** — Copying another submission or significant portions of existing open-source projects without attribution. Using AI tools (Claude Code, Cursor, Copilot) is expected and encouraged — copying human-written code is not.
- **Setup doesn't work** — If we can't run the application after following your instructions, we can't evaluate it.
- **No core loop** — If the basic post → claim → deliver → accept flow doesn't work at all (partial implementation with some steps working is acceptable).
- **No Skill files** — Zero Skill files means you didn't engage with the Trinity Architecture, which is fundamental to the test.
- **No API key auth** — Agent API must use API key authentication. Session-only auth means no agent API.
- **Deception** — Skill files that describe functionality that doesn't exist, fake test results, etc.

---

## Bonus Points (up to +10)

These can push your score above 100:

| Bonus | Points | Criteria |
|-------|--------|----------|
| Working demo bot | +3 | A script that runs the full agent lifecycle end-to-end |
| Creative ID mapping | +2 | Interesting solution to the UUID ↔ integer bridge |
| Exceptional error messages | +2 | Error messages that teach agents how the system works |
| Good git history | +1 | Atomic commits, meaningful messages, logical progression |
| Comprehensive Skill files | +2 | Skill file for every endpoint, not just the minimum 3 |
| Closed loop | +5 | If an agent can verify the spec is met without a human in the loop, you've understood something important |

---

## Compensation

All candidates receive **$20** for completing the first round (valid submission). Top candidates are invited to further rounds with additional compensation.

- Each submission gets a unique score
- Scoring is deterministic — same submission = same score
- We provide score breakdowns per category

---

## What We're Really Looking For

Beyond the rubric, here's what separates great submissions:

1. **Agent empathy** — Did you think about what it's like to be an agent using your API? Are error messages helpful? Is discovery intuitive?

2. **Architectural reasoning** — Does your `DECISIONS.md` show thoughtful trade-offs, or did you just pick whatever was easiest?

3. **Binding discipline** — Do your Skill files actually match your API? Did you update them when the implementation changed?

4. **Credit correctness** — Credits are reputation points, but the ledger must be consistent. Transactions are atomic, `balance_after` snapshots are accurate.

5. **Signal over noise** — We'd rather see 5 well-implemented endpoints with great Skill files than 15 endpoints with sloppy error handling and no Skill files.
