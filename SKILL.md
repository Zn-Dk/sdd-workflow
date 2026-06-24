---
name: sdd-workflow
description: |
  SDD 开发工作流编排技能。
  负责阶段路由、`_dev/` 产物管理与迭代同步协议。
  适用场景：新功能、Bug 修复、需求迭代、约束管理。
allowed-tools:
  - Read
  - Grep
  - Edit
  - Terminal
execution-mode: orchestrate
tier: standard
references:
  - knowledge-base
  - code-generate
  - code-review
  - spec-writing
---

# SDD Workflow — 规范驱动开发工作流

通用项目的开发编排层。不重复定义"怎么写代码"或"怎么做 review"，而是定义：

- **何时**触发哪个子 skill
- **产出物**的管理规范（\_dev/ 目录结构）
- **迭代协议**（Bug 修复闭环、回归检测、防遗漏机制）
- **MARK 系统**（项目级约束管理）

> ⚠️ **SPEC 为第一准则**：需求方向调整必须先改 SPEC，再改代码。

## 核心执行约束

1. `_dev/` 文件是当前需求的状态面板，不是最后统一补写的归档材料。
2. 每轮结束前必须执行 `_dev` dirty-state 判定：代码、需求、阶段、约束、待办、决策任一变化，都要映射到对应文件。
3. 被判定为 dirty 的文件必须在本轮同步，或明确说明无需同步的原因；存在未同步 dirty 产物时，不得以“已完成”“已修复”结束本轮。
4. 连续多轮对话、切换焦点后回到原需求、或重新进入已有 `_dev/` 目录时，必须执行最小恢复：`SPEC.md`、`CLAUDE.md`、`PLAN.md`、`changelog.md` 最近条目，必要时补读 `TODO.md`。

## 阶段 → 子 Skill 映射

| 阶段     | 产出物                   | 调用对象                     |
| -------- | ------------------------ | ---------------------------- |
| 需求分析 | SPEC.md, DESIGN_GUIDE.md | `spec-writing`               |
| 架构设计 | DECISIONS.md, PLAN.md    | 自身 `03_artifacts`          |
| 代码实施 | 源代码 + changelog       | `code-generate`              |
| 质量验证 | Review 报告              | `code-review`                |
| Bug 修复 | changelog + DECISIONS    | 自身 `04_iteration-protocol` |
| 约束管理 | CLAUDE.md                | 自身 `05_mark-system`        |

## 子文档索引

| 章节       | 文件                                                                 | 职责                                     | 何时加载                     |
| ---------- | -------------------------------------------------------------------- | ---------------------------------------- | ---------------------------- |
| 核心理念   | [modules/01_philosophy.md](modules/01_philosophy.md)                 | SDD 原则与文件职责矩阵                   | 首次加载本 skill 时          |
| 开发工作流 | [modules/02_workflow.md](modules/02_workflow.md)                     | 阶段编排、回顾检查点与回合级同步协议     | 进入常规开发流程时           |
| 产出物规范 | [modules/03_artifacts.md](modules/03_artifacts.md)                   | 各文件的写入规则与反模式                 | 判断 `_dev/` 文件怎么写时    |
| 迭代协议   | [modules/04_iteration-protocol.md](modules/04_iteration-protocol.md) | Bug 修复、回归、决策回溯与实验性代码约束 | 处理 Bug / 回归 / 实验代码时 |
| MARK 系统  | [modules/05_mark-system.md](modules/05_mark-system.md)               | 约束记录与防遗漏机制                     | 管理项目级约束时             |

## 资源文件

| 文件                                                       | 用途                 | 何时加载                  |
| ---------------------------------------------------------- | -------------------- | ------------------------- |
| [assets/project-skeleton.md](assets/project-skeleton.md)   | 新需求项目脚手架模板 | 初始化新的 `_dev/` 目录时 |
| [assets/decision-template.md](assets/decision-template.md) | ADR 决策记录模板     | 需要补写架构决策时        |
| [assets/checklist.md](assets/checklist.md)                 | 各阶段自检清单       | 阶段收尾或回合自检时      |

## Agent 启动协议

Skill 被加载后，Agent 必须按以下决策树确定入口：

```
0. 确定实施目录（DEV_DIR）
   ├── 用户已指定目录 / 正在某模块下工作 → 使用该目录
   └── 未明确 → 询问用户：「本次需求应在哪个模块目录下实施？」
        → 确认后设定 DEV_DIR = <模块路径>/_<feature>_dev

1. 检查 DEV_DIR 是否已存在
   ├── 有 → 读取 CLAUDE.md + SPEC.md，恢复上下文
   │        → 根据用户指令判断进入哪个阶段
   └── 无 → 询问用户需求类型：
            ├── 新功能 → 用 project-skeleton 初始化 DEV_DIR，进入阶段 1
            ├── Bug 修复 → 直接进入 04_iteration-protocol
            └── 约束管理 → 参考 05_mark-system

2. 确定需求规模（参见 02_workflow.md 规模分级）
   → 决定走完整流程还是简化流程

3. 输出启动确认：
   📋 已加载 SDD 工作流，DEV_DIR：[路径]，当前状态：[新建/继续]，进入阶段：[N]
```

## 快速使用指南

```
新功能需求 → 加载本 skill → 按 02_workflow 阶段执行
Bug 修复   → 加载本 skill → 直接跳转 04_iteration-protocol
约束管理   → 加载本 skill → 参考 05_mark-system
```
