---
name: sdd-workflow
description: |
  规范驱动开发（SDD）工作流编排技能。
  定义需求开发的全生命周期：需求分析 → 架构设计 → 代码实施 → 质量验证 → 迭代修复。
  作为编排层，协调 spec-writing / code-generate / code-review 等子 skill。
  适用场景：新功能开发、Bug 修复、需求迭代、约束管理。
allowed-tools:
  - Read
  - Grep
  - Edit
  - Terminal
execution-mode: orchestrate
tier: standard
references:
  - code-generate
  - code-review
  - spec-writing
---

# SDD Workflow — 规范驱动开发工作流

通用的开发编排层。不重复定义"怎么写代码"或"怎么做 review"，而是定义：

- **何时**触发哪个子 skill
- **产出物**的管理规范（\_dev/ 目录结构）
- **迭代协议**（Bug 修复闭环、回归检测、防遗漏机制）
- **MARK 系统**（项目级约束管理）

> ⚠️ **SPEC 为第一准则**：需求方向调整必须先改 SPEC，再改代码。

## 阶段 → 子 Skill 映射

| 阶段     | 产出物                   | 引用的子 Skill             |
| -------- | ------------------------ | -------------------------- |
| 需求分析 | SPEC.md, DESIGN_GUIDE.md | `spec-writing`             |
| 架构设计 | DECISIONS.md, PLAN.md    | — (自身 03_artifacts)      |
| 代码实施 | 源代码 + changelog       | `code-generate`（如有）    |
| 质量验证 | Review 报告              | `code-review`（如有）      |
| Bug 修复 | changelog + DECISIONS    | 自身 04_iteration-protocol |
| 约束管理 | CLAUDE.md                | 自身 05_mark-system        |

## 子文档索引

| 章节       | 文件                                                                 | 描述                              | 优先级  |
| ---------- | -------------------------------------------------------------------- | --------------------------------- | ------- |
| 核心理念   | [modules/01_philosophy.md](modules/01_philosophy.md)                 | SDD 原则 + 文件职责矩阵           | 🔴 必读 |
| 开发工作流 | [modules/02_workflow.md](modules/02_workflow.md)                     | 阶段编排 + 回顾检查点             | 🔴 必读 |
| 产出物规范 | [modules/03_artifacts.md](modules/03_artifacts.md)                   | 各文件的写入规则与反模式          | 🟡 按需 |
| 迭代协议   | [modules/04_iteration-protocol.md](modules/04_iteration-protocol.md) | Bug 修复/回归/决策回溯/实验性代码 | 🟡 按需 |
| MARK 系统  | [modules/05_mark-system.md](modules/05_mark-system.md)               | 约束记录 + 防遗漏机制             | 🟡 按需 |

## 资源文件

| 文件                                                       | 用途                 |
| ---------------------------------------------------------- | -------------------- |
| [assets/project-skeleton.md](assets/project-skeleton.md)   | 新需求项目脚手架模板 |
| [assets/decision-template.md](assets/decision-template.md) | ADR 决策记录模板     |
| [assets/checklist.md](assets/checklist.md)                 | 各阶段自检清单       |

## 快速使用指南

```
新功能需求 → 加载本 skill → 按 02_workflow 阶段执行
Bug 修复   → 加载本 skill → 直接跳转 04_iteration-protocol
约束管理   → 加载本 skill → 参考 05_mark-system
```
