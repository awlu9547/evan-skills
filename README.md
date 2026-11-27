<div align="center">
  <p>
    <img src="assets/readme-banner-cn.jpg" alt="Evan Skills：工程化 AI Agent 工作流" width="100%">
  </p>
  <p>
    <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/license-MIT-2ea44f"></a>
    <a href="#3-安装"><img alt="Install" src="https://img.shields.io/badge/install-Claude%20Code%20%7C%20Codex%20%7C%20Pi-111827"></a>
    <a href="#4-技能索引"><img alt="Skills" src="https://img.shields.io/badge/skills-2-0ea5e9"></a>
    <a href="README_EN.md"><img alt="Language" src="https://img.shields.io/badge/language-%E4%B8%AD%E6%96%87%20%7C%20English-1f6feb"></a>
  </p>
  <p>
    <a href="#2-快速开始">快速开始</a>
    · <a href="#3-安装">立即安装</a>
    · <a href="#4-技能索引">技能索引</a>
    · <a href="#5-运行依赖与环境适配">环境适配</a>
    · <a href="#6-开源协议">MIT 协议</a>
    · <a href="README_EN.md">English</a>
  </p>
</div>

---

`evan-skills` 收录了一套围绕 `PLAN.md` 状态机组织的可复用编程工作流技能，将复杂开发任务拆解为**先只读规划、按需双模型交叉核实、再单线程分阶段交付与审查收口**的标准流水线。

## 目录

- [1. 工作流理念](#1-工作流理念)
- [2. 快速开始](#2-快速开始)
- [3. 安装](#3-安装)
  - [3.1 `npx skills` 安装方式](#31-npx-skills-安装方式)
  - [3.2 Claude Code 安装方式](#32-claude-code-安装方式)
  - [3.3 Codex 安装方式](#33-codex-安装方式)
  - [3.4 Pi 与其他 Agent 场景](#34-pi-与其他-agent-场景)
- [4. 技能索引](#4-技能索引)
- [5. 运行依赖与环境适配](#5-运行依赖与环境适配)
- [6. 开源协议](#6-开源协议)

## 1. 工作流理念

1. **先定方案，不动代码。** `evan-plan` 从最新可核实的 Git 基准出发，查清根因或跨模块影响。标准任务一次确认是否需要第二方案、在哪个分支做、是否允许开新线程；单阶段、不超过 3 个文件且不改跨模块契约的小任务只问分支策略。受保护分支默认另开分支。
2. **按同一份计划交接。** 冻结的计划留在 `~/.t3/plans/<owner__repo>/<slug>/PLAN.md`，不进 Git。每个工作区可以通过本地指针找回它。收到开工指令后，`evan-build` 按阶段检查差异、重跑验证、做审查，按项目规范本地提交并记下真实提交号。
3. **中途改需求，不推倒重来。** 需要改契约或验收时转 `REVISING`，保留已完成阶段，只改受影响部分，经你确认再恢复 `FROZEN`。PR 评审或 CI 失败按追加阶段修复。
4. **本地完成不等于已经发布。** 收口先刷新目标分支、预检冲突、提议本地 CI，并给出 PR 摘要。是否 push、创建或更新 PR、合并 PR，逐项由你决定。

## 2. 快速开始

安装完成后，在任务描述中直接点名技能或自然触发即可。下面这些提示词可以直接复制使用：

| 想做什么 | 直接这样说 |
|---|---|
| 调研现状并制定企业级计划 | `使用 evan-plan 调研这个问题，评估后询问我是否开方案 B 及分支策略，不要改业务代码。` |
| 跑双模型对比并汇总定稿 | `用 evan-plan 规划这个需求，列出当前可用的异构模型让我选一个跑方案 B，汇总后删掉草稿。` |
| 按已定稿计划连续跑完全部阶段 | `使用 evan-build，按 ~/.t3/plans/<owner__repo>/<slug>/PLAN.md 开始连续实施并完成 PR 前收口。` |
| 每阶段跑完先停下汇报 | `使用 evan-build，按 ~/.t3/plans/<owner__repo>/<slug>/PLAN.md 执行，每阶段跑完结束跟我汇报。` |
| 实施中遇阻塞或需求变化，修订计划 | `使用 evan-plan 修订 ~/.t3/plans/<owner__repo>/<slug>/PLAN.md：<阻塞原因或新需求>，已完成阶段不要改，只重做受影响部分。` |
| 提 PR 后按评审意见修复 | `使用 evan-build，按 ~/.t3/plans/<owner__repo>/<slug>/PLAN.md 处理这批 PR 评审意见，只本地提交，是否推送我来决定。` |
| 中断后从交接快照续跑 | `使用 evan-build，核对 ~/.t3/plans/<owner__repo>/<slug>/PLAN.md 的阶段状态与交接快照，继续剩余阶段。` |

## 3. 安装

`evan-skills` 按照标准 `skills/<skill-name>/SKILL.md` 结构组织，每个子目录都是独立可安装的技能单元。

### 3.1 `npx skills` 安装方式

需要先安装 [Node.js 18 或更高版本](https://nodejs.org/)。先查看仓库中可安装的技能列表：

```bash
npx skills add awlu9547/evan-skills --list
```

安装全部技能到全局（推荐）：

```bash
npx skills add awlu9547/evan-skills --global --skill '*' --yes
```

如果只为当前项目安装或只选单个技能，可按需调整参数：

```bash
npx skills add awlu9547/evan-skills --skill evan-plan --yes
npx skills add awlu9547/evan-skills --skill evan-build --yes
```

### 3.2 Claude Code 安装方式

可以使用上面的 `npx skills` 命令直接安装，也可以保留稳定本地 clone 后软链接到 `~/.claude/skills/`：

```bash
git clone https://github.com/awlu9547/evan-skills.git ~/ai-skills/evan-skills
mkdir -p ~/.claude/skills
ln -s ~/ai-skills/evan-skills/skills/evan-plan ~/.claude/skills/evan-plan
ln -s ~/ai-skills/evan-skills/skills/evan-build ~/.claude/skills/evan-build
```

后续更新只需在本地仓库目录执行 `git pull`。

### 3.3 Codex 安装方式

同样支持通过 `npx skills` 安装，或 clone 后软链接至 `~/.codex/skills/`：

```bash
git clone https://github.com/awlu9547/evan-skills.git ~/ai-skills/evan-skills
mkdir -p ~/.codex/skills
ln -s ~/ai-skills/evan-skills/skills/evan-plan ~/.codex/skills/evan-plan
ln -s ~/ai-skills/evan-skills/skills/evan-build ~/.codex/skills/evan-build
```

安装完成后开启新会话即可通过 `$evan-plan` 或 `$evan-build` 调用。

### 3.4 Pi 与其他 Agent 场景

如果你使用 Pi、OpenCode 等兼容 Agent Skills 规范的工具，可将完整技能目录复制或软链接到对应技能目录（例如 `~/.agents/skills/` 或 `~/.pi/agent/skills/`）：

```bash
mkdir -p ~/.agents/skills
cp -R ~/ai-skills/evan-skills/skills/evan-plan ~/.agents/skills/
cp -R ~/ai-skills/evan-skills/skills/evan-build ~/.agents/skills/
```

## 4. 技能索引

| 技能 | 状态 | 一句话用途 | 典型触发词 | 详情 |
|---|---|---|---|---|
| [`evan-plan`](skills/evan-plan/README.md) | `Stable` | 调研 Git 基准与代码入口，生成并按需双模型论证分阶段 `PLAN.md`，并支持冻结后修订 | `evan-plan`、出方案、先 plan | [详情](skills/evan-plan/README.md) |
| [`evan-build`](skills/evan-build/README.md) | `Stable` | 按 `FROZEN` 计划单线程实施、硬验证、执行 OCR 审查、按项目规范提交、授权后接力收口并处理 PR 评审反馈 | `evan-build`、执行第 N 阶段 | [详情](skills/evan-build/README.md) |

## 5. 运行依赖与环境适配

两个技能的核心规划、编码、验证与 Git 提交流程**不依赖特定 IDE 即可开箱使用**；部分增强能力会根据你的本地环境自动启用或优雅降级：

| 增强能力 | 默认使用工具 | 无对应工具时的降级方式 |
|---|---|---|
| 跨模型并行生成 `PLAN_B.md` | T3 Code 编排工具（先 `orchestrator_capabilities` 列出可用模型，由用户选定后 `delegate_task`） | 由用户二选一：直接转单方案，或生成 `HANDOFF_SECOND_OPINION.md` 交外部模型 |
| 阶段提交前代码审查 | `open-code-review`（本机 `ocr` CLI） | 自动降级为 `open-code-review-delegate`（复用宿主模型审查）；两者均不可用则记录阻塞 |
| 跨阶段抗上下文腐烂换线程 | 经明确授权的 T3 Code 线程接力（`t3_worktree_status`、`t3_thread_launch`） | 输出极简接力提示词（含 `PLAN.md`、worktree 与哈希），由用户开新会话粘贴继续 |

## 6. 开源协议

本项目基于 [MIT License](LICENSE) 开源。
