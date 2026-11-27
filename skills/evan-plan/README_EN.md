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
2. **Pre-Plan Decision Gate**: Run `git fetch`, detect project conventions (commit style, sync/merge method, protected branches), then ask the user once for three decisions: whether to enable Plan B (and which model); the branch and worktree strategy (direct edits are not offered on protected branches; the default is a new branch in `~/.t3/worktrees/<owner__repo>/<slug>`) together with sync method, merge method, and commit convention; and the execution mode plus whether `evan-build` may auto-launch relay threads.
3. **Generate & Merge**: Draft `PLAN.md` directly in single-plan mode, or draft `PLAN_A.md` and `PLAN_B.md` in parallel with strict brief isolation, merge verified decisions into `PLAN.md`, and remove `PLAN_A.md` / `PLAN_B.md`.
4. **Enterprise Gate & Freeze**: Check reuse, minimal diffs, cross-module contracts, and PR readiness against `references/plan-template.md`, then mark `状态: FROZEN` upon user confirmation.
5. **Revise a Frozen Plan**: When `evan-build` is blocked or requirements change mid-flight, follow the [revision protocol](references/revision-protocol.md) into `REVISING`: completed stages are never rewritten (reversals are appended as compensation stages), only affected stages are redone, the reason and impact go into the revision log, and the plan is re-frozen after user confirmation.

## Typical Requests

- "Use `evan-plan` to inspect the codebase and draft a staged plan for this refactor."
- "Use `evan-plan` to find the root cause of this bug, then ask me before creating plan files."
- "Continue `~/.t3/plans/my-repo/auth-refactor/PLAN.md`; here is Plan B from another model."

## What You Need To Provide

- Problem statement, target feature, or bug description.
- Target reference/merge branch (if it cannot be unambiguously inferred).
- Existing behaviors, compatibility constraints, or boundaries that must remain intact.

## Outputs

- `~/.t3/plans/<owner__repo>/<slug>/PLAN.md`: Out-of-repo persistent plan (never committed or pushed) with lifecycle tracking (including `REVISING` after freezing), depth analysis, strong-relevance file list, contract checks, PR checklist, 5-state progress table, stage handoff section, and revision log.
- `PLAN_A.md`, `PLAN_B.md` (and `HANDOFF_SECOND_OPINION.md` if used): Temporary planning drafts deleted automatically once `PLAN.md` is merged.

## Boundaries

- Strictly **read-only on the working tree** during planning; never edits business code or creates branches/worktrees early.
- When automated cross-model dispatch is unavailable, the user chooses between single-plan mode or generating `HANDOFF_SECOND_OPINION.md`.
- Only plans marked `状态: FROZEN` are eligible for execution.
- Planning and revision leave the working tree and index untouched. A fetch can update local refs; freezing also records the absolute plan path in the checkout’s private Git pointer `evan-plan-ref`; pushing and creating PRs are always the user's decision.
- Small tasks (one stage, at most 3 files, no cross-module contract change) use a lightweight tier: a compact plan template and only the branch-strategy question.

## Related Skills

- [`evan-build`](../evan-build/README_EN.md): Executes a `FROZEN` `PLAN.md` stage by stage with testing, Open Code Review, and clean commits.
