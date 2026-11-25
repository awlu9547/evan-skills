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

大型或跨模块改动的常见翻车点在于：**未查明现状就直接改代码**、**单一模型思考存在盲区**、**长对话后期上下文腐烂**以及**改完就宣称通过却未重跑最新验证**。`evan-plan` 与 `evan-build` 通过显式的计划状态机（`DRAFT` → `AWAITING_OPINION` → `MERGING` → `FROZEN`）将“想清楚”和“写正确”彻底解耦：

- **规划阶段严守只读**：`evan-plan` 仅更新 `docs/<feature-slug>/` 下的计划文件，不提前触碰业务代码。
- **独立双方案抗盲区**：标准档任务会避开当前主模型家族独立生成 `PLAN_B.md`，所有否定结论必须给出代码或测试证据。
- **硬验证与受控审查**：`evan-build` 要求每次改码后重跑验证命令，每阶段默认只做 1 次 Open Code Review，避免无休止循环。

## 2. 快速开始

安装完成后，在任务描述中直接点名技能或自然触发即可。下面这些提示词可以直接复制使用：

| 想做什么 | 直接这样说 |
|---|---|
| 调研现状并出分阶段计划 | `使用 evan-plan 调研这个问题，先按现状出一份分阶段计划，不要改业务代码。` |
| 复杂任务跑双模型交叉方案 | `用 evan-plan 按标准档规划这个重构，跑完独立方案 B 核实分歧后再给我确认。` |
| 按已定稿计划执行单个阶段 | `使用 evan-build，按 docs/xxx/PLAN.md 执行第 1 阶段。` |
| 连续执行多阶段并自动接力 | `使用 evan-build，按 docs/xxx/PLAN.md 依次执行第 2 和第 3 阶段。` |
| 中断后恢复未完成阶段 | `使用 evan-build，核对 docs/xxx/PLAN.md 和当前提交记录，继续完成剩余步骤。` |

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
| [`evan-plan`](skills/evan-plan/README.md) | `Stable` | 调研 Git 基准与代码入口，生成并按需双模型论证分阶段 `PLAN.md` | `evan-plan`、出方案、先 plan | [详情](skills/evan-plan/README.md) |
| [`evan-build`](skills/evan-build/README.md) | `Stable` | 按 `FROZEN` 计划单线程实施、硬验证、执行 OCR 审查、提交与接力收口 | `evan-build`、执行第 N 阶段 | [详情](skills/evan-build/README.md) |

## 5. 运行依赖与环境适配

两个技能的核心规划、编码、验证与 Git 提交流程**不依赖特定 IDE 即可开箱使用**；部分增强能力会根据你的本地环境自动启用或优雅降级：

| 增强能力 | 默认使用工具 | 无对应工具时的降级方式 |
|---|---|---|
| 标准档自动生成 `PLAN_B.md` | T3 Code 编排工具（`orchestrator_capabilities`、`delegate_task`） | 自动退回生成 `HANDOFF_SECOND_OPINION.md`，交由外部模型人工接力 |
| 阶段提交前代码审查 | `open-code-review`（本机 `ocr` CLI） | 自动降级为 `open-code-review-delegate`（复用宿主模型审查）或常规人工/测试把关 |
| 跨阶段抗上下文腐烂换线程 | T3 Code 线程接力工具（`t3_worktree_status`、`t3_thread_launch`） | 提示用户手动开启新会话，附极简 `PLAN.md` 路径与阶段编号继续 |

## 6. 开源协议

本项目基于 [MIT License](LICENSE) 开源。
