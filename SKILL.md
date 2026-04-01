---
name: trade-outreach-email
description: Generate conservative first-touch or follow-up foreign-trade outreach emails from structured operator input and customer-intel summaries. Use when a sales operator needs editable English email drafts, subject options, and Chinese review notes without overstating inferred facts.
---

# 开发信 Skill

## Overview

用这个 Skill 把客户基础信息、客户画像摘要和跟进阶段，转成可人工修改后发送的外贸英文邮件草稿。

角色定位：

- `开发信策略员`
- 负责把上游结构化信息转成可编辑英文草稿
- 不负责自动发送
- 不负责替代搜索、初筛和背调

## Chain Role

- 在总链路中固定作为 `stage_worker`
- 默认单节点策略：`attach_only`
- 默认不独立声明飞书工作容器
- 所有数据最终统一挂到 `Trade Lead Workflow Hub`

## Agent-First Installation Notes

这个仓库默认提供两层说明：

- 公开层：根目录 `README.md`，保证最小可用
- 增强层：`for-openclaw/README.md` 和 `references/00-单节点增强执行词.md`

如果你要在龙虾里使用这个节点，优先复制增强执行词给龙虾，而不是先看教程型长文。

## Inputs

最终输入统一为结构化业务上下文，至少包括：

- `email_type`
- `product_or_offer`
- `goal`
- `sender_name`
- `sender_company`

如果上游来自客户背调，还可以先用桥接脚本生成标准输入。

## Output Requirements

- 必须输出标题候选
- 必须输出英文邮件草稿
- 必须输出中文复核提示
- 不能自动发送邮件
- 不能改写上游已经明确的事实
- OpenClaw 单节点默认只 attach，不单独建表

## Main Scripts

- [build_email_draft.py](./scripts/build_email_draft.py)
- [build_email_input_from_customer_intel.py](./scripts/build_email_input_from_customer_intel.py)

### Example

```bash
python3 ./scripts/build_email_draft.py --input-json ./examples/first-touch.json
```

```bash
python3 ./scripts/build_email_input_from_customer_intel.py --input-json ./examples/customer-intel-report.json --email-type first_touch
```

## References

- [00-单节点增强执行词.md](./references/00-单节点增强执行词.md)
- [for-openclaw/README.md](./for-openclaw/README.md)
- [for-openclaw/SKILL.md](./for-openclaw/SKILL.md)
