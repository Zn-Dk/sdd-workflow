# ADR 决策记录模板

Architecture Decision Record（ADR）格式，用于记录关键技术决策。

---

## 使用方式

在 DECISIONS.md 中追加新条目时，复制以下模板并填写。

---

## 模板

```markdown
### ADR #[编号]: [决策标题]

**日期**：[YYYY-MM-DD]
**状态**：已采纳 | [SUPERSEDED] 被 ADR #X 取代

**背景**：
[1-2 句描述面临的问题或选择]

**决策**：
[选择了什么方案]

**理由**：
[为什么选这个方案（≤ 3 点）]

**考虑的替代方案**：
1. [方案 B] — 不选的原因：[简述]
2. [方案 C] — 不选的原因：[简述]

**影响**：
- [对代码/架构/性能的影响]

**关联**：
- [相关的 Bug/MARK/SPEC 章节，可选]
```

---

## 示例

```markdown
### ADR #1: 并发节点不使用 autoResize

**日期**：2025-03-15
**状态**：已采纳

**背景**：
并发节点的分支包围框需要动态计算大小。VueFlow 提供了 autoResize 选项，但在嵌套场景下表现异常。

**决策**：
手动计算包围框尺寸，不使用 VueFlow 的 autoResize。

**理由**：
1. autoResize 在嵌套模板场景下会导致无限循环更新
2. 手动计算可以精确控制时机，避免不必要的重排
3. 与 collapsible 功能冲突

**考虑的替代方案**：
1. 使用 autoResize + debounce — 不选：仍有循环风险，且 debounce 导致视觉跳动
2. 使用 ResizeObserver — 不选：无法控制计算时机，性能开销大

**影响**：
- 需要在 appendTemplateNodes 和 combineCanvas 中手动维护 BBox 计算逻辑

**关联**：
- Bug 23: autoResize 导致的无限循环
- MARK #4: 不使用 autoResize
```

---

## 编写规则

1. 每条决策 ≤ 500 字
2. 必须包含"考虑的替代方案"（至少 1 个）
3. 废弃时标记 `[SUPERSEDED]`，不删除原文
4. 编号递增，不复用
