# `evan-build` Skill

[中文说明](README.md)

`evan-build` executes `FROZEN` out-of-repo `PLAN.md` plans continuously across stages—handling worktree creation, strict strong-relevance diff checks, post-edit test verification, bounded Open Code Review, business-only commits that follow the project's convention, 5-line handoff snapshots back to `PLAN.md`, authorized T3-first thread handoffs, PR-ready branch closure, and PR review-feedback fixes.

## What To Use It For

- Running all stages continuously by default with per-stage verification and commits (stopping per stage only when explicitly requested).
- Enforcing strict verification: re-running tests and acceptance commands on the latest code after every edit (including post-review fixes).
- Auditing `git diff --stat` against `PLAN.md`'s strong-relevance file list and running bounded Open Code Review (1 pass by default, at most 1 high-risk follow-up).
- Committing only the code, tests, and supporting files needed for the stage, then writing real short SHAs, 5-state progress, and 5-line handoff snapshots to out-of-repo `PLAN.md`.
- Handing off across clean T3 threads when the plan records `接力授权: 已确认` (otherwise, or on other harnesses, outputting a compact copy-ready prompt), and at final wrap-up running `git fetch`, a `git merge-tree` precheck, a whole-branch review, offering `ci-test`, and generating a PR-ready delivery summary.
- Writing commit messages per the project's convention (the plan's `提交规范` and `关联工单`; Simplified Chinese only when no convention exists).
- Handling PR review comments or CI failures after the PR is open as an appended review-fix stage; commits stay local and pushing is your decision.

## Workflow

1. **Locate Plan, Create Worktree & Dual-Check**: Find `PLAN.md` via your path, then the pointer file inside the worktree, then the default path, and confirm it is `FROZEN`; create `~/.t3/worktrees/<owner__repo>/<slug>` (and write the pointer file) in Stage 1 when planned; on later stages run `git fetch` and verify the 5-line handoff snapshot against real Git `HEAD`, switching to `REVISING` if new reference-branch commits affect the plan.
2. **Implement, Check Diff & Hard-Verify**: Code single-threadedly with minimal changes, check `git diff --stat` for strict relevance, and re-run acceptance plus cross-module regression tests after every edit.
3. **Review, Commit & Relay**: Run Open Code Review, commit pure business changes following the project's convention, write the real short SHA and 5-line handoff snapshot to out-of-repo `PLAN.md`, relay via T3 thread once authorized (or fallback prompt), and present a PR-ready summary awaiting user confirmation before syncing and merging per the plan's sync and merge methods; pushing and PR creation remain the user's decision.

## Typical Requests

- "Execute `~/.t3/plans/<owner__repo>/<slug>/PLAN.md` continuously."
- "Run the plan and stop to report after each stage."
- "Check the handoff snapshot in `PLAN.md` and finish the remaining stages."

## What You Need To Provide

- Path to a `PLAN.md` marked `状态: FROZEN`.
- Optional instruction if you want to pause after each stage (continuous execution is the default).
- Confirmation before final local merge/temp-branch deletion, remote push, or PR creation.

## Outputs

- Business-only stage commits that follow the project's commit convention.
- Updated out-of-repo `PLAN.md` recording 5-state progress, real short SHAs, key decisions, and 5-line handoff snapshots.
- Fresh T3 thread link (or compact fallback prompt) for multi-stage relay, plus a PR-ready summary at final closure.

## Boundaries

- Will not start implementation unless `PLAN.md` is `FROZEN`; sets the plan to `REVISING` and routes back to `evan-plan` if scope, public contracts, or acceptance criteria must change.
- Never performs `git push`, PR creation or update, PR merge, force-push, or deployment without your explicit per-action authorization; when the `git merge-tree` precheck finds conflicts it only reports them.
- Never auto-launches relay threads without your recorded authorization.
- Stage review uses `open-code-review` (`ocr` CLI) or falls back to `open-code-review-delegate`; automated thread relay uses T3 Code's `t3_worktree_status` / `t3_thread_launch`, and can be continued manually in a fresh session when those tools are absent.

## Related Skills

- [`evan-plan`](../evan-plan/README_EN.md): Produces and freezes the staged `PLAN.md` before building begins.
