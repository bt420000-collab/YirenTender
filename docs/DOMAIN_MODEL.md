# 核心领域模型 V0.1

## 1. 顶层聚合

```text
Project
├─ TenderRound
│  ├─ WorkflowInstance
│  ├─ Notice
│  ├─ Bidder / Supplier
│  ├─ Opening
│  ├─ Evaluation
│  └─ Award
├─ Documents
├─ Tasks
├─ Organizations
├─ People
├─ Approvals
├─ FinanceRecords
├─ Archive
└─ AuditLog
```

## 2. 首批核心实体

| 实体 | 作用 |
|---|---|
| Project | 项目主体 |
| TenderRound | 招标/采购批次 |
| WorkflowInstance | 某项目某批次的流程实例 |
| WorkflowNode | 流程节点定义 |
| Transition | 状态迁移 |
| Task | 待办 |
| Organization | 招标人、采购人、投标人、供应商、监管单位等 |
| Contact | 联系人 |
| Bidder | 投标人/供应商参与记录 |
| Expert | 评审专家基础记录 |
| Document | 文档逻辑对象 |
| DocumentVersion | 文档版本 |
| DocumentTemplate | 文书模板 |
| Approval | 审批/确认 |
| Notice | 公告、公示、更正等 |
| Meeting | 开标、评审会议 |
| Evaluation | 评审过程与结果 |
| Award | 中标/成交结果 |
| Contract | 合同及后续资料 |
| FinanceRecord | 费用记录 |
| ArchiveItem | 档案项 |
| AuditEvent | 审计事件 |
| RulePack | 地区/业务规则包 |

## 3. 文档版本原则

Document 是“这是什么文件”，DocumentVersion 是“这个文件的第几个版本”。

```text
招标文件
├─ V1 初稿
├─ V2 内审稿
├─ V3 招标人反馈稿
├─ V4 正式发布稿 [LOCKED]
└─ V5 更正版
```

正式发布版本不得被覆盖。

## 4. 组织与角色

用户账号、组织身份和项目角色分离。

同一自然人可以：

- 属于某机构
- 在某项目担任负责人
- 在另一项目担任审核人
- 在第三项目仅有只读权限

## 5. 审计事件

建议采用 append-oriented 模型。

关键字段包括：

- actor_id
- project_id
- tender_round_id
- event_type
- entity_type
- entity_id
- before
- after
- reason
- timestamp
- source_ip / client
- related_document_ids

## 6. 删除策略

核心业务数据默认不物理删除。

采用：

- active / inactive
- deleted_at
- voided
- superseded
- revoked

等状态保存历史。

真正的物理删除仅用于明确允许删除的临时数据，并遵循后续数据治理规则。
