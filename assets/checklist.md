# 各阶段自检清单

## 阶段 1：需求分析

```markdown
- [ ] SPEC.md 已创建，包含概述、功能需求、验收标准
- [ ] SPEC 顶部有关键约束摘要（≤ 5 行 blockquote）
- [ ] 已与用户确认 SPEC 内容
- [ ] DESIGN_GUIDE.md 已创建（如有设计稿）
```

## 阶段 2：架构设计

```markdown
- [ ] PLAN.md 已创建，包含 Phase 划分 + 验收标准
- [ ] PLAN.md 大小 ≤ 3KB，不含实施细节
- [ ] 技术分歧已记录到 DECISIONS.md（如有）
- [ ] 每条 ADR 包含"考虑的替代方案"
```

## 阶段 3：代码实施

```markdown
- [ ] 已回顾 CLAUDE.md 确认约束
- [ ] 已按 code-generate 流程执行（如有该 skill）
- [ ] 代码遵循 Karpathy 准则（最小改动）
- [ ] changelog.md 已追加本次变更条目
- [ ] 索引表已同步更新
- [ ] 新发现的问题已记录到 TODO.md
```

## 阶段 4：质量验证

```markdown
- [ ] 已按 code-review 流程执行（如有该 skill）
- [ ] 审查发现的必修问题已修复
- [ ] 可后续优化的问题已记录 TODO.md
```

## 阶段 5：迭代修复（Bug 修复）

```markdown
- [ ] Bug 已复现，复现条件明确
- [ ] 根因已分析（≤ 5 行）
- [ ] 修复为最小改动，未顺手重构
- [ ] 回归验证已执行（相邻功能未受影响）
- [ ] changelog.md 已追加 Bug 条目
- [ ] 索引表已同步更新
```

## 跨模块修改附加检查

```markdown
- [ ] 变更影响评估已完成
- [ ] 所有调用方已验证
- [ ] DECISIONS.md 已确认不违反已有决策
```

---

## 产出物完整性验证

可执行的验证命令：

```bash
# 检查必要文件是否存在
ls SPEC.md CLAUDE.md PLAN.md changelog.md TODO.md

# 检查 changelog 是否有最新条目（当月）
head -20 changelog.md | grep "$(date +%Y-%m)"

# 检查 CLAUDE.md 的 MARK 数量（不应超过 20）
grep -c "^### MARK" CLAUDE.md

# 检查 PLAN.md 大小（不应超过 3KB）
wc -c PLAN.md

# 检查 changelog 索引表是否有内容
grep -c "^|" changelog.md
```
