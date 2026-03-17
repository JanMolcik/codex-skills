# Workflow Blueprint

## Table of Contents

1. Canonical File Set
2. Spec Hierarchy and Drift Gates
3. Architecture Contract (`ARCHITECTURE.md`)
4. Harness Engineering Checklist
5. Conformance Suite Design Patterns
6. Required Policy Blocks
7. Invariant Starter Pack
8. 30-Day Test and Question Bank
9. Configurancy Delta Template
10. Loop Runner Wiring Checklist
11. Artifact Hygiene Checklist
12. First-Run Audit Output Template
13. Quick Search

## 1) Canonical File Set

Use project-specific names, but keep this shape:

1. Canonical spec:
- `docs/spec/PROJECT_SPEC.md`

2. Operational truth:
- `docs/spec/CURRENT_TRUTH.md`
- Contains precedence and currently active decisions.

3. Iteration protocol:
- `docs/spec/AGENT_ITERATION_PROTOCOL.md`
- Contains required task brief, bug-fix loop, and DoD.

4. Spec/contract changelog:
- `docs/spec/SPEC_CHANGELOG.md`
- Logs behavioral contract changes.

5. Current operational docs:
- `TASKS.md`-style board (current only)
- `PLAN_PROGRESS.md`-style snapshot (current only)
- Archive legacy logs to `docs/archive/<date>/`.

6. Harness + evaluation:
- `docs/spec/HARNESS.md` (or equivalent)
- Defines evaluation sets, failure taxonomy, and triage policy.

7. Architecture contract:
- `ARCHITECTURE.md` (repo root or `docs/architecture/ARCHITECTURE.md`)
- Defines module boundaries, dependency direction, ownership, and forbidden edges.
- Start from template: `references/architecture-template.md`

## 2) Spec Hierarchy and Drift Gates

Make hierarchy explicit and enforceable:

1. External oracle (upstream standard/system/reference implementation)
2. Reference model (executable translation of expected behavior)
3. Conformance suite (gates implementations)
4. Prose rationale (why constraints exist)

Drift policy:
- If hierarchy layers disagree, treat as CI failure.
- If conformance passes but oracle mismatches, treat conformance as defective.
- Any behavior change must update affected layers in the same iteration.

## 3) Architecture Contract (`ARCHITECTURE.md`)

Minimum required sections:
1. System decomposition (modules/services/packages)
2. Allowed dependency direction
3. Forbidden dependencies / anti-corruption boundaries
4. Ownership map (who owns what)
5. Change protocol (how to propose architecture changes)

Enforcement policy:
- Add structural checks (custom lint, dependency rules, architecture tests).
- Failing architecture rule = CI failure.
- If structure changes, update `ARCHITECTURE.md` + checks in the same PR.

Prompting policy:
- `AGENTS.md` links to `ARCHITECTURE.md` as source for structural decisions.
- Agent tasks involving refactors/import graph changes must cite architecture constraints.

## 4) Harness Engineering Checklist

Harness-first flow:

1. Build `A+` eval set first:
- concise, representative, high-signal tasks
- derived from real bugs/incidents where possible

2. Add hidden eval set:
- production-derived or hard examples
- used to detect overfitting to visible evals

3. Tag each failure cause:
- `REQ`: requirement ambiguity/missing contract
- `CTX`: missing or misplaced context
- `CODE`: implementation defect

4. Enforce stable context layout:
- predictable sections in prompt docs
- canonical pointers from `AGENTS.md`
- no duplicated policy blocks across multiple files

5. Verify harness quality:
- precision on known failures
- recall on historical regressions
- flaky tests tracked and triaged

## 5) Conformance Suite Design Patterns

Choose by failure surface:

- Deterministic scenario suites:
For crisp input/output invariants.

- Property/fuzz tests:
For combinatorial spaces and parser/protocol edge cases.

- Differential tests:
When comparing against an oracle/reference implementation.

- History/state checkers:
For ordering-sensitive or distributed behavior.

- Model/state exploration:
For concurrency and interleaving risks.

Rule: at least one primary conformance pattern per critical invariant.

## 6) Required Policy Blocks

### 6.1 Precedence Policy

Use explicit order:
1. Canonical spec
2. Current truth
3. Code + tests
4. Historical logs

### 6.2 Task Brief Policy

Require this block before edits:

```md
Goal:
Scope:
Non-goals:
Invariants:
Acceptance:
```

### 6.3 Bug-Fix Loop Policy

Require this sequence:
1. Reproduce
2. Fix (minimal scope)
3. Guard (test/scenario)
4. Verify
5. Document (if contract changed)

### 6.4 Conformance Gate Policy

Require:
1. targeted tests for touched invariants,
2. type/static checks,
3. relevant conformance suite,
4. changelog + configurancy delta update if behavior contract changed.

## 7) Invariant Starter Pack

Adapt IDs and wording for each domain:
- `INV-1`: single source of truth for core domain entities,
- `INV-2`: integrity constraint (e.g., no overlap, no duplicate settlement),
- `INV-3`: auditable/idempotent state transitions,
- `INV-4`: immutable historical snapshots,
- `INV-5`: RBAC + tenant isolation,
- `INV-6`: owner/resource isolation,
- `INV-7`: read model consistency with quote/calculation,
- `INV-8`: security baseline,
- `INV-9`: privacy/compliance baseline,
- `INV-10`: every critical fix has regression guard.

## 8) 30-Day Test and Question Bank

Goal: validate whether behavior can be predicted from docs + tests alone.

Process:
1. Prepare 5-10 behavior questions.
2. Ask an agent to answer from docs/tests only (no implementation code).
3. Mark each question:
- `Pass`: answer is precise and justified by docs/tests.
- `Partial`: answer exists but ambiguous.
- `Fail`: cannot answer or answer contradicts tests.
4. Convert `Partial`/`Fail` cases into missing spec/conformance tasks.

Question bank starter:
- What does `409` mean in this protocol?
- Which transitions are illegal from state `X`?
- Can value at logical offset/key `N` change after acknowledgement?
- Which invariants are guaranteed at-least-once vs exactly-once?
- What ordering constraints exist between event handlers/jobs?

## 9) Configurancy Delta Template

Use when behavior contract changes:

```yaml
config_delta:
  affordances:
    - add: "..."
  invariants:
    - strengthen: "..."
  constraints:
    - add: "..."
  rationale:
    - "..."
  enforcement:
    - "conformance: ..."
    - "runtime: ..."
```

## 10) Loop Runner Wiring Checklist

For each runner script:
- Load one prompt source file, do not duplicate inline prompt variants.
- Fail fast if prompt file is missing.
- Ensure prompt points to canonical docs and precedence rules.
- Ensure prompt enforces task brief + bug-fix loop + verify gates.

## 11) Artifact Hygiene Checklist

- Add ignore rules for local agent artifacts.
- If files are already tracked, run `git rm --cached` for those paths.
- Keep generated local artifacts outside repo when possible.
- Keep historical logs in archive, not active docs.

## 12) First-Run Audit Output Template

```md
Maturity: M0|M1|M2|M3
Confidence: Low|Medium|High

Findings:
1. ...
2. ...

Remediation plan:
1. ...
2. ...

Changes applied:
1. ...
2. ...

Remaining optional improvements:
1. ...
```

## 13) Quick Search

Use targeted search instead of loading entire docs during audits:

```bash
rg -n "Goal:|Scope:|Non-goals:|Invariants:|Acceptance:" docs spec .
rg -n "Reproduce|Fix|Guard|Verify|Document" docs spec .
rg -n "precedence|source of truth|canonical" docs spec .
rg -n "oracle|reference model|conformance|invariant|constraint|rationale" docs spec .
rg -n "ARCHITECTURE.md|dependency direction|forbidden dependencies|architecture test|module boundary" docs spec .
rg -n "harness|eval|A\\+|hidden examples|REQ|CTX|CODE" docs spec .
rg -n "config_delta|behavior change|SPEC_CHANGELOG" docs spec .
rg -n "prompt|loop|runner|RALPH|TASKS|PLAN_PROGRESS" . --glob "*.md" --glob "*.sh" --glob "*.ts" --glob "*.js"
```
