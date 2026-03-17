---
name: agent-workflow-optimizer
description: >-
  Audits and hardens agent workflows using harness-first evaluation,
  executable contracts, and conformance suites. Use when agent output drifts,
  specs/tests diverge, or teams need deterministic iteration in critical code.
---

# Agent Workflow Optimizer

## Purpose

Agent output becomes unstable when requirements, invariants, and verification are implicit.
This skill installs a minimal configurancy layer: explicit contracts + executable checks + deterministic loops.

## When To Use

- Starting a new agent-enabled codebase.
- Refactoring an existing workflow with drift, rework, or flaky outcomes.
- Aligning specs, prompts, code, and tests in critical systems.
- Building conformance suites or harnesses that must survive agent scale.

## Core Principles

1. Configurancy first: keep the smallest explicit set of `affordances`, `invariants`, `constraints`, and `rationale`.
2. Executable over prose: invariants must be enforced by conformance tests, static checks, or runtime assertions.
3. Spec hierarchy is explicit:
- External oracle
- Reference model
- Conformance suite
- Prose rationale
4. Drift is a first-class failure: mismatch between hierarchy layers must fail CI.
5. Harness-first iteration: evaluate from day 1, then adjust context/code based on observed failures.
6. `AGENTS.md` is a table of contents to canonical docs, not a duplicated encyclopedia.
7. `ARCHITECTURE.md` is the structural contract: module boundaries, dependency direction, and ownership.

## Quick Start (mandatory)

1. Run a first-pass audit.
2. Classify maturity (`M0`-`M3`) and select mode (`Bootstrap`, `Retrofit`, `Harden`).
3. Apply the smallest patch set that restores deterministic behavior.
4. Prioritize in this order:
- Harness + evaluation dataset
- Spec hierarchy + conformance gates
- Iteration protocol + verification gates
5. Return this report:

```md
Maturity: M0|M1|M2|M3
Confidence: Low|Medium|High

Findings:
1. ...
2. ...

Mode: Bootstrap|Retrofit|Harden

Planned edits:
1. ...
2. ...

Verification plan:
1. ...
2. ...

Remaining gaps:
1. ...
```

## Maturity Rubric

- `M0`: ad-hoc prompts, no explicit contracts, no eval harness.
- `M1`: docs exist, but invariants are mostly prose and unenforced.
- `M2`: partial conformance/harness exists, but layers drift or scripts conflict.
- `M3`: deterministic loop exists (`harness -> conformance -> verify -> document`) with minor enforcement gaps only.

## Mode Selection

- `Bootstrap` for `M0-M1`: create missing canonical artifacts.
- `Retrofit` for `M2`: reconcile conflicts, keep existing structure where valid.
- `Harden` for `M3`: add enforcement checks and remove drift sources.

## Workflow

1. Audit current artifacts and map evidence.
- Agent instruction files (`AGENTS.md`, `CLAUDE.md`, equivalents).
- Spec files (`docs/spec/*`, `spec/*`, RFCs).
- Loop/task files (`TASKS.md`, `PLAN_PROGRESS.md`, `RALPH_TASKS.md`, runner scripts).
- Test, conformance, type/static, and runtime assertion gates.

2. Establish or repair the spec hierarchy.
- Identify the strongest external oracle available (standard, upstream system, or reference impl).
- Define/repair reference model.
- Make conformance suite executable and authoritative for behavior.
- Keep prose rationale tied to decisions and trade-offs.

3. Install harness-first loop.
- Start with high-signal `A+` eval examples before broad coverage.
- Add hidden/production-derived examples once baseline behavior stabilizes.
- Triage failures with cause tags: `REQ` (unclear requirement), `CTX` (missing context), `CODE` (implementation bug).

4. Align architecture contract.
- Ensure `ARCHITECTURE.md` defines stable module boundaries and allowed dependency flow.
- Ensure prompts and task docs reference `ARCHITECTURE.md` for structural decisions.
- Add/verify structural lint or architecture-conformance tests for boundary rules.

5. Wire loop runner prompts.
- Loop scripts must read one prompt source file.
- Prompt must reference canonical docs and precedence order.
- Scripts must fail fast when prompt source file is missing.
- Keep context in predictable sections; avoid duplicated policy copies.

6. Enforce deterministic iteration behavior.
- Require task brief before edits: `Goal`, `Scope`, `Non-goals`, `Invariants`, `Acceptance`.
- Require bug-fix loop: `Reproduce -> Fix -> Guard -> Verify -> Document`.
- Require verify gates before completion:
- targeted tests for touched invariants
- type/static checks
- relevant conformance suite
- changelog/config-delta update when behavior or architecture contract changes

7. Add intelligence-retention checks.
- Run the 30-day test: answer behavior questions from docs + tests, without reading implementation code.
- Add bidirectional checks:
- `Doc -> Code`: claimed invariants are enforced.
- `Code -> Doc`: enforced behavior is documented.

8. Clean artifact hygiene.
- Add ignore rules for local agent artifacts.
- If already tracked, untrack via `git rm --cached`; ignore rules are not retroactive.
- Keep generated local artifacts outside repo when possible.

## Non-Negotiable Rules

- Update existing docs before creating new files.
- Keep naming consistent with project conventions.
- Treat historical logs as non-normative unless explicitly promoted.
- Never ship a fix without regression guard for touched invariants.
- If behavior contract changes, update spec, conformance, and rationale in the same iteration.
- If architecture contract changes, update `ARCHITECTURE.md` and structural checks in the same iteration.
- A markdown invariant without enforcement is incomplete.
- Keep SKILL context lean: load detailed templates only from references when needed.

## References

Load detailed templates/checklists from `references/workflow-blueprint.md`:
- `references/workflow-blueprint.md`
- `references/architecture-template.md`
- `Canonical artifact set`
- `Spec hierarchy + drift gates`
- `Harness engineering checklist`
- `Conformance suite patterns`
- `30-day test + question bank`
- `Configurancy delta template`
