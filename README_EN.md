<div align="center">
  <p>
    <img src="assets/readme-banner-en.jpg" alt="Evan Skills: Engineering-Grade AI Agent Workflow" width="100%">
  </p>
  <p>
    <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/license-MIT-2ea44f"></a>
    <a href="#3-installation"><img alt="Install" src="https://img.shields.io/badge/install-Claude%20Code%20%7C%20Codex%20%7C%20Pi-111827"></a>
    <a href="#4-skills-index"><img alt="Skills" src="https://img.shields.io/badge/skills-2-0ea5e9"></a>
    <a href="README.md"><img alt="Language" src="https://img.shields.io/badge/language-%E4%B8%AD%E6%96%87%20%7C%20English-1f6feb"></a>
  </p>
  <p>
    <a href="#2-quick-start">Quick Start</a>
    · <a href="#3-installation">Install</a>
    · <a href="#4-skills-index">Skills Index</a>
    · <a href="#5-runtime-dependencies-and-environment-adaptation">Environment</a>
    · <a href="#6-license">MIT License</a>
    · <a href="README.md">中文说明</a>
  </p>
</div>

---

`evan-skills` is a reusable coding-workflow skill pack organized around a `PLAN.md` state machine—turning complex engineering tasks into a disciplined pipeline of **read-only investigation, optional cross-model second opinions, single-threaded staged execution, bounded code review, and clean branch wrap-up**.

## Contents

- [1. Workflow Philosophy](#1-workflow-philosophy)
- [2. Quick Start](#2-quick-start)
- [3. Installation](#3-installation)
  - [3.1 `npx skills`](#31-npx-skills)
  - [3.2 Claude Code](#32-claude-code)
  - [3.3 Codex](#33-codex)
  - [3.4 Pi and Other Agents](#34-pi-and-other-agents)
- [4. Skills Index](#4-skills-index)
- [5. Runtime Dependencies and Environment Adaptation](#5-runtime-dependencies-and-environment-adaptation)
- [6. License](#6-license)

## 1. Workflow Philosophy

Most failures in non-trivial agent coding tasks stem from four habits: **editing code before understanding entry points**, **single-model blind spots**, **context rot in long sessions**, and **claiming tests pass without re-running after the last edit**. `evan-plan` and `evan-build` separate thinking from building through an explicit lifecycle (`DRAFT` → `AWAITING_OPINION` → `MERGING` → `FROZEN`):

- **Read-only planning & out-of-repo plans**: `evan-plan` keeps the repository strictly read-only and stores plans out of repo at `~/.t3/plans/<repo>/<feature-slug>/PLAN.md` so planning drafts never pollute commits or PRs.
- **Pre-plan gate & depth analysis**: Before writing any plan file, the agent probes accessible models and asks whether to enable an independent Plan B (user selects the model) and whether to branch into `~/.t3/worktrees/<repo>/<task-slug>`. Bug fixes require first-principles root-cause analysis; new features require second- and third-order impact analysis.
- **Continuous execution, strict diff checks & handoff snapshots**: `evan-build` runs all stages continuously by default, verifies strong-relevance diffs, runs tests and bounded Open Code Review, commits only business code, writes real short SHAs and a 5-line handoff snapshot back to `PLAN.md`, and outputs a PR-ready summary at closure.

## 2. Quick Start

Once installed, mention the skill explicitly or describe your task naturally. Copy-ready prompts:

| What You Want | Prompt Example |
|---|---|
| Investigate & create an enterprise plan | `Use evan-plan to inspect this codebase, then ask me whether to run Plan B and create a new worktree branch without modifying source files.` |
| Run dual-model comparison & merge | `Use evan-plan to list accessible cross-family models for Plan B, let me pick one, merge into PLAN.md, and delete the drafts.` |
| Execute all stages continuously | `Use evan-build to run all stages from ~/.t3/plans/<repo>/<slug>/PLAN.md continuously through PR-ready closure.` |
| Pause and report after each stage | `Use evan-build to execute ~/.t3/plans/<repo>/<slug>/PLAN.md, stopping to report after each stage.` |
| Resume from handoff snapshot | `Use evan-build to check stage states and the handoff snapshot in ~/.t3/plans/<repo>/<slug>/PLAN.md, then continue.` |

## 3. Installation

`evan-skills` follows the standard `skills/<skill-name>/SKILL.md` layout, making each subdirectory an independently installable skill unit.

### 3.1 `npx skills`

Requires [Node.js 18 or newer](https://nodejs.org/). List available skills in this repository first:

```bash
npx skills add awlu9547/evan-skills --list
```

Install all skills globally (recommended):

```bash
npx skills add awlu9547/evan-skills --global --skill '*' --yes
```

Or install only a specific skill for your current project or globally:

```bash
npx skills add awlu9547/evan-skills --skill evan-plan --yes
npx skills add awlu9547/evan-skills --skill evan-build --yes
```

### 3.2 Claude Code

You can install via `npx skills` above, or keep a local clone and symlink into `~/.claude/skills/`:

```bash
git clone https://github.com/awlu9547/evan-skills.git ~/ai-skills/evan-skills
mkdir -p ~/.claude/skills
ln -s ~/ai-skills/evan-skills/skills/evan-plan ~/.claude/skills/evan-plan
ln -s ~/ai-skills/evan-skills/skills/evan-build ~/.claude/skills/evan-build
```

Run `git pull` inside `~/ai-skills/evan-skills` whenever you want to update.

### 3.3 Codex

Install via `npx skills`, or clone and symlink into `~/.codex/skills/`:

```bash
git clone https://github.com/awlu9547/evan-skills.git ~/ai-skills/evan-skills
mkdir -p ~/.codex/skills
ln -s ~/ai-skills/evan-skills/skills/evan-plan ~/.codex/skills/evan-plan
ln -s ~/ai-skills/evan-skills/skills/evan-build ~/.codex/skills/evan-build
```

Start a new session and invoke with `$evan-plan` or `$evan-build`.

### 3.4 Pi and Other Agents

For Pi, OpenCode, or other Agent Skills-compatible tools, copy or symlink the full skill directories into your agent's skills folder (e.g., `~/.agents/skills/` or `~/.pi/agent/skills/`):

```bash
mkdir -p ~/.agents/skills
cp -R ~/ai-skills/evan-skills/skills/evan-plan ~/.agents/skills/
cp -R ~/ai-skills/evan-skills/skills/evan-build ~/.agents/skills/
```

## 4. Skills Index

| Skill | Status | Purpose | Typical Triggers | Details |
|---|---|---|---|---|
| [`evan-plan`](skills/evan-plan/README_EN.md) | `Stable` | Inspect Git baseline & code entry points, draft `PLAN.md`, and optionally reconcile a cross-model Plan B | `evan-plan`, plan first, staged plan | [Details](skills/evan-plan/README_EN.md) |
| [`evan-build`](skills/evan-build/README_EN.md) | `Stable` | Implement `FROZEN` plans stage by stage with hard verification, OCR review, commits, and thread handoffs | `evan-build`, execute stage N | [Details](skills/evan-build/README_EN.md) |

## 5. Runtime Dependencies and Environment Adaptation

The core planning, implementation, verification, and Git commit workflows work **out of the box** without specialized IDE tooling. Optional enhancements gracefully degrade when specific tools are not installed:

| Enhancement | Preferred Tooling | Fallback When Unavailable |
|---|---|---|
| Cross-model `PLAN_B.md` generation | T3 Code orchestration (`orchestrator_capabilities` to list accessible models, user selects, `delegate_task`) | Offers a choice between single-plan mode or generating `HANDOFF_SECOND_OPINION.md` |
| Pre-commit stage code review | `open-code-review` (`ocr` CLI) | Falls back to `open-code-review-delegate` (host-model review) or standard testing checks |
| Cross-stage fresh-thread handoff | T3 Code thread relay (highest priority: `t3_worktree_status`, `t3_thread_launch`) | Outputs a compact copy-ready handoff prompt with `PLAN.md`, worktree path, and commit SHA |

## 6. License

Released under the [MIT License](LICENSE).
