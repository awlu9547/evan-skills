# `evan-plan` Skill

[中文说明](README.md)

`evan-plan` creates enterprise-grade staged plans for bug fixes, new features, or optimizations in existing Git repositories—combining read-only investigation, a pre-plan decision gate (Plan B model selection & worktree strategy), first-principles or higher-order impact analysis, and out-of-repo `PLAN.md` freezing ready for PR delivery.

## What To Use It For

- Investigating entry points, callers, test gaps, and Git history before writing any production code.
- Performing first-principles root-cause analysis for bug fixes, or second- and third-order impact analysis (cross-module contracts, user experience, complexity, and maintenance cost) for new features/optimizations.
- Auditing reusable modules, justifying any new abstractions, and verifying strict relevance for every file to be touched.
- Generating independent `PLAN_A.md` and `PLAN_B.md` (using a user-selected accessible cross-family model), reconciling differences with evidence, deleting drafts, and freezing `PLAN.md`.

## Workflow

1. **Read-only Investigation & Depth Analysis**: Align requirements, Git baseline and branch drift, reusable modules, and run root-cause or higher-order impact analysis.
2. **Pre-Plan Decision Gate**: Probe accessible cross-family models and ask the user once whether to enable Plan B (and which model to use) and whether to branch into `~/.t3/worktrees/<repo>/<task-slug>`.
3. **Generate & Merge**: Draft `PLAN.md` directly in single-plan mode, or draft `PLAN_A.md` and `PLAN_B.md` in parallel with strict brief isolation, merge verified decisions into `PLAN.md`, and remove `PLAN_A.md` / `PLAN_B.md`.
4. **Enterprise Gate & Freeze**: Check reuse, minimal diffs, cross-module contracts, and PR readiness against `references/plan-template.md`, then mark `状态: FROZEN` upon user confirmation.

## Typical Requests

- "Use `evan-plan` to inspect the codebase and draft a staged plan for this refactor."
- "Use `evan-plan` to find the root cause of this bug, then ask me before creating plan files."
- "Continue `~/.t3/plans/my-repo/auth-refactor/PLAN.md`; here is Plan B from another model."

## What You Need To Provide

- Problem statement, target feature, or bug description.
- Target reference/merge branch (if it cannot be unambiguously inferred).
- Existing behaviors, compatibility constraints, or boundaries that must remain intact.

## Outputs

- `~/.t3/plans/<repo>/<feature-slug>/PLAN.md`: Out-of-repo persistent plan (never committed or pushed) with lifecycle tracking, depth analysis, strong-relevance file list, contract checks, PR checklist, 5-state progress table, and stage handoff section.
- `PLAN_A.md`, `PLAN_B.md` (and `HANDOFF_SECOND_OPINION.md` if used): Temporary planning drafts deleted automatically once `PLAN.md` is merged.

## Boundaries

- Strictly **read-only on the repository** during planning; never edits business code or creates branches/worktrees early.
- When automated cross-model dispatch is unavailable, the user chooses between single-plan mode or generating `HANDOFF_SECOND_OPINION.md`.
- Only plans marked `状态: FROZEN` are eligible for execution.

## Related Skills

- [`evan-build`](../evan-build/README_EN.md): Executes a `FROZEN` `PLAN.md` stage by stage with testing, Open Code Review, and clean commits.
