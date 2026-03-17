# ARCHITECTURE.md Template

Use this as a starting point for `ARCHITECTURE.md` in the target repository.

## Purpose

Define structural constraints that must remain true as humans and agents modify the codebase.

## Scope

- Covers module boundaries, dependency direction, ownership, and forbidden edges.
- Complements behavior contracts from spec/conformance docs.

## System Decomposition

List top-level modules/services/packages and one-line responsibilities.

Example:
- `apps/api`: external API surface and request orchestration.
- `packages/domain`: business rules, pure domain logic, invariants.
- `packages/data`: persistence adapters and data access implementations.
- `packages/integrations`: external provider clients and adapters.

## Dependency Direction

Define allowed dependency flow from outer layers to inner layers.

Example:
1. `apps/*` -> `packages/domain`, `packages/data`, `packages/integrations`
2. `packages/data` -> `packages/domain`
3. `packages/integrations` -> `packages/domain`
4. `packages/domain` -> no inward dependency on app, data, or integration layers

## Forbidden Dependencies

List explicit forbidden edges.

Example:
- `packages/domain` MUST NOT import from `apps/*`.
- `packages/domain` MUST NOT import from `packages/data`.
- `packages/data` MUST NOT import from `apps/*`.

## Ownership

Map owners to architectural surfaces.

Example:
- `apps/api`: Team Platform
- `packages/domain`: Team Core
- `packages/data`: Team Platform
- `packages/integrations`: Team Integrations

## Invariants

List architecture invariants that must hold.

Example:
- INV-A1: Domain logic is side-effect free and testable in isolation.
- INV-A2: Transport/API schemas do not leak into domain entities.
- INV-A3: Integration-specific models stay at the boundary adapter layer.

## Enforcement

Define mechanical checks and CI gates.

Example:
- Lint rule: dependency boundaries (`dependency-cruiser`, ESLint boundaries, custom checker).
- Architecture test suite: verifies forbidden import edges.
- CI policy: structural violations fail pull requests.

## Change Protocol

How to change architecture safely:

1. Propose change with rationale and impact.
2. Update this `ARCHITECTURE.md`.
3. Update structural checks in the same PR.
4. Link migration tasks if refactor spans multiple PRs.

## Agent Execution Notes

- `AGENTS.md` should link to this file as structural source of truth.
- Refactor tasks must cite relevant dependency rules before code edits.
- If implementation conflicts with this document, resolve by updating docs + checks together.
