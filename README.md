# Trade Outreach Email

把客户基础信息、客户画像摘要和跟进阶段，转成可人工修改后发送的外贸英文邮件草稿。

An open-source Codex skill for turning structured sales context into conservative English outreach drafts for first touch and follow-up scenarios.

当前状态：可交付

角色定位：`开发信策略员`

链路角色：

- 在总链路里是 `stage_worker`
- 组合包 / 主代理是 `workflow_owner`
- 单节点默认 `attach_only`
- `feishu_container_creation = forbidden`
- 单节点不独立声明飞书工作容器
- 所有数据最终统一挂到同一个 `Trade Lead Workflow Hub`

上下游关系：

- 上游：[trade-customer-intel](https://github.com/FloydTang/trade-customer-intel) 或人工整理输入
- 下游：人工复核与发送动作

## 公开最小可用说明

这个仓库公开层只解决一个问题：

- 把上游结构化信息转成可人工复核的英文开发信草稿
- 和组合包一样，这个单节点仓库本身就拥有可独立执行的最小功能

当前最小能力：

- 输入 `first_touch` 或 `follow_up`
- 输出英文标题候选、英文正文草稿、中文复核提示
- 支持从客户背调报告桥接成开发信输入
- 默认本地运行，不依赖联网

## 飞书增强入口

如果你要把这个节点接进龙虾 / OpenClaw 多代理链路，优先不要先看长教程，直接复制增强执行词给龙虾：

- [飞书增强入口：复制增强执行词给龙虾](https://evenbetter.feishu.cn/wiki/ADmiwiultihx6Yk1p2UcjfmVn6d)

如果链接打开失败，请使用登记在半斤九两群里的飞书账号打开。

如果你拿到的是半斤九两科技沟通过的执行包用户链接，也可以优先使用那个链接打开。

如果你当前还没有半斤九两科技的账号，需要联系半斤九两科技，请访问：[evenbetter.tech](https://evenbetter.tech)

仓库内对应的源码基线在：

- `references/00-单节点增强执行词.md`
- `for-openclaw/README.md`
- `for-openclaw/SKILL.md`

## 推荐模型

- `coze/doubao-seed-2-0-lite-260215`

## Quick Start

```bash
python3 ./scripts/build_email_draft.py \
  --input-json ./examples/first-touch.json \
  --markdown-out /tmp/first-touch-email.md \
  --json-out /tmp/first-touch-email.json
```

```bash
python3 ./scripts/build_email_draft.py \
  --input-json ./examples/follow-up.json
```

```bash
python3 ./scripts/build_email_input_from_customer_intel.py \
  --input-json ./examples/customer-intel-report.json \
  --email-type first_touch \
  --product-or-offer "frozen mixed vegetables" \
  --sender-name "Leo" \
  --sender-company "Ningbo FreshGrow Foods" \
  --json-out /tmp/email-input-from-intel.json
```

```bash
python3 ./scripts/run_regression_checks.py
```

```bash
python3 ./scripts/run_pre_release_gate.py
```

## Feishu / OpenClaw Stage Export

如果你只想单独使用开发信 Skill，也建议把结果并入统一主表。

在生成 `email input` 和 `email draft json` 后，再运行：

```bash
python3 ./scripts/build_feishu_stage_payload.py \
  --email-input-json /tmp/email-input-from-intel.json \
  --email-output-json /tmp/first-touch-email.json \
  --combo-run-id manual-run \
  --lead-id lead-001
```

这个脚本不会重新生成邮件草稿。

它只负责把已有邮件结果转成 OpenClaw 可消费的文档 payload，用于：

- 创建或更新开发信云文档
- 回写 `Lead Workflow Master`
- 支持“外部 lead 直接跑邮件草稿”的单点接入方式

## Chain Position

推荐链路：

`trade-lead-discovery -> trade-lead-screening -> trade-customer-intel -> trade-outreach-email`

关联仓库：

- 客户搜索 Skill: [trade-lead-discovery](https://github.com/FloydTang/trade-lead-discovery)
- 线索整理 Skill: [trade-lead-screening](https://github.com/FloydTang/trade-lead-screening)
- 客户背调 Skill: [trade-customer-intel](https://github.com/FloydTang/trade-customer-intel)

## Agent-First 增强价值

会员增强层当前不是改业务逻辑，而是补这几件事：

- 单节点在龙虾里有明确的 `stage_worker` 角色
- 单节点默认只 attach，不独立建飞书工作容器
- 飞书里提供可直接复制给龙虾的增强执行词
- 与总编排链路保持同一套文档复用、失败回报和协作口径

## Repository Structure

```text
.
├── README.md
├── SKILL.md
├── 立项方案.md
├── 验收记录.md
├── scripts/
│   ├── build_email_draft.py
│   ├── build_email_input_from_customer_intel.py
│   ├── run_regression_checks.py
│   └── run_pre_release_gate.py
├── examples/
├── references/
│   ├── 00-单节点增强执行词.md
│   ├── customer-intel-integration.md
│   ├── input-fields.md
│   ├── output-template.md
│   └── review-rules.md
├── schemas/
└── for-openclaw/
```

## OpenClaw Variant

`for-openclaw/` 提供和总仓一致口径的单节点 OpenClaw 包装版本：

- 角色固定为 `stage_worker`
- 默认只允许 attach 到 `Trade Lead Workflow Hub`
- 不允许独立创建 Base、主表或平行工作容器

## 作者

半斤九两科技
