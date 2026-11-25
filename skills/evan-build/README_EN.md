# `evan-build` Skill

[中文说明](README.md)

`evan-build` implements approved stages strictly according to a `FROZEN` `PLAN.md`, enforcing single-threaded implementation, post-edit verification, bounded Open Code Review, stage-scoped commits, clean-thread handoffs between stages, and final branch wrap-up checks.

## What To Use It For

- Implementing planned changes stage by stage within the boundaries defined in `PLAN.md`.
- Enforcing strict verification: re-running tests and acceptance commands on the latest code after every edit (including post-review fixes).
- Running bounded Open Code Review on each stage's full diff before committing (1 review by default, at most 1 follow-up review for high-risk fixes—preventing endless review loops).
- Keeping `PLAN.md` progress tables, commit titles, and minor implementation decisions synchronized in the repository.
- Preventing context rot across multi-stage tasks via clean top-level thread handoffs, and running `git merge-tree` conflict pre-checks plus a whole-branch final review on the last stage.

## Workflow

1. **Verify Plan & Git Starting Point**: Confirm `PLAN.md` is `FROZEN` with prerequisites completed, and check current branch, worktree, and reference branch drift.
2. **Implement & Hard-Verify**: Code single-threadedly along planned entry points, log minor private implementation decisions in `PLAN.md`, and re-run verification commands after every change.
3. **Review, Commit & Wrap Up**: Run 1 Open Code Review, fix blocking issues in batch and re-verify, commit only the current stage's changes with updated `PLAN.md`, optionally hand off remaining stages to a fresh thread, and run final branch wrap-up checks on the last stage.

## Typical Requests

- "Execute Stage 1 according to `PLAN.md`."
- "Proceed with Stages 2 and 3 from the frozen plan."
- "Resume the interrupted stage and finish the final wrap-up."

## What You Need To Provide

- Path to a `PLAN.md` marked `状态: FROZEN`.
- Explicit authorization specifying which stage number(s) to execute.
- Explicit authorization before any remote push, PR merge, or deployment.

## Outputs

- Stage-scoped code and test changes committed cleanly to Git.
- Updated `PLAN.md` recording stage status, test/review results, commit title identifiers, and key decisions.
- Fresh thread handoff link when multi-stage thread relay is enabled.

## Boundaries

- Will not start implementation unless `PLAN.md` is `FROZEN`; pauses and routes back to `evan-plan` if scope, public contracts, or acceptance criteria must change.
- Never performs `git push`, PR merge, or deployment without explicit authorization.
- Stage review uses `open-code-review` (`ocr` CLI) or falls back to `open-code-review-delegate`; automated thread relay uses T3 Code's `t3_worktree_status` / `t3_thread_launch`, and can be continued manually in a fresh session when those tools are absent.

## Related Skills

- [`evan-plan`](../evan-plan/README_EN.md): Produces and freezes the staged `PLAN.md` before building begins.
