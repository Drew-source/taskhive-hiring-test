# Requirements

Requirements are organized into three tiers. **Tier 1 is mandatory** — without it, the submission is incomplete. Tiers 2 and 3 earn additional points.

---

## Tier 1: Core Loop (60% of possible points)

These requirements represent the minimum viable product. Every submission must implement all of Tier 1 to be considered.

### Deployment

- [ ] Application is deployed to Vercel (or equivalent) with a public URL
- [ ] Database is hosted (Neon, Supabase, or equivalent — not localhost)
- [ ] API endpoints are accessible at `<your-url>/api/v1/*`
- [ ] Web UI is accessible and functional at the deployed URL

### Task Management (Web UI)

- [ ] Human can create a task with title, description, budget, category, and optional deadline
- [ ] Human can view a list of their posted tasks
- [ ] Human can view task details including claims and deliverables
- [ ] Human can accept one claim on their task (rejecting all others)
- [ ] Human can accept or request revision on a deliverable
- [ ] Task status transitions follow the state machine in `specs/core-loop.md`

### UI Discoverability

These aren't design requirements — they're clarity requirements. Your UI should be navigable by any user (human or AI agent with browser control):

- [ ] Registration / login flow is findable and completable
- [ ] "Create task" action is accessible from the dashboard
- [ ] Task details page shows claims and their status
- [ ] "Accept Claim" action is visible on a task with pending claims
- [ ] "Accept Deliverable" action is visible on a task with submitted work
- [ ] Credit balance is displayed somewhere in the authenticated UI
- [ ] Task status updates are visible after actions (e.g., status changes from "open" to "claimed")

You are free to design the UI however you want. These requirements are about clarity, not layout. Clear labels, visible actions, obvious state — that's what matters.

### Agent API

- [ ] `GET /api/v1/tasks` — Browse tasks with filtering and pagination
- [ ] `GET /api/v1/tasks/:id` — Get task details
- [ ] `POST /api/v1/tasks/:id/claims` — Claim a task
- [ ] `POST /api/v1/tasks/:id/deliverables` — Submit deliverable
- [ ] `GET /api/v1/agents/me` — Get authenticated agent's profile
- [ ] All endpoints use the standard response envelope (see `API-CONTRACT.md`)
- [ ] All endpoints require API key authentication
- [ ] All error responses include `code`, `message`, and `suggestion` fields
- [ ] API uses integer IDs (not UUIDs) for all entities

### Credit System

- [ ] New users receive 500 welcome credits
- [ ] New agent registration grants 100 bonus credits to the operator
- [ ] Agent operator earns credits (budget minus 10% fee) when deliverable is accepted
- [ ] Credit transactions are logged in an append-only ledger
- [ ] Each ledger entry includes `balance_after` snapshot
- [ ] No escrow — budget is a promise, payment happens off-platform

### Authentication

- [ ] Humans authenticate via session (email + password minimum)
- [ ] Agents authenticate via API key (Bearer token)
- [ ] API keys use `th_agent_` prefix + 64 hex chars
- [ ] API keys stored as SHA-256 hashes (never raw)
- [ ] Middleware routes to correct auth system based on path

### Data Model

- [ ] All entities from `specs/data-model.md` are present (Users, Agents, Tasks, TaskClaims, Deliverables, Reviews, CreditTransactions, Categories)
- [ ] Status enums match the specified values
- [ ] Foreign key relationships are enforced

---

## Tier 2: Agent Experience (25% of possible points)

These requirements improve the agent API experience and demonstrate understanding of agent-first design.

### Skill Files

- [ ] Minimum 3 Skill files, one per implemented endpoint
- [ ] Each Skill file matches the quality of `examples/skill-example.md`
- [ ] Skill files accurately describe the actual API behavior (binding rule)
- [ ] Each Skill file includes: tool, purpose, auth, parameters, response shape, error codes with suggestions, latency target, rate limit info, example request/response

### Bulk Operations

- [ ] `POST /api/v1/tasks/bulk/claims` — Claim multiple tasks in one request
- [ ] Partial success supported (individual failures don't fail the batch)
- [ ] Response includes per-item results and summary

### Error Quality

- [ ] Every error response includes an actionable `suggestion` field
- [ ] Error messages include specific context (e.g., "Task 42" not just "Task")
- [ ] Error suggestions reference specific endpoints (e.g., "Use GET /api/v1/tasks to browse")

### Pagination

- [ ] Cursor-based pagination on all list endpoints
- [ ] Cursors are opaque (Base64 encoded)
- [ ] Pagination is deterministic (no skipped/duplicated items)

### Rate Limiting

- [ ] 100 requests per minute per API key
- [ ] `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` headers on every response
- [ ] 429 response when limit exceeded, with retry timing in suggestion

### Agent Profile Endpoints

- [ ] `PATCH /api/v1/agents/me` — Update agent profile
- [ ] `GET /api/v1/agents/me/claims` — List agent's claims
- [ ] `GET /api/v1/agents/me/tasks` — List agent's active tasks
- [ ] `GET /api/v1/agents/me/credits` — Get operator's credit balance and recent transactions

### Idempotency

- [ ] Support `Idempotency-Key` header on POST endpoints
- [ ] Duplicate requests with same key return original response
- [ ] Keys expire after 24 hours

---

## Tier 3: Polish (15% of possible points)

These requirements are stretch goals. Implementing any of them demonstrates ambition and thoroughness.

### Webhooks

- [ ] `POST /api/v1/webhooks` — Register webhook URL with event subscriptions
- [ ] `GET /api/v1/webhooks` — List agent's webhooks
- [ ] `DELETE /api/v1/webhooks/:id` — Remove webhook
- [ ] Webhook payloads signed with HMAC
- [ ] Events: task.new_match, claim.accepted, claim.rejected, deliverable.accepted, deliverable.revision_requested

### Rollback Support

- [ ] Tasks can be rolled back from `claimed` to `open` (poster cancels after claim acceptance)
- [ ] Credits are correctly adjusted on rollback
- [ ] State machine transitions are validated (cannot skip states)

### Search

- [ ] `GET /api/v1/tasks/search` — Full-text search on task title and description
- [ ] Search results ranked by relevance

### Agent Visibility

- [ ] `GET /api/v1/agents/:id` — Public agent profile with stats
- [ ] Agent profiles show reputation score, tasks completed, average rating

### Real-Time Updates

- [ ] WebSocket or SSE for task status changes
- [ ] Agents can subscribe to task updates

### Demo Bot

- [ ] A working bot script that demonstrates the full lifecycle
- [ ] Bot authenticates, browses, claims, delivers, and verifies credits
- [ ] Can be run with a single command (e.g., `npm run demo-bot`)

### Reviews

- [ ] Poster can leave a review after accepting deliverable
- [ ] Reviews include rating (1-5), quality score, speed score, and comment
- [ ] Agent's reputation score updates based on reviews

---

## Explicit Non-Requirements

Do NOT implement these. They are out of scope and will not earn extra points:

- **Real payments or escrow** — No Stripe, no PayPal, no escrow. Credits are reputation points. Payment happens off-platform.
- **File uploads** — Deliverables are text/markdown only. No file attachments.
- **Email notifications** — No Resend, no SendGrid. UI notifications only.
- **Admin panel** — No admin dashboard. Admin actions (dispute resolution) are out of scope.
- **OAuth providers** — Email + password is sufficient for human auth. GitHub/Google OAuth is optional.
- **Mobile responsiveness** — Desktop-only web UI is fine.
- **CI/CD** — No GitHub Actions required.
- **Internationalization** — English only.
- **Complex infrastructure** — Vercel free tier + Neon/Supabase free tier is sufficient. No Kubernetes, no Docker in production, no load balancers.

---

## Bonus Tier: Reviewer Agent (+10 points)

This is the highest-value bonus. It is entirely optional but demonstrates you can work with our actual agent infrastructure (LangGraph / Python).

### What You Build

A **Reviewer Agent** — an AI-powered bot that lives inside your TaskHive platform. When a freelancer or their agent submits a deliverable on a task, this bot automatically evaluates the submission against the task requirements and returns a binary **PASS** or **FAIL** verdict with detailed feedback.

When the review result is **PASS**, the task is automatically marked as completed and credits flow. No manual acceptance step — the Reviewer Agent IS the acceptance mechanism for auto-reviewed tasks. Since payments happen off-platform anyway, there's no need for a human in the loop.

When the review result is **FAIL**, the freelancer gets the feedback and can resubmit.

### The Review Loop

```
┌──────────────┐     ┌──────────────────┐     ┌──────────────────┐
│  Agent       │────→│  Deliverable     │────→│  Reviewer Agent  │
│  submits     │     │  submitted       │     │  evaluates       │
│  deliverable │     │                  │     │  against task    │
└──────────────┘     └──────────────────┘     │  requirements    │
       ▲                                      └────────┬─────────┘
       │                                               │
       │                                        ┌──────┴──────┐
       │                                        │             │
       │                                     PASS           FAIL
       │                                        │             │
       │                                        ▼             ▼
       │                                ┌────────────┐ ┌────────────────┐
       │                                │ Task       │ │ Feedback       │
       │                                │ completed  │ │ posted,        │
       │                                │ Credits    │ │ freelancer     │
       │                                │ flow       │ │ can resubmit   │
       │                                └────────────┘ └───────┬────────┘
       │                                                       │
       └───────────────────────────────────────────────────────┘
                          resubmit loop
```

### Who Pays for the LLM

The Reviewer Agent needs an LLM to analyze submissions — and LLM calls cost money. Two parties can provide the API key:

**1. The task poster (buyer)** — Provides their key when creating a task. They want automated QA on submissions, so they pay for it. But they set a **review limit** (e.g., max 10 reviews) to cap their cost. After the limit is hit, the poster's key is no longer used.

**2. The freelancer (agent operator)** — Can provide their own key in two scenarios:
- **From the start** — The poster never provides a key, but the freelancer wants automated feedback on their own work before formally submitting. Think of it as a self-review: "let me check if this is good enough before I submit."
- **After poster's limit is hit** — The poster paid for 10 reviews, the freelancer used all 10. Now the freelancer can continue getting automated reviews by using their own key.

**If neither party provides a key**, there is no automated review. The task runs the normal flow (poster reviews manually).

### Review Cost Control

The poster sets a `max_reviews` limit per task. This caps how many automated reviews the poster will pay for.

```
Example: Poster sets max_reviews = 10

Submission #1  → reviewed with poster's key (1/10 used)
Submission #2  → reviewed with poster's key (2/10 used)
...
Submission #10 → reviewed with poster's key (10/10 used)
Submission #11 → poster's limit exhausted
                  → if freelancer has a key: reviewed with freelancer's key
                  → if no key available: falls back to manual review
```

The freelancer's own reviews (using their key) have no platform-imposed limit — they're paying for it themselves.

### Submission History: Track Everything

Every submission attempt must be tracked with full data. Data is the new oil — store everything.

For each submission attempt on a task, track:

| Field | Type | Notes |
|-------|------|-------|
| `attempt_number` | integer | 1, 2, 3... sequential per task per agent |
| `content` | text | The actual deliverable content |
| `submitted_at` | timestamp | When it was submitted |
| `review_result` | enum | `pass`, `fail`, `pending`, `skipped` (no key available) |
| `review_feedback` | text | LLM-generated feedback explaining the verdict |
| `review_scores` | jsonb | Structured scores (e.g., `{ "requirements_met": 8, "code_quality": 7, ... }`) |
| `reviewed_at` | timestamp | When the review completed |
| `review_key_source` | enum | `poster`, `freelancer`, `none` — who paid for this review |
| `llm_model_used` | string, nullable | Which model was used (e.g., `anthropic/claude-sonnet-4-20250514`) |

This data is visible to:
- **The poster** — sees all submission attempts, review history, and pass/fail trend
- **The freelancer** — sees their own submission history and feedback
- **Public** (optional) — attempt count on agent profiles as a transparency signal

**Important:** Submission count does NOT affect agent reputation. A complex task might legitimately need multiple feedback loops — that's the system working as designed. The review limit exists purely for cost control, not as a quality signal.

### The Review Verdict: Binary Pass/Fail

The Reviewer Agent's verdict is strictly **PASS** or **FAIL**. No percentages, no "close enough."

- **PASS** — All task requirements are fully met. Task is automatically completed. Credits flow to the agent operator.
- **FAIL** — Requirements are not fully met. Detailed feedback is provided. Freelancer can resubmit.

Even if 90% of the requirements are met, it's a FAIL. The task either meets the spec or it doesn't. This is intentionally strict — it forces clear task descriptions and clear deliverables.

### Supported LLM Providers

Implement at least one: **OpenRouter** (recommended — single key gives access to multiple models), OpenAI, or Anthropic.

**For testing/demo purposes:** The candidate can use their own API key via environment variable. The platform integration (poster/freelancer provides key) is what we evaluate.

### Technical Requirements

- [ ] Built with **LangGraph** (Python) — a proper graph with nodes, edges, and state
- [ ] The agent authenticates with your TaskHive API using an API key (it's a platform agent)
- [ ] The agent reads deliverables via the API
- [ ] The agent uses an LLM to analyze submission quality against task requirements
- [ ] Binary PASS/FAIL verdict — PASS auto-completes the task and triggers credit flow
- [ ] LLM API key provided by poster OR freelancer (both paths work)
- [ ] Poster can set `max_reviews` limit to cap their cost
- [ ] When poster's limit is exhausted, freelancer can continue with their own key
- [ ] Falls back to manual review if no key is available from either party
- [ ] Full submission history tracked (every attempt with content, verdict, feedback, scores, timestamps)
- [ ] The agent can be triggered when a deliverable is submitted (webhook, polling, or manual trigger)
- [ ] Can be run with a single command (e.g., `python reviewer_agent/run.py` or `npm run reviewer`)

### How the +10 Points Break Down

| Feature | Points |
|---------|--------|
| Agent exists, uses LangGraph, authenticates with TaskHive API | +2 |
| Binary PASS/FAIL review with structured feedback via LLM | +2 |
| Dual key support (poster key + freelancer key with limit/fallback) | +2 |
| Full submission history tracking (all attempts, scores, feedback, timestamps) | +1 |
| Agent uses browser control to navigate a submitted URL and verify it works | +1 |
| PASS auto-completes task + credits flow (closed loop) | +2 |

### Data Model Extension

Extend the Task entity with these optional fields:

| Field | Type | Notes |
|-------|------|-------|
| `auto_review_enabled` | boolean | Default false. Poster opts in to automated review. |
| `poster_llm_key_encrypted` | string, nullable | Poster's encrypted LLM API key. Never in API responses. |
| `poster_llm_provider` | string, nullable | `openrouter`, `openai`, `anthropic` |
| `poster_max_reviews` | integer, nullable | Max reviews poster will pay for. Null = unlimited. |
| `poster_reviews_used` | integer | Default 0. Counter of reviews charged to poster's key. |

Extend the Agent entity (or a per-claim setting):

| Field | Type | Notes |
|-------|------|-------|
| `freelancer_llm_key_encrypted` | string, nullable | Freelancer's encrypted LLM API key. |
| `freelancer_llm_provider` | string, nullable | `openrouter`, `openai`, `anthropic` |

Add a new **SubmissionAttempt** entity (or extend Deliverables) to track the full history per the tracking table above.

### Example Reviewer Agent Graph

```
read_task → fetch_deliverable → resolve_api_key → analyze_content → [optional: browse_url] → generate_verdict → post_review → [if PASS: complete_task]
                                      │
                          ┌───────────┴───────────┐
                          │                       │
                   poster key available?    freelancer key?
                   under limit?                   │
                          │                       │
                          ▼                       ▼
                    use poster key          use freelancer key
                                                  │
                                           no key available?
                                                  │
                                                  ▼
                                          skip (manual review)
```

Each node is a discrete step. The `resolve_api_key` node handles the priority logic: poster key first (if under limit) → freelancer key → no key (skip). Standard LangGraph pattern.

### What We're Looking For

This isn't about building a perfect code reviewer. It's about demonstrating:

- **LangGraph competence** — Can you design an agent graph with proper nodes and conditional routing?
- **API integration** — Your agent consumes your own TaskHive API. Does it handle errors?
- **Cost control design** — Does the dual-key system with limits actually work?
- **Data discipline** — Is every submission attempt tracked with full context?
- **Closed loop** — Does a PASS verdict actually complete the task and flow credits?

### Where It Goes

```
your-project/
├── reviewer-agent/          # or whatever you name it
│   ├── graph.py             # LangGraph graph definition
│   ├── nodes/               # Individual node implementations
│   ├── state.py             # Agent state definition
│   ├── requirements.txt     # Python dependencies
│   └── run.py               # Entry point
├── src/                     # Your Next.js app
├── skills/
└── ...
```

Include setup instructions in your `README.md` for running the reviewer agent.
