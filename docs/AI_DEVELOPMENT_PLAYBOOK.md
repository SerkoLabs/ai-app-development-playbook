# AI Development Playbook

The lifecycle order is fixed. Model selection follows `docs/MODEL_ROUTING.md`.

## Stage 01 — IDEA
Goal: turn an idea into a falsifiable product concept. Identify primary user, problem, core value proposition, repeatable action, MVP constraints, risks and primary success signal.

Gate: PASS when the idea is specific enough to produce a product README without inventing the product.

## Stage 02 — README.md
Required: one-line promise, problem, target user, product concept, core loop, MVP, explicit non-goals, differentiation, success metrics, stack constraints if known, risks/assumptions, current status.

Gate: PASS when another competent engineer can explain what is being built, for whom, why and what is excluded.

## Stage 03 — PRODUCT_SPEC.md
For every MVP feature define user goal, trigger, preconditions, happy path, alternate paths, validation, permissions, loading, empty, error/retry, offline/degraded behavior when relevant, analytics where useful, acceptance criteria and out-of-scope behavior.

Gate: PASS when every MVP feature has testable acceptance criteria and no major behavior is left to implementation guesswork.

## Stage 04 — USER_FLOWS.md
Map first-run, auth/account, primary core loop, creation, consumption, settings/profile, permission-denied, network/error recovery, destructive actions, payments and moderation where applicable.

Every flow includes starting state, transitions, decision branches, terminal outcomes and recovery paths.

Gate: PASS when every MVP feature is reachable through at least one defined flow and every major screen/state exists for a reason.

## Stage 05 — ARCHITECTURE.md
Define chosen stack and rationale, app boundaries, directory/module structure, navigation, state, data fetching/cache, auth/session lifecycle, authorization boundary, server/API responsibilities, storage, background jobs, notifications, localization, analytics, error handling, observability, env/secrets, testing, CI/CD, security/privacy, deployment and performance assumptions.

For version-sensitive technology consult current official docs.

Gate: Sol produces the first draft; Astra performs the critical architecture gate review. PASS only when no unresolved architecture blocker or P0/P1 review finding remains.

## Stage 06 — DATABASE.md
For each table/collection specify fields/types, PK/FK, unique/check constraints, nullability/defaults, indexes, relationships, timestamps, soft-delete behavior, ownership and data classification. Also define roles, RLS/authorization, grants, storage policies, cascade behavior, retention/deletion, migration strategy and allow/deny security tests.

For Supabase, grants and RLS are both explicit and `service_role` remains server-side only.

Gate: Sol produces the first draft; Astra reviews authorization/data-boundary risk. PASS only when unauthorized read/write paths can be stated and tested and no unresolved P0/P1 remains.

## Stage 07 — IMPLEMENTATION_PLAN.md
Break approved design into dependency-ordered, small, testable tasks. Every task has ID, purpose, work, affected areas, dependencies, acceptance criteria, verification, complexity and risk notes. Identify the first real vertical slice explicitly.

Gate: PASS when the next task can be implemented without architectural invention.

Hard stop: no new application code before this gate unless the user explicitly asks for a throwaway prototype.

## Stage 08 — Foundation
Typical work: scaffold, package manager lockfile, strict typing, formatter/linter, tests, env example, configuration validation, CI baseline, logging/error baseline and secrets hygiene.

Gate: relevant install/lint/typecheck/test/build commands pass or are explicitly PARTIAL with a documented blocker.

## Stage 09 — App shell/navigation
Build navigation/layout/providers/base state and error/loading boundaries without pretending unfinished features work.

Gate: app starts, intended shell navigation works and startup/failure states do not crash the app.

## Stage 10 — First end-to-end vertical slice
Prove one real core journey using real UI, real data path, real auth/authorization where applicable, persistence/reload where applicable, success/failure handling and verification evidence.

Gate: a tester can complete the smallest meaningful product action end-to-end with no hidden critical mock.

## Stage 11 — Audit #1
Independent Astra review of requirements coverage, runtime correctness, architecture drift, auth/security, data/RLS, test gaps, UX state coverage, accessibility baseline and dependency health.

Gate: no unresolved P0/P1.

## Stage 12 — Core feature implementation
Implement remaining MVP features in dependency order. Security/testing/analytics are handled with their features, not deferred to the end.

Gate: all beta MVP acceptance criteria are implemented or explicitly deferred with approved rationale.

## Stage 13 — Audit #2
Repeat independent product-scope review, adding dead code/placeholders, cross-feature regressions, performance, localization, privacy, moderation, analytics integrity, offline/degraded behavior and production configuration.

Gate: no P0/P1; remaining P2 risk is understood.

## Stage 14 — Store/release readiness
For mobile, verify current Apple/Google rules, privacy/data disclosures, permissions, account deletion where applicable, UGC, digital payments, signing/build, crash/analytics and release smoke tests. For web/backend, verify production env/secrets, migrations, rollback, observability and deployment smoke tests.

Gate: release candidate is reproducible with no known release-blocking policy/security issue.

## Stage 15 — Beta readiness
Define beta audience, onboarding, measurable primary value action, feedback/report channel, crash/error visibility, rollback/hotfix path, seed/content/operations needs and success/failure thresholds.

Final gate: the product is not considered validated merely because it runs; beta must measure whether users complete the intended value behavior.

## Decision framework
When several approaches are viable, compare product fit, correctness/security, implementation complexity, operational complexity, reversibility, ecosystem support, cost and testability. Prefer the simplest reversible option that meets requirements.

## Definition of Done
A task is done only when acceptance criteria are met, relevant verification was executed, severe findings are resolved, no production-critical placeholder remains and docs/status reflect reality. "Code was written" is never the definition of done.
