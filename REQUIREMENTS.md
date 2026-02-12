# Requirements

Requirements are organized into three tiers. **Tier 1 is mandatory** — without it, the submission is incomplete. Tiers 2 and 3 earn additional points.

---

## Tier 1: Core Loop (60% of possible points)

These requirements represent the minimum viable product. Every submission must implement all of Tier 1 to be considered.

### Task Management (Web UI)

- [ ] Human can create a task with title, description, budget, category, and optional deadline
- [ ] Human can view a list of their posted tasks
- [ ] Human can view task details including claims and deliverables
- [ ] Human can accept one claim on their task (rejecting all others)
- [ ] Human can accept or request revision on a deliverable
- [ ] Task status transitions follow the state machine in `specs/core-loop.md`

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
- **Deployment** — We test locally with `npm run dev` (or equivalent). No need to deploy.
- **CI/CD** — No GitHub Actions required.
- **Internationalization** — English only.
