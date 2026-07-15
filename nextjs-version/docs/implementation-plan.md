# EliteDev Implementation Plan

## Status
- Product: Elite Development Enterprise Logistics Platform
- Mode: Single-tenant now; multi-tenant-ready architecture
- Repository foundation: approved Shadcn dashboard/landing Next.js template
- Current phase: 0 — repository inspection
- Phase owner: GLM-5.2 coding agent with human approval gates
- Last updated: 2026-07-15

## Product objective
Build a secure bilingual Arabic/English web operating system for Elite Development’s Saudi 3PL business. The driver is the central operational entity, connected to vehicles, assignments, attendance, violations, expenses, advances, maintenance, payroll, documents, and performance.

## Delivery principles
- Database-first, audit-first, secure-by-default
- Visual parity with the approved template; business domain fully replaced
- Single tenant UX; `organization_id` and RLS on all tenant-owned data
- Supabase PostgreSQL is source of truth
- No mock persistence or client-authoritative financial/business state
- Financial, legal, HR, and access controls are server validated and auditable
- Arabic RTL and English LTR are built together, not retrofitted

## Phase 0 — inspect and baseline
### Goal
Understand the fork before production modifications.

### Permitted work
- Inspect package manager, package.json, source tree, routes, configuration, theme, UI primitives, existing sample content, and auth/data code.
- Create/update only documentation in `docs/`.
- Run safe read-only commands and existing lint/typecheck only if no setup changes are required.

### Prohibited work
- No production code changes
- No new dependencies
- No Supabase migration creation/application
- No secrets or environment changes
- No destructive commands

### Required output
- Existing architecture and relevant files
- Components/routes to preserve, replace, or remove
- Dependency gap analysis
- i18n/RTL, auth, database, security, and design risks
- Proposed exact Phase 1 file plan

### Acceptance criteria
- Repository facts are verified from files
- Documentation has been updated
- No production source changes

## Phase 1 — visual and localization foundation
### Scope
- Elite brand tokens, typography, dark/light support where compatible
- Arabic/English locale architecture and true RTL/LTR
- Dashboard shell: sidebar, header, mobile drawer, command palette boundary, profile menu
- Landing page and auth UI matching approved reference quality
- Replace visible generic demo copy with EliteDev logistics language

### Explicit exclusions
- No business CRUD persistence
- No public signup
- No production Supabase migrations unless Phase 2 is approved

### Acceptance criteria
- Landing, sign-in, forgot/reset password, accept-invite UI routes render
- Responsive at 375/768/1024/1440
- RTL correctly flips logical layout
- No generic template user-facing copy in implemented routes
- Lint/typecheck pass

## Phase 2 — authentication, authorization, and data foundation
### Scope
- Supabase client/server setup and environment documentation
- Auth middleware and protected routes
- organizations, memberships, profiles, roles, permissions, audit logs
- RLS policies, seed of Elite Development organization and base roles
- Invite architecture and authorization service boundary

### Approval gate
Show SQL migration plan, RLS policy intent, indexes, test plan, seed and rollback effect before applying migrations.

### Acceptance criteria
- Deny-by-default tenant isolation
- Base role/action model exists
- Critical auth/role flows have tests or documented SQL tests
- Service-role key stays server-only

## Phase 3 — drivers, fleet, assignments, dashboard
### Scope
- Drivers, driver documents, expiry alerts
- Vehicles, vehicle documents, expiry alerts
- Driver-vehicle assignment transaction/history/odometer validation
- Data-backed executive dashboard summaries

### Acceptance criteria
- Organization-safe CRUD with RLS and audit events
- One active assignment constraint by default
- Profile pages display related data intentionally
- Empty/loading/error/permission states exist

## Phase 4 — daily operations
### Scope
- Attendance and approved workflow
- Expenses, receipts, advances
- Maintenance and vehicle availability status
- Violations, evidence, warnings, controlled deduction eligibility
- Notifications/expiry alerts and permission-aware exports

### Acceptance criteria
- Unique attendance constraint
- Advances cannot deduct without approval
- Maintenance links safely to expenses
- No automatic financial deduction without configuration/approval

## Phase 5 — payroll
### Mandatory design gate
Before mutations: submit rule model, data schema, status transitions, decimal method, approval/reversal design, RLS/permissions, audit model, and test fixtures. Await human approval for ambiguous money rules.

### Scope after approval
- Periods, rule versions, runs, lines, ledger entries
- Server-side calculation services and deterministic unit tests
- Review/approve/pay/lock lifecycle
- Controlled corrections/reversals
- Bilingual one-A4 payslip with QR verification

### Acceptance criteria
- Approved/locked snapshots are immutable
- All values trace to ledger items and sources
- Calculations server-side and decimal-safe
- Permission and lifecycle tests pass

## Phase 6 — HR, documents, reports
### Scope
- Leave, contracts, HR documents, onboarding/offboarding boundaries
- Template selection, generation, versioned snapshots, signed storage
- Safe public verification
- CSV/Excel/PDF/print reports and document layouts

## Phase 7 — platforms, invoices, hardening
### Scope
- Platform management/performance records
- Invoice/payout tracking
- ZATCA readiness only unless verified requirements are approved
- E2E tests, observability hooks, performance/security review, deployment runbook

## Cross-phase validation checklist
- `pnpm lint`
- `pnpm typecheck` or repository equivalent
- Relevant unit/integration tests
- Manual LTR and RTL review
- Responsive review at required breakpoints
- Keyboard/focus/dialog review
- RLS/authorization review for changed entities
- No secrets in diff
- Migration review before apply
- Audit events for relevant critical mutations

## Initial environment contract
```env
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
RESEND_API_KEY=
RESEND_FROM_EMAIL=
SENTRY_DSN=
```

Never commit populated `.env.local`. Maintain `.env.example` with names only.
