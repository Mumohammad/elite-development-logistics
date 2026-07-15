# Architecture Decisions — EliteDev

This document records accepted technical decisions. Add a new numbered entry for every material decision. Do not overwrite history; supersede a decision explicitly when necessary.

## ADR-001 — Use the approved Next.js template as the visual foundation
- **Status:** Accepted
- **Decision:** Build from the repository’s `nextjs-version` application and retain compatible design primitives, page patterns, and responsive behaviors.
- **Reason:** It provides the approved landing, dashboard, and auth visual baseline while allowing the domain to be replaced.
- **Consequences:** Generic demo content and unrelated sample routes are removed/refactored incrementally; the template is not treated as a production domain model.

## ADR-002 — Single tenant now, multi-tenant ready by schema and RLS
- **Status:** Accepted
- **Decision:** Seed one Elite Development organization; include `organization_id` in every tenant-owned table and enforce organization membership/RLS from the first migration.
- **Reason:** Avoid a costly future redesign while keeping current UX simple.
- **Consequences:** No organization-switcher or billing is required initially. No hardcoded organization IDs in production business logic.

## ADR-003 — Supabase PostgreSQL is the source of truth
- **Status:** Accepted
- **Decision:** Use Supabase PostgreSQL, Auth, Storage, and RLS. UI state is transient only.
- **Reason:** The system requires durable relationships, auditability, access control, document storage, and future SaaS isolation.
- **Consequences:** No mock persistence, no localStorage business data, and no frontend-only critical workflows.

## ADR-004 — Server-authoritative mutation and finance model
- **Status:** Accepted
- **Decision:** Protected Server Actions/Route Handlers perform validation, authorization, financial calculation, state transition, persistence, and audit actions.
- **Reason:** Client code cannot be trusted for financial totals, approval states, permissions, or tenant scope.
- **Consequences:** Client forms revalidate on server; money uses PostgreSQL numeric or integer halalas; no JavaScript floating-point authority.

## ADR-005 — Arabic/English and RTL/LTR are core architecture
- **Status:** Accepted
- **Decision:** Implement localized routes or equivalent documented locale architecture, Cairo/Geist(or Inter), `lang`/`dir`, complete message catalogs, and logical CSS.
- **Reason:** Arabic is a primary Saudi operating language, not an afterthought.
- **Consequences:** Every new component must be direction-tested. No hardcoded left/right positioning where logical properties apply.

## ADR-006 — Authorization combines RBAC and RLS
- **Status:** Accepted
- **Decision:** Model roles, permissions, role permissions, user role assignments, organization membership, server permission checks, and database RLS.
- **Reason:** Hiding UI elements is not security; tenant and action isolation must survive direct requests.
- **Consequences:** Every module defines read/create/update/delete/approve/export/print/manage needs. RLS must deny by default.

## ADR-007 — Audit history is immutable in normal UI
- **Status:** Accepted
- **Decision:** Critical business actions write audit events; normal users cannot edit/delete audit records.
- **Reason:** Payroll, documents, permissions, and operational history require traceability.
- **Consequences:** Sensitive fields must be minimized/redacted in audit views; audit table policies are restrictive.

## ADR-008 — Payroll is ledger-first and snapshot-based
- **Status:** Accepted
- **Decision:** Payroll is modeled as periods, rule versions, runs, lines, itemized ledger entries, review/approval/payment/lock states, and immutable approved snapshots.
- **Reason:** Manual auditable operations are safer than opaque spreadsheet-only or client-only calculations.
- **Consequences:** Approved/paid/locked values cannot be edited directly; corrections use a controlled reversal/correction workflow. Exact salary/allowance/GOSI rules need business approval before implementation.

## ADR-009 — Documents are generated from versioned data snapshots
- **Status:** Accepted
- **Decision:** Generated official documents store their template version, input snapshot, document number, output reference, generation metadata, and verification identity.
- **Reason:** Historical documents must remain reproducible and auditable after source records change.
- **Consequences:** Public verification shows only safe authenticity metadata. Restricted source files use signed URLs.

## ADR-010 — ZATCA is readiness, not an unverified claim
- **Status:** Accepted
- **Decision:** Model invoice fields and secure integration boundaries; defer signing/submission until verified requirements, credentials, and approved workflow exist.
- **Reason:** UI alone does not create legal/tax compliance.
- **Consequences:** Do not market or label the product as ZATCA compliant until implemented and independently verified.

## ADR-011 — Design quality is operational, not ornamental
- **Status:** Accepted
- **Decision:** Preserve template quality with fixed Elite palette and sidebar gradient, data-dense tables, restrained motion, and readable surfaces.
- **Reason:** Logistics users need speed, clarity, and professional trust.
- **Consequences:** No neon/crypto patterns, excessive blur, decorative metrics, or global gradient buttons.

## ADR-012 — Print layouts are dedicated views
- **Status:** Accepted
- **Decision:** Reports and official documents use dedicated print CSS/PDF layouts; dashboard chrome never prints.
- **Reason:** A raw dashboard print is unsuitable for official documents and signatures.
- **Consequences:** Payslips target one A4 page and require print testing in both languages.

## Decision template
```md
## ADR-XXX — Title
- **Status:** Proposed | Accepted | Superseded | Rejected
- **Date:** YYYY-MM-DD
- **Context:**
- **Decision:**
- **Alternatives considered:**
- **Consequences:**
- **Approval required:** Yes/No; owner
```
