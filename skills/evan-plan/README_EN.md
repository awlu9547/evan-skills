# `evan-plan` Skill

[中文说明](README.md)

`evan-plan` creates staged implementation plans for new features, optimizations, or bug fixes in existing Git repositories—prioritizing read-only codebase and Git baseline inspection, optional independent second-opinion planning from another model family, and user-confirmed freezing before implementation.

## What To Use It For

- Investigating entry points, callers, test gaps, and Git history before writing any production code.
- Assessing task complexity (`轻量` / `标准`) and execution mode (`单线程` / `每阶段提交后换新线程接力`).
- Drafting `PLAN.md` (Plan A) sliced into independently verifiable functional stages.
- Dispatching a separate model family to produce an independent `PLAN_B.md` (or generating `HANDOFF_SECOND_OPINION.md` as fallback) on standard-complexity tasks.
- Resolving differences between Plan A and Plan B with concrete code/test evidence and freezing `PLAN.md` as `FROZEN`.

## Workflow

1. **Align Requirements & Git Baseline**: Clarify the problem, observable success criteria, and preserved behaviors; inspect `git merge-base`, working tree status, and real code paths in read-only mode.
2. **Assess Complexity & Draft Plan A**: Write `DRAFT` to `docs/<feature-slug>/PLAN.md` with dependencies, file paths, risks, and verification commands per stage.
3. **Independent Plan B (Standard Tier)**: Dispatch a subagent from a different model family to read the repo and write `PLAN_B.md` (`AWAITING_OPINION`); lightweight tasks may skip this step.
4. **Reconcile & Freeze**: Merge verified decisions back into `PLAN.md` and mark `状态: FROZEN` once confirmed by the user.

## Typical Requests

- "Use `evan-plan` to inspect the codebase and draft a staged plan for this refactor."
- "This change is non-trivial—run `evan-plan` with a second-opinion plan before we freeze."
- "Continue `docs/auth-refactor/PLAN.md`; here is Plan B from another model."

## What You Need To Provide

- Problem statement, target feature, or bug description.
- Target reference/merge branch (if it cannot be unambiguously inferred).
- Existing behaviors, compatibility constraints, or boundaries that must remain intact.

## Outputs

- `docs/<feature-slug>/PLAN.md`: Lifecycle-tracked plan (`DRAFT` → `AWAITING_OPINION` → `MERGING` → `FROZEN`) with Git baseline, stages, verification commands, and progress table.
- `docs/<feature-slug>/PLAN_B.md` (Standard tier): Independent plan produced by another model family.
- `docs/<feature-slug>/HANDOFF_SECOND_OPINION.md` (Fallback): Structured handoff file when automated subagent dispatch is unavailable.

## Boundaries

- Strictly **read-only on production code** during planning; only writes plan artifacts under the target plan directory.
- Automated Plan B dispatch relies on multi-model orchestration (such as T3 Code's `orchestrator_capabilities` / `delegate_task`); when unavailable, it falls back cleanly to `HANDOFF_SECOND_OPINION.md`.
- Only plans marked `状态: FROZEN` are eligible for execution.

## Related Skills

- [`evan-build`](../evan-build/README_EN.md): Executes a `FROZEN` `PLAN.md` stage by stage with testing, Open Code Review, and clean commits.
