# AGENTS.md — AI App Development Operating System

This repository uses a deterministic, document-first software delivery lifecycle. This file is the canonical behavioral contract for AI coding agents.

## 0. Instruction priority

1. Explicit user instructions in the current task.
2. This `AGENTS.md`.
3. `docs/AI_DEVELOPMENT_PLAYBOOK.md`.
4. `docs/MODEL_ROUTING.md` for cost-aware model selection.
5. Project-specific approved documents in `docs/`.
6. Relevant `SKILL.md` files.
7. Existing implementation conventions inferred from the codebase.

If instructions conflict, follow the higher-priority source. Never silently resolve a material conflict: record it in `docs/PROJECT_STATUS.md`.

## 1. Core objective

Build the smallest coherent product that satisfies the approved product specification with production-grade engineering discipline.

Optimize for:
- correctness before breadth,
- one complete vertical slice before many partial features,
- explicit acceptance criteria,
- secure-by-default data access,
- measurable product behavior,
- small reviewable changes,
- current official documentation for version-sensitive decisions,
- minimal unnecessary complexity.

Do not optimize for:
- maximum feature count,
- speculative architecture,
- unrequested redesigns,
- impressive but unused abstractions,
- replacing working code merely because another approach is fashionable.

## 2. Mandatory lifecycle — do not reorder

The project lifecycle is fixed:

01. IDEA  
02. README.md  
03. docs/PRODUCT_SPEC.md  
04. docs/USER_FLOWS.md  
05. docs/ARCHITECTURE.md  
06. docs/DATABASE.md  
07. docs/IMPLEMENTATION_PLAN.md  
08. Implementation Phase 0 — repository/tooling foundation  
09. Implementation Phase 1 — app shell/navigation  
10. First end-to-end vertical slice  
11. Audit #1  
12. Core feature implementation  
13. Audit #2  
14. Store/release readiness  
15. Beta readiness

The ordering is invariant. A stage may be marked `N/A` only when it genuinely does not apply. Never skip a stage without recording the reason.

### Planning autonomy

Stages 01–07 may be completed sequentially in one planning session if:
- each stage passes its gate before the next begins,
- no material unresolved product decision requires the user,
- no application code is written.

After Stage 07, stop before implementation unless the user explicitly asked to implement/code/continue development.

### Coding autonomy

During implementation:
- work only inside the currently authorized implementation phase,
- complete its tasks and quality gate,
- update project status,
- do not silently jump multiple implementation phases in one task unless the user explicitly asked for autonomous multi-phase execution.

A simple user message such as `continue` is sufficient authorization to proceed to the next eligible phase. The user should not need to restate the workflow.

## 3. Phase detection

At the start of every task:

1. Read `AGENTS.md`.
2. Read `docs/AI_DEVELOPMENT_PLAYBOOK.md`.
3. Inspect `docs/PROJECT_STATUS.md` if present.
4. Inspect the repository and relevant approved docs.
5. Determine the earliest incomplete mandatory lifecycle stage.
6. Reconcile that with the user's explicit request.
7. Work only on the allowed stage/task.
8. Update `docs/PROJECT_STATUS.md` before finishing.

A README containing the marker `AI_PLAYBOOK_TEMPLATE` is a template README and does not count as the project's product README.

If `docs/PROJECT_STATUS.md` conflicts with the actual repository, the repository and approved artifacts win. Correct the status file and explain the reconciliation in its changelog.

## 4. Source-of-truth hierarchy

Use these files for these decisions:

- `README.md`: product promise, target user, MVP scope, non-goals.
- `docs/PRODUCT_SPEC.md`: detailed product behavior and acceptance criteria.
- `docs/USER_FLOWS.md`: navigation and user journeys.
- `docs/ARCHITECTURE.md`: system boundaries and technical design.
- `docs/DATABASE.md`: database/storage/auth/RLS model.
- `docs/IMPLEMENTATION_PLAN.md`: dependency-ordered executable tasks.
- `docs/DECISIONS.md`: material architecture/product decisions and rationale.
- `docs/PROJECT_STATUS.md`: current phase, gate state, blockers and next action.
- Code/tests/migrations: implementation truth after coding starts.

Do not introduce behavior that contradicts higher-level approved documents. If code and docs drift, determine which is intended, then synchronize both.

## 5. Research-before-decision rule

Use current primary/official sources when a decision depends on:
- framework/library versions,
- app-store rules,
- privacy or platform policies,
- security practices,
- SDK/API capabilities,
- payments,
- authentication,
- deployment,
- database behavior,
- operating-system behavior,
- legal/compliance requirements.

Record material findings and source links in the relevant project document. Do not browse for timeless decisions when it adds no value.

Prefer:
1. official vendor documentation,
2. standards bodies,
3. first-party repositories/changelogs,
4. reputable secondary sources only when necessary.

Never treat model memory as authoritative for version-sensitive facts.

## 6. Product discipline

Before code exists, the project must answer:
- Who is the primary user?
- What painful job/problem is solved?
- What is the core repeatable action?
- What is in MVP?
- What is explicitly outside MVP?
- What single behavior would demonstrate product value?
- What are the main failure/empty/loading/offline states?
- What data is public, private, sensitive, or derived?
- What metrics determine whether the beta is working?

Every feature must map to at least one approved user flow and acceptance criterion.

No feature may enter the implementation plan merely because it is "nice to have."

## 7. Engineering rules

### Always
- Inspect before modifying.
- Preserve working behavior unless change is required.
- Prefer the smallest complete change.
- Use strict typing when supported.
- Validate external input.
- Handle loading, empty, error and retry states where applicable.
- Keep secrets out of client code and git.
- Add or update tests for behavior changed.
- Run relevant lint/typecheck/tests/build before declaring completion.
- Report commands run and failures honestly.
- Keep docs synchronized with material behavior changes.
- Use migrations for database changes after database design is approved.
- Add observability for important failures and critical product events.

### Never
- Never claim tests passed if they were not run.
- Never invent environment variables, credentials, API keys or production data.
- Never expose service-role/admin secrets to mobile/web clients.
- Never weaken auth/RLS/security to make a test pass.
- Never use mock data in a production flow without an explicit, visible marker and removal task.
- Never perform unrelated refactors during a scoped feature task.
- Never rewrite the design system when the user asked to preserve it.
- Never add dependencies without a concrete need.
- Never use destructive database operations without an explicit migration/rollback strategy.
- Never silently change product scope.

### Ask/stop only for materially blocking decisions
Do not ask for routine implementation preferences that can be derived from approved docs. Stop and surface a decision only when proceeding would:
- create irreversible data loss,
- incur meaningful cost,
- change the product promise,
- require credentials/ownership only the user can provide,
- introduce a major vendor dependency,
- materially affect legal/privacy/payment behavior.

## 8. Vertical-slice rule

Before broad feature implementation, deliver one real end-to-end path.

A valid vertical slice:
- starts from a real user action,
- traverses real UI/state,
- reaches the real backend/database when the product requires one,
- enforces real auth/authorization,
- persists or retrieves real data,
- handles success and failure,
- has tests or reproducible verification,
- contains no hidden production-critical mocks.

Example shape:
`sign in → open feature → perform core action → persist → reload → observe persisted result`.

The vertical slice is a proof that the architecture works, not a demo with disconnected screens.

## 9. Quality gates

A lifecycle stage is complete only if its gate in `docs/AI_DEVELOPMENT_PLAYBOOK.md` passes.

For coding phases, the minimum evidence is:
- relevant tests pass,
- typecheck passes when applicable,
- lint passes when applicable,
- build/compile passes when applicable,
- no known P0/P1 defect in the changed scope,
- acceptance criteria verified,
- status/docs updated.

If a command cannot run, mark the gate `PARTIAL`, explain why, and do not report a full pass.

## 10. Audit protocol

Audits are read-first and evidence-driven.

Classify findings:
- `P0`: security/data-loss/app-unusable/release-blocking.
- `P1`: major core-flow correctness or authorization failure.
- `P2`: meaningful quality/UX/performance/maintainability issue.
- `P3`: polish or low-risk improvement.

Audit for:
- missing implementation,
- placeholders/TODOs,
- mock data,
- broken navigation,
- runtime risks,
- typing errors,
- auth/authorization,
- database grants/RLS,
- secret exposure,
- race conditions,
- loading/error/empty/offline states,
- accessibility,
- localization,
- privacy,
- analytics correctness,
- performance regressions,
- dependency/security issues,
- store-policy risks.

Fix P0 first, then P1. Do not bury severe findings under polish.

## 11. Mobile defaults

When the project is a mobile app:
- treat OWASP MASVS categories as the security baseline,
- verify secure local storage and token handling,
- verify deep links/universal links if present,
- verify permissions are minimal and justified,
- verify offline/network-loss behavior where material,
- test on real device/simulator targets relevant to release,
- include app-store privacy, account deletion, UGC, payments and permission requirements when applicable,
- do not assume store rules from memory; check current official policies before release.

When Expo/React Native/Supabase is used, load the `expo-supabase-mobile` skill.

## 12. Supabase defaults

If Supabase is used:
- design schema/RLS before migrations,
- enable RLS on every client-exposed table/view unless a documented reason says otherwise,
- explicitly design grants and policies,
- keep `service_role` server-side only,
- test allow and deny paths,
- define storage bucket privacy and object policies,
- record cascade and soft-delete behavior,
- index columns used by foreign keys, filters and policy predicates when appropriate,
- do not place privileged business logic in the client.

## 13. Model routing

Read `docs/MODEL_ROUTING.md` before selecting or escalating models. Use Sol for routine planning/work and reserve Astra for the explicit high-consequence gates defined there. Never claim a critical Astra review occurred unless the runtime actually used Astra.

## 14. Agents and skills

If the environment supports native custom agents, use the repository definitions in `.github/agents/` or `.claude/agents/`.

If native agents are unavailable, simulate the same separation of concerns sequentially:
1. Orchestrator determines scope.
2. Specialist performs the work.
3. Independent reviewer audits against acceptance criteria.
4. Orchestrator resolves findings and updates status.

Skills are stored in `.claude/skills/`. GitHub Copilot also supports this project skill location. When the current task matches a skill, read the relevant `SKILL.md` before acting.

Do not load every skill into context preemptively. Use progressive disclosure.

## 15. Implementation-plan rules

`docs/IMPLEMENTATION_PLAN.md` must:
- use dependency order,
- use small reviewable tasks,
- give each task an ID,
- specify purpose, work, affected areas, dependencies, acceptance criteria, verification and complexity,
- identify the first vertical slice,
- separate must-have beta work from post-beta work,
- include security/testing/store work inside the relevant feature phases rather than dumping all quality work at the end.

Default internal implementation phases:
- Phase 0 — Repository/tooling foundation
- Phase 1 — App shell/navigation
- Phase 2 — Authentication/user model
- Phase 3 — First vertical slice
- Phase 4 — Core product features
- Phase 5 — Social/community features (N/A when irrelevant)
- Phase 6 — Notifications/localization (N/A when irrelevant)
- Phase 7 — Moderation/security
- Phase 8 — Analytics/performance
- Phase 9 — Store/release readiness

These internal phases adapt to the product; the top-level 01–15 lifecycle does not reorder.

## 16. Completion report

Every implementation task should finish with a compact report:
- lifecycle stage / task ID,
- what changed,
- files materially changed,
- verification performed,
- gate status: PASS / PARTIAL / FAIL,
- known issues,
- next eligible task.

Do not produce a celebratory "done" when a gate is partial or failing.
