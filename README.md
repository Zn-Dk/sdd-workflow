# SDD Workflow — 规范驱动开发工作流

一个用于编排需求开发全生命周期的 **Agent Skill**，覆盖从需求分析到迭代修复的完整闭环。

## 简介

规范驱动开发（Spec-Driven Development）的核心理念：**SPEC 为第一准则**——需求方向调整必须先改 SPEC，再改代码。

本 Skill 作为编排层，不重复定义"怎么写代码"或"怎么做 review"，而是定义：

- **何时**触发哪个子 Skill
- **产出物**的管理规范（`_dev/` 目录结构）
- **迭代协议**（Bug 修复闭环、回归检测、防遗漏机制）
- **MARK 系统**（项目级约束管理）

## 核心执行约束

| 约束         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| 实时状态面板 | `_dev/` 文件是当前需求的状态面板，不是最后统一补写的归档材料 |
| 回合级同步   | 每轮结束前必须执行 dirty-state 判定                          |
| 零遗漏       | 存在未同步 dirty 产物时，不得以"已完成"结束本轮              |
| 恢复协议     | 切换焦点后回到原需求，必须执行最小恢复检查                   |

## 目录结构

```
sdd-workflow/
├── SKILL.md                              # 入口文件（总览 + 索引 + Agent 启动协议）
├── modules/                              # 分层文档（按需加载）
│   ├── 01_philosophy.md                  # 核心理念 + 文件职责矩阵
│   ├── 02_workflow.md                    # 阶段编排 + 回顾检查点 + 回合级同步协议
│   ├── 03_artifacts.md                   # 产出物规范（各文件写入规则与反模式）
│   ├── 04_iteration-protocol.md          # 迭代协议（Bug 修复/回归/回溯/实验性代码）
│   └── 05_mark-system.md                 # MARK 约束管理与防遗漏机制
├── assets/                               # 资源文件
│   ├── project-skeleton.md               # 新需求项目脚手架模板
│   ├── decision-template.md              # ADR 决策记录模板
│   └── checklist.md                      # 各阶段自检清单
├── README.md
└── LICENSE
```

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

| 章节       | 文件                               | 职责                                     | 何时加载                     |
| ---------- | ---------------------------------- | ---------------------------------------- | ---------------------------- |
| 核心理念   | `modules/01_philosophy.md`         | SDD 原则与文件职责矩阵                   | 首次加载本 skill 时          |
| 开发工作流 | `modules/02_workflow.md`           | 阶段编排、回顾检查点与回合级同步协议     | 进入常规开发流程时           |
| 产出物规范 | `modules/03_artifacts.md`          | 各文件的写入规则与反模式                 | 判断 `_dev/` 文件怎么写时    |
| 迭代协议   | `modules/04_iteration-protocol.md` | Bug 修复、回归、决策回溯与实验性代码约束 | 处理 Bug / 回归 / 实验代码时 |
| MARK 系统  | `modules/05_mark-system.md`        | 约束记录与防遗漏机制                     | 管理项目级约束时             |

## Agent 启动协议

Skill 加载后，Agent 按以下决策树确定入口：

1. **确定实施目录**（DEV_DIR）— 已有则复用，否则询问用户
2. **检查 DEV_DIR 是否已存在** — 已有则恢复上下文，无则初始化
3. **确定需求规模** — 决定走完整流程还是简化流程
4. **输出启动确认** — 📋 已加载 SDD 工作流，DEV_DIR：[路径]，当前状态：[新建/继续]，进入阶段：[N]

## 快速使用指南

```
新功能需求 → 加载本 skill → 按 02_workflow 阶段执行
Bug 修复   → 加载本 skill → 直接跳转 04_iteration-protocol
约束管理   → 加载本 skill → 参考 05_mark-system
```

## 适用场景

- 🆕 新功能需求开发（完整五阶段）
- 🐛 Bug 修复（直接跳转迭代协议）
- 🔄 需求迭代（SPEC 变更 → 代码跟进）
- 📌 约束管理（MARK 系统防遗漏）

## 许可证

本项目基于 [MIT License](LICENSE) 开源。
