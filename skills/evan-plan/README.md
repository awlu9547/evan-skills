# `evan-plan` 技能

[English](README_EN.md)

`evan-plan` 用于为现有 Git 项目的功能新增、优化或修复制定分阶段实施计划，强调先只读调研代码与 Git 基准、按需引入跨模型独立方案交叉核实，并在用户确认定稿后冻结计划供执行。

## 适合用它做什么

- 需求尚未完全对齐时，先从代码入口、调用方、测试缺口与 Git 历史查明现状。
- 评估任务复杂度（`轻量` / `标准`）与执行策略（`单线程` / `每阶段提交后换新线程接力`）。
- 撰写按可独立验收功能切片划分的 `PLAN.md`（方案 A）。
- 标准档任务中，调度异构模型独立产出 `PLAN_B.md`（或生成 `HANDOFF_SECOND_OPINION.md` 人工交接），避免单一模型盲区。
- 逐项核实方案 A 与方案 B 的分歧，附带代码/测试证据裁决后将 `PLAN.md` 定稿为 `FROZEN`。

## 工作方式

1. **对齐需求与 Git 基准**：三句话对齐问题、验收标准与保留边界，结合 `git merge-base` 与工作区状态只读追踪真实入口。
2. **评估复杂度并写方案 A**：在 `docs/<feature-slug>/PLAN.md` 写入 `DRAFT` 计划，包含各阶段前置依赖、改动路径、风险与验证命令。
3. **独立第二方案（标准档）**：避开当前主模型家族派发子 Agent 只读仓库并写入 `PLAN_B.md`，状态切至 `AWAITING_OPINION`；轻量档可跳过。
4. **分歧核实与定稿**：对比两版方案后将结论合并回 `PLAN.md`，经用户确认后标记 `状态: FROZEN`。

## 典型请求

- “先用 `evan-plan` 调研下这个重构，出个分阶段方案。”
- “这个功能比较复杂，按 `evan-plan` 跑双方案对比后再定稿。”
- “继续完善 `docs/auth-refactor/PLAN.md`，我把外部模型的方案 B 贴回来了。”

## 你需要提供

- 要解决的问题、目标功能或 Bug 现象。
- 目标合并分支（若当前上下文无法唯一判断，需明确指出参考分支）。
- 必须保留的原有行为、兼容性要求或特定约束。

## 产出

- `docs/<feature-slug>/PLAN.md`：包含状态机（`DRAFT` → `AWAITING_OPINION` → `MERGING` → `FROZEN`）、复杂度与执行方式、Git 基准、分阶段步骤、验证命令及进度表。
- `docs/<feature-slug>/PLAN_B.md`（标准档）：由另一模型家族独立产出的对比方案。
- `docs/<feature-slug>/HANDOFF_SECOND_OPINION.md`（降级兜底）：当无可用外部模型调度工具时生成的交接文件。

## 边界

- 规划阶段严格**只读业务代码**，仅更新计划目录文档，不直接修改业务源码。
- 标准档依赖多模型调度环境（如 T3 Code 的 `orchestrator_capabilities` / `delegate_task`）；若不可用，会自动降级为交接文件模式，由用户转交外部模型生成第二方案。
- 只有标记为 `状态: FROZEN` 的计划才可交由下游实施。

## 相关技能

- [`evan-build`](../evan-build/README.md)：按已定稿（`FROZEN`）的 `PLAN.md` 分阶段实施、验证、审查与提交。
