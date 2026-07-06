# 各阶段自检清单

## 启动确认

```markdown
- [ ] 已确定实施目录（用户指定 / 从上下文推断 / 主动询问用户）
- [ ] 已按 SKILL.md 启动协议输出启动确认信息（状态 + 进入阶段 + DEV_DIR）
```

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
- [ ] 已调用 code-generate 流程
- [ ] 代码遵循 Karpathy 准则（最小改动）
- [ ] changelog.md 已追加本次变更条目
- [ ] 索引表已同步更新
- [ ] 新发现的问题已记录到 TODO.md
```

## 阶段 4：质量验证

```markdown
- [ ] 已调用 code-review 流程
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

## 会话持久化检查（中大需求 / 多轮迭代 / 跨会话场景）

> 仅适用于中大需求或多轮迭代场景，小需求可跳过。参见 [06_session-persistence.md](../modules/06_session-persistence.md)。

```markdown
- [ ] 已创建 findings.md（如有调研/源码阅读阶段）
- [ ] 已创建 progress.md（如为多轮迭代或跨会话场景）
- [ ] 每完成 2 次读取/浏览操作后，关键发现已写入 findings.md（2-Action Rule）
- [ ] 重大决策前已读取 findings.md + DECISIONS.md + SPEC.md（Read Before Decide）
- [ ] 每轮结束前已更新 progress.md（Write After Act）
- [ ] 跨会话恢复时已执行 4 步恢复流程（git diff → 会话级文件 → _dev/ 产出物 → 对比修正）
- [ ] 会话级文件未被当作产出物提交（应 .gitignore 或标注临时性）
- [ ] 遇到错误时未重复相同的失败动作（3-Strike Protocol）
```

---

## 产出物完整性验证

可执行的验证命令（需先设置 `DEV_DIR` 为当前需求的 `_dev/` 目录路径）：

```bash
# 设置 _dev 目录路径（启动协议步骤 0 确定的实际路径）
DEV_DIR="<启动协议确定的实施目录路径>"

# 检查必要文件是否存在
ls "$DEV_DIR"/{SPEC.md,CLAUDE.md,PLAN.md,changelog.md,TODO.md}

# 检查 changelog 是否有最新条目（当月）
head -20 "$DEV_DIR/changelog.md" | grep "$(date +%Y-%m)"

# 检查 CLAUDE.md 的 MARK 数量（不应超过 20）
grep -c "^### MARK" "$DEV_DIR/CLAUDE.md"

# 检查 PLAN.md 大小（不应超过 3KB）
wc -c "$DEV_DIR/PLAN.md"

# 检查 changelog 索引表是否有内容
grep -c "^|" "$DEV_DIR/changelog.md"
```
