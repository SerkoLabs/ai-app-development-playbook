<!-- AI_PLAYBOOK_TEMPLATE -->
# AI App Development Playbook

A reusable, agent-friendly operating system for building software with vibe coding without jumping directly from idea to uncontrolled code generation.

## Lifecycle

IDEA → README → PRODUCT_SPEC → USER_FLOWS → ARCHITECTURE → DATABASE → IMPLEMENTATION_PLAN → foundation → app shell → first vertical slice → audit → core features → audit → release readiness → beta.

## Model strategy

- `gpt-5.6-sol`: routine planning, decomposition, documentation, repository exploration and state tracking.
- `gpt-6-astra`: critical architecture/data-authorization review, security, P0/P1 analysis, vertical-slice audit and release gate.
- Implementation can be performed by Claude, Astra or another coding agent, but it must follow the repository contract.

See `docs/MODEL_ROUTING.md`.

## Canonical files

- `AGENTS.md` — behavioral contract.
- `docs/AI_DEVELOPMENT_PLAYBOOK.md` — lifecycle gates.
- `docs/MODEL_ROUTING.md` — cost-aware Sol/Astra routing.
- `docs/PROJECT_STATUS.md` — current phase and next action.
- `.github/agents/` — GitHub Copilot specialists.
- `.claude/skills/` — reusable task skills.
- `templates/` — required artifact templates.

## New project usage

1. Create a new repository from this template or copy the playbook files into the product repository.
2. Replace this README with the actual product README.
3. Give the agent one short instruction:

   `Read AGENTS.md and continue the lifecycle for this repository. Do not skip gates.`

4. After planning, use a capable implementation agent task-by-task from `docs/IMPLEMENTATION_PLAN.md`.
5. Use independent Astra reviewers only at the critical gates defined in `docs/MODEL_ROUTING.md`.

## Important

This system improves reliability; it does not guarantee perfect software. The quality mechanism is explicit scope + small tasks + tests + independent review + security/release gates.
