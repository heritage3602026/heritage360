<!--
Sync Impact Report
Version change: template -> 1.0.0
Modified principles:
- Template principle 1 -> I. Spec-First Business Clarity
- Template principle 2 -> II. Independently Valuable User Journeys
- Template principle 3 -> III. Traceable Requirements and Decisions
- Template principle 4 -> IV. Security, Access, and Payment Integrity
- Template principle 5 -> V. Cross-Repo Handoff Discipline
Added sections:
- Spec Quality Standards
- Workflow and Change Control
Removed sections:
- None
Templates requiring updates:
- UPDATED: .specify/templates/plan-template.md
- UPDATED: .specify/templates/spec-template.md
- UPDATED: .specify/templates/tasks-template.md
- REVIEWED: .specify/templates/checklist-template.md
- REVIEWED: .specify/templates/commands/*.md (directory absent)
Runtime guidance reviewed:
- REVIEWED: README.md
- REVIEWED: CLAUDE.md
Follow-up TODOs:
- None
-->
# Heritage360 Spec Repository Constitution

## Core Principles

### I. Spec-First Business Clarity
Every feature MUST begin with a business-readable specification before planning,
tasking, or implementation starts in downstream repositories. Specs MUST describe
the user problem, scope boundaries, priority, measurable outcomes, and acceptance
scenarios without relying on implementation details. Rationale: this repository is
the shared source of truth between BA work and backend, mobile, and CMS delivery.

### II. Independently Valuable User Journeys
User stories MUST be prioritized and independently testable. A P1 story MUST define
the smallest valuable MVP slice, and later stories MUST not hide mandatory behavior
needed for that MVP to work. Cross-story dependencies MUST be explicit. Rationale:
teams need to plan, demo, and release increments without guessing which behavior is
essential.

### III. Traceable Requirements and Decisions
Functional requirements, acceptance scenarios, assumptions, clarifications, and
success criteria MUST be traceable to user stories or documented business decisions.
When a requirement is unclear, the spec MUST mark it with `NEEDS CLARIFICATION` or
record the chosen assumption. Deferred behavior MUST state what is deferred and why.
Rationale: requirement changes are expected, so decisions must remain auditable.

### IV. Security, Access, and Payment Integrity
Specs touching authentication, authorization, RBAC, ticket issuance, payments,
callbacks, personal data, or administrative access MUST include explicit abuse,
permission, and failure cases. Payment and ticket state MUST be derived from trusted
backend verification, not unverified client assertions. Rationale: Heritage360
features manage revenue, access rights, and user-owned tickets.

### V. Cross-Repo Handoff Discipline
Specs MUST identify the affected product surfaces and downstream repositories
where planning is expected, such as backend, mobile, CMS, or shared services.
Implementation plans and tasks belong in the target delivery repositories unless a
feature is explicitly scoped to this specs repository. Rationale: this repository
coordinates BA output while technical planning is owned by the responsible team.

## Spec Quality Standards

Feature specifications MUST use clear Vietnamese or English consistently within a
feature file. Each spec MUST include prioritized user stories, acceptance scenarios,
functional requirements, key entities when data is involved, measurable success
criteria, assumptions, and unresolved clarifications. Requirements MUST use
declarative language such as MUST, SHOULD, MAY, PHẢI, NÊN, or CÓ THỂ with a clear
meaning. Ambiguous phrases like "friendly", "fast", or "secure" MUST be replaced
with observable behavior or measurable criteria.

## Workflow and Change Control

The default BA workflow is `/speckit-specify`, `/speckit-clarify` when needed, then
`/speckit-git-commit`. Tech Leads run `/speckit-plan`, `/speckit-tasks`, and
implementation workflows in the appropriate delivery repositories.

Before a sprint or major delivery phase, the team SHOULD capture a baseline. When
requirements change after baseline, the updated spec MUST be committed and analyzed
for impact before downstream issues or tasks are changed. Material requirement
changes MUST preserve prior decisions in the spec through clarifications,
assumptions, or an impact note rather than silently rewriting scope.

## Governance

This constitution supersedes conflicting repository conventions for Spec Kit
artifacts. Amendments require a documented change to this file, a semantic version
bump, and a Sync Impact Report describing affected templates and follow-up work.

Versioning policy:
- MAJOR: Removes or redefines a core principle in a way that invalidates existing
  specs or workflows.
- MINOR: Adds a principle, mandatory section, or material governance requirement.
- PATCH: Clarifies wording, fixes errors, or makes non-semantic refinements.

Compliance review is required when creating or updating specs, plans, tasks, or
checklists. The Constitution Check in plans MUST verify scope clarity, story
independence, traceability, security/payment impact, and cross-repo handoff.
Reviews MUST flag unresolved placeholders, unexplained bracket tokens, and vague
requirements before downstream implementation begins.

**Version**: 1.0.0 | **Ratified**: 2026-05-19 | **Last Amended**: 2026-05-19
