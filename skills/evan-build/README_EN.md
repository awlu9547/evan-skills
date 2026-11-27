# `evan-build` Skill

[中文说明](README.md)

`evan-build` executes `FROZEN` out-of-repo `PLAN.md` plans continuously across stages—handling worktree creation, strict strong-relevance diff checks, post-edit test verification, bounded Open Code Review, business-only Simplified Chinese commits, 5-line handoff snapshots back to `PLAN.md`, T3-first thread handoffs, and PR-ready branch closure.

## What To Use It For

- Running all stages continuously by default with per-stage verification and commits (stopping per stage only when explicitly requested).
- Enforcing strict verification: re-running tests and acceptance commands on the latest code after every edit (including post-review fixes).
- Auditing `git diff --stat` against `PLAN.md`'s strong-relevance file list and running bounded Open Code Review (1 pass by default, at most 1 high-risk follow-up).
- Committing only business code and tests, then writing real short SHAs, 5-state progress, and 5-line handoff snapshots to out-of-repo `PLAN.md`.
- Handing off across clean T3 threads with highest priority (or outputting a compact copy-ready prompt for other harnesses) and generating a PR-ready delivery summary at final wrap-up.

## Workflow

1. **Verify Plan, Create Worktree & Dual-Check**: Confirm `PLAN.md` is `FROZEN`; create `~/.t3/worktrees/<repo>/<task-slug>` in Stage 1 when planned; verify 5-line handoff snapshot against real Git `HEAD` before resuming.
2. **Implement, Check Diff & Hard-Verify**: Code single-threadedly with minimal changes, check `git diff --stat` for strict relevance, and re-run acceptance plus cross-module regression tests after every edit.
3. **Review, Commit & Relay**: Run Open Code Review, commit pure business changes in Simplified Chinese, write the real short SHA and 5-line handoff snapshot to out-of-repo `PLAN.md`, relay via T3 thread (or fallback prompt), and present a PR-ready summary awaiting user confirmation before merging/cleaning temporary branches or pushing.

## Typical Requests

- "Execute `~/.t3/plans/<repo>/<slug>/PLAN.md` continuously."
- "Run the plan and stop to report after each stage."
- "Check the handoff snapshot in `PLAN.md` and finish the remaining stages."

## What You Need To Provide

- Path to a `PLAN.md` marked `状态: FROZEN`.
- Optional instruction if you want to pause after each stage (continuous execution is the default).
- Confirmation before final local merge/temp-branch deletion, remote push, or PR creation.

## Outputs

- Business-only stage commits with Simplified Chinese messages.
- Updated out-of-repo `PLAN.md` recording 5-state progress, real short SHAs, key decisions, and 5-line handoff snapshots.
- Fresh T3 thread link (or compact fallback prompt) for multi-stage relay, plus a PR-ready summary at final closure.

## Boundaries

- Will not start implementation unless `PLAN.md` is `FROZEN`; pauses and routes back to `evan-plan` if scope, public contracts, or acceptance criteria must change.
- Never performs `git push`, PR merge, or deployment without explicit authorization.
- Stage review uses `open-code-review` (`ocr` CLI) or falls back to `open-code-review-delegate`; automated thread relay uses T3 Code's `t3_worktree_status` / `t3_thread_launch`, and can be continued manually in a fresh session when those tools are absent.

## Related Skills

- [`evan-plan`](../evan-plan/README_EN.md): Produces and freezes the staged `PLAN.md` before building begins.
