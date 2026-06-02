# SDD Workflow — 规范驱动开发工作流

一个用于编排需求开发全生命周期的 **Agent Skill**，覆盖从需求分析到迭代修复的完整闭环。

## 简介

规范驱动开发（Spec-Driven Development）的核心理念：**先明确"做什么"，再决定"怎么做"**。

本 Skill 作为编排层，定义了：

- **五阶段工作流** — 需求分析 → 架构设计 → 代码实施 → 质量验证 → 迭代修复
- **产出物管理** — `_dev/` 目录结构规范（SPEC / PLAN / DECISIONS / changelog 等）
- **迭代协议** — Bug 修复闭环、回归检测、决策回溯、实验性代码管理
- **MARK 系统** — 项目级约束管理与防遗漏机制

## 核心设计原则

| 原则            | 说明                                |
| --------------- | ----------------------------------- |
| SPEC 为第一准则 | 需求方向调整必须先改 SPEC，再改代码 |
| 变更必留痕      | 每次迭代记录 changelog，含根因分析  |
| 外科手术式修改  | 最小改动、只改必须改的、不顺手重构  |
| 验证闭环        | 修复后必须验证不引入回归            |
| 渐进式实施      | 分阶段交付，每阶段有明确验收标准    |

## 目录结构

```
sdd-workflow/
├── SKILL.md                              # 入口文件（总览 + 索引）
├── modules/                              # 分层文档（按需加载）
│   ├── 01_philosophy.md                  # 核心理念 + 文件职责矩阵
│   ├── 02_workflow.md                    # 五阶段编排 + 回顾检查点
│   ├── 03_artifacts.md                   # 产出物规范（各文件写入规则）
│   ├── 04_iteration-protocol.md          # 迭代协议（Bug 修复/回归/回溯）
│   └── 05_mark-system.md                 # MARK 约束管理系统
├── assets/                               # 资源文件
│   ├── project-skeleton.md               # 新需求项目脚手架模板
│   ├── decision-template.md              # ADR 决策记录模板
│   └── checklist.md                      # 各阶段自检清单
├── README.md
└── LICENSE
```

## 阶段 → 子 Skill 映射

| 阶段     | 产出物                   | 引用的子 Skill          |
| -------- | ------------------------ | ----------------------- |
| 需求分析 | SPEC.md, DESIGN_GUIDE.md | `spec-writing`          |
| 架构设计 | DECISIONS.md, PLAN.md    | — (自身规范)            |
| 代码实施 | 源代码 + changelog       | `code-generate`（如有） |
| 质量验证 | Review 报告              | `code-review`（如有）   |
| Bug 修复 | changelog + DECISIONS    | 自身迭代协议            |
| 约束管理 | CLAUDE.md                | 自身 MARK 系统          |

## 快速使用指南

```
新功能需求 → 加载本 skill → 按 02_workflow 阶段执行
Bug 修复   → 加载本 skill → 直接跳转 04_iteration-protocol
约束管理   → 加载本 skill → 参考 05_mark-system
```

## 使用方式

### 作为 Agent Skill 加载

将本目录放置到你的 Agent Skill 目录中（如 `.codebuddy/skills/`），Agent 会自动识别 `SKILL.md` 作为入口文件。

### 查阅路径（推荐）

1. **SKILL.md** — 了解总览和阶段映射
2. **modules/01 + 02** — 必读（核心理念 + 工作流编排）
3. **modules/03~05** — 按需深入（产出物规范、迭代协议、MARK 系统）
4. **assets/** — 实操时使用模板和清单

## 适用场景

- 🆕 新功能需求开发（完整五阶段）
- 🐛 Bug 修复（直接跳转迭代协议）
- 🔄 需求迭代（SPEC 变更 → 代码跟进）
- 📌 约束管理（MARK 系统防遗漏）

## 许可证

本项目基于 [MIT License](LICENSE) 开源。
