---
name: trade-outreach-email-for-openclaw
description: OpenClaw-native version of the foreign-trade outreach email skill. Use structured operator input plus public-context summaries to generate conservative first-touch or follow-up email drafts without overstating inferred facts.
openclaw_role: stage_worker
workspace_owner_skill: trade-active-outreach-combo
single_skill_policy: attach_only
feishu_container_creation: forbidden
requires_master_base: true
requires_master_record: true
---

# 开发信 Skill for OpenClaw

## Overview

这个版本面向 OpenClaw 云端工作流。

它假设操作员输入和公开资料摘要已经由上游节点整理好，然后由当前脚本进行保守合并，并调用根目录里的核心草稿生成器输出统一结构的邮件包。

## Inputs

最终输入应为：

```json
{
  "operator_input": {},
  "public_context": {}
}
```

## Feishu Runtime Contract

- 当前角色固定为 `stage_worker`
- 默认只允许附着到 `Trade Lead Workflow Hub`
- 只允许复用 `Outreach Email Docs`
- 不允许独立创建 Base、主表或平行工作容器
- 必须先查 `Lead Workflow Master`
- 已有开发信文档时，只追加草稿版本，不新建平行文档

## Output Requirements

- 输出结构与根目录版本一致
- 保留中文复核提示
- 生成标题候选、草稿正文和阶段 payload
- 不自动发送邮件
- 不重新做搜索或背调
