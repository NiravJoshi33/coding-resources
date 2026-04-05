AI-Assisted Software Project Starter Kit
A living reference for humans and coding agents. Feed this doc + your PRD to any AI coding agent to establish shared context, constraints, and quality bars before writing a single line of code.

How to Use This Kit
You fill in the bracketed placeholders.

AI uses every section below as a hard constraint, not a suggestion.

Keep docs in /docs in your repo. Update them as the project evolves — never let code drift from the docs.

text
/docs
  PRD.md              ← Product requirements (what & why)
  TECH_PLAN.md        ← Architecture & sequencing (how)
  ARCHITECTURE.md     ← Module map, boundaries, data flow
  AI_USAGE.md         ← Agent contract (rules & guardrails)
  ACCEPTANCE.md       ← Testable criteria per requirement
  DECISIONS.md        ← ADR log (Architecture Decision Records)

1. PRD.md — Product Requirements Document
Defines what the product does and for whom. This is the north star. Do not implement anything not in scope.

text
# PRD: [Project Name]

## Overview
- **What**: One-line description of the product.
- **Why**: Problem it solves. User pain point.
- **Who**: Primary user persona(s).
- **Success metric**: How you know it's working.

## Goals
- [ ] Goal 1 — measurable outcome
- [ ] Goal 2

## Non-Goals (Out of Scope)
- Feature A will NOT be built in v1.
- No support for [platform/use-case].

## User Stories
| ID  | Story                                         | Priority |
|-----|-----------------------------------------------|----------|
| U01 | As a user, I want to [X] so that [Y].         | P0       |
| U02 | As an admin, I want to [X] so that [Y].       | P1       |

## Functional Requirements
| ID  | Requirement                                               | Priority |
|-----|-----------------------------------------------------------|----------|
| F01 | System shall authenticate users via [OAuth/JWT/etc].      | P0       |
| F02 | User shall be able to [action] under [condition].         | P0       |

> Rule for AI: Every requirement must be unambiguous and independently testable.
> No vague words like "nice", "fast", "secure" without quantified constraints.

## Non-Functional Requirements
| Category     | Requirement                                              |
|--------------|----------------------------------------------------------|
| Performance  | P95 API response < 200ms under 500 concurrent users      |
| Security     | All endpoints require auth except [list].                |
| Scalability  | Must handle [X] req/sec without config change.           |
| Availability | 99.5% uptime. Graceful degradation on DB timeout.        |

## Edge Cases & Constraints
- What happens when [external service] is down?
- Maximum allowed [input size / file size / payload]?
- Regulatory constraints? (GDPR, HIPAA, etc.)

## Milestones
| Phase | Scope                        | Target  |
|-------|------------------------------|---------|
| v0.1  | Core [feature] working E2E   | [date]  |
| v0.2  | Auth + error handling        | [date]  |
| v1.0  | Production-ready             | [date]  |
2. TECH_PLAN.md — Technical Plan
Translates PRD into an engineering blueprint. Coding agents use this as primary reference for architecture decisions.

text
# Technical Plan: [Project Name]

## Stack
| Layer      | Choice                  | Reason         |
|------------|-------------------------|----------------|
| Frontend   | [Next.js / React]       | [reason]       |
| Backend    | [Hono / Express]        | [reason]       |
| DB         | [Postgres / Supabase]   | [reason]       |
| Auth       | [Supabase Auth / Clerk] | [reason]       |
| Deployment | [Vercel / Fly.io]       | [reason]       |

## Module Breakdown
| Module     | Owns                          | Does NOT own          |
|------------|-------------------------------|-----------------------|
| Auth       | session, JWT, role checks     | user profile data     |
| [Module 2] | [responsibilities]            | [boundaries]          |

## Data Models
AI must not invent new tables/fields not listed here.

```ts
// User
{
  id: uuid
  email: string (unique)
  role: "admin" | "user"
  created_at: timestamp
}
```

## API Design
| Method | Endpoint        | Auth? | Description                  |
|--------|-----------------|-------|------------------------------|
| POST   | /api/auth/login | No    | Exchange credentials for JWT |
| GET    | /api/[resource] | Yes   | Fetch [resource] for user    |

## Implementation Phases
Agents work phase-by-phase, not all at once.

### Phase 1 — Core Flow
- [ ] F01: [requirement]
- [ ] F02: [requirement]

### Phase 2 — Auth & Guards
- [ ] F03: [requirement]

### Phase 3 — Production Hardening
- [ ] Error handling on all async paths
- [ ] Logging + observability (structured logs)
- [ ] Rate limiting on [routes]
- [ ] Input validation on all endpoints
- [ ] Migration scripts for schema changes
3. ARCHITECTURE.md — Module Map
One-page description of how the system is assembled. Prevents AI from crossing module boundaries or coupling things that should be separate.

text
# Architecture: [Project Name]

## High-Level Diagram
Frontend (Next.js) → API layer (Hono) → Services → DB (Supabase)
                                     ↘ External APIs (Stripe, etc.)

## Module Responsibilities
Each module has a single clear responsibility. No module should
import from another module's internals.

- **/lib/auth** — session validation, role enforcement
- **/lib/db** — all DB queries; no raw SQL outside this module
- **/api/[route]** — HTTP handling only; delegates to lib/
- **/components** — UI only; no business logic
- **/hooks** — client state + data fetching only

## Data Flow
1. Request hits /api/[route]
2. Auth middleware validates JWT → attaches user to context
3. Route handler validates input → calls /lib/ function
4. /lib/ queries DB or external service → returns typed result
5. Route handler formats response → returns JSON

## Key Constraints
- No direct DB access from components or hooks.
- No business logic in route handlers; only in /lib/.
- All external service calls go through /lib/adapters/.
- Secrets only via environment variables; never hardcoded.
4. AI_USAGE.md — Agent Contract
Hard rules for any AI coding agent. Include this file every time you invoke Claude, Cursor, Copilot, or any agentic tool.

text
# Agent Contract: [Project Name]

## Ground Rules
1. Only implement what is listed in PRD.md. Do not add unrequested features.
2. Follow the stack and module structure in ARCHITECTURE.md exactly.
3. If a requirement is ambiguous, ASK before implementing. Do not guess.
4. Never invent new DB tables, routes, or env variables without stating so.
5. Prefer small, focused changes. One PR = one concern.

## Code Standards
- Language: TypeScript (strict mode). No `any`.
- Naming: camelCase for variables/functions, PascalCase for components/types.
- Error handling: all async functions must handle errors explicitly.
- Logging: structured logs (JSON) with request IDs. No console.log in prod.

## Security Rules
- All routes require auth unless explicitly listed as public in TECH_PLAN.md.
- Validate and sanitize all user inputs at the API boundary.
- Never log sensitive data (passwords, tokens, PII).
- Parameterized queries only. No string concatenation in DB queries.

## Testing Requirements
- Every new function or route must have at least one test.
- Tests live in __tests__/ mirroring the source structure.
- Do not modify existing passing tests unless explicitly asked.

## What AI Must NOT Do
- Do not refactor code outside files relevant to the current task.
- Do not add dependencies without stating reason and asking approval.
- Do not remove error handling, logging, or auth guards as "cleanup".
- Do not use deprecated APIs from any library in the stack.
5. ACCEPTANCE.md — Testable Criteria
Converts each PRD requirement into concrete pass/fail criteria. Give this to AI to generate test stubs.

text
# Acceptance Criteria: [Project Name]

## How to Use
- Each row maps to a requirement ID in PRD.md.
- "Done" means ALL criteria for that requirement are passing.
- AI agents: generate tests from this file. Do not mark a requirement
  complete unless all criteria pass.

| Req ID | Criteria                                      | Test Type   | Pass Condition                         |
|--------|-----------------------------------------------|-------------|----------------------------------------|
| F01    | User logs in with valid credentials           | Integration | Returns 200 + valid JWT                |
| F01    | Login fails with wrong password               | Integration | Returns 401 with error message         |
| F01    | Login blocked after 5 failed attempts         | Integration | Returns 429 on 5th attempt             |
| F02    | [Action] succeeds under normal conditions     | Unit        | Returns expected output for valid input|
| F02    | [Action] fails gracefully on [edge case]      | Unit        | Returns typed error, no 500            |
| NFR-P  | API P95 latency < 200ms @ 500 concurrent      | Load test   | k6 report shows < 200ms P95           |
| NFR-S  | Protected routes reject unauthenticated reqs  | Integration | Returns 401 for missing/invalid JWT    |
6. DECISIONS.md — Architecture Decision Records
Logs why key decisions were made. Prevents AI from "helpfully" undoing deliberate choices.

text
# Architecture Decisions

## ADR-001: [Decision Title]
- **Date**: YYYY-MM-DD
- **Status**: Accepted
- **Context**: Why did this decision need to be made?
- **Decision**: What was chosen?
- **Consequences**: What trade-offs does this create?
- **Alternatives considered**: What else was evaluated and why rejected?

## ADR-002: [Next Decision]
...
Workflow: Using These Docs with AI
Starting a new project
text
1. Fill in PRD.md (discuss + iterate with AI)
2. Derive TECH_PLAN.md from the PRD (AI-assisted, you review)
3. Write ARCHITECTURE.md module map
4. Set up AI_USAGE.md with your rules
5. Generate ACCEPTANCE.md criteria from PRD requirements
6. Start coding: feed [PRD slice + TECH_PLAN phase +
   AI_USAGE.md + relevant source files] to agent

Every time you invoke a coding agent, include:
The relevant PRD requirement IDs you're working on

The TECH_PLAN phase

The full AI_USAGE.md

ACCEPTANCE.md rows for those requirements

Relevant existing source files (don't rely on agent to infer structure)

"Is It Actually Done?" Checklist
Code Quality
No TypeScript errors in strict mode

No lint errors

All new functions have explicit return types

No console.log in production paths

No hardcoded secrets

Security
Auth guard on every protected route

Input validated at API boundary

No sensitive data in logs

Dependencies scanned (no critical CVEs)

Reliability
All async paths have error handling

External service calls have timeouts and fallbacks

DB queries are parameterized

Migrations are idempotent and reversible

Testing
Unit tests for core logic

Integration tests for API routes

All ACCEPTANCE.md criteria passing

No existing tests broken

Observability
Structured logs with request ID

Health check endpoint returns sensible status

Errors logged with enough context to debug

Keep this kit alive. Update the PRD when requirements change. Update DECISIONS.md when you make architectural trade-offs. Update AI_USAGE.md when you discover a pattern that works or reliably breaks things.
