# 项目生命周期与状态机 V0.1

## 1. 主流程

```text
PROJECT_CREATED
  ↓
COMMISSION_ACCEPTED
  ↓
PREPARATION
  ↓
PLAN_DRAFTING
  ↓
DOCUMENT_DRAFTING
  ↓
INTERNAL_REVIEW
  ↓
OWNER_CONFIRMATION
  ↓
NOTICE_PUBLISHED
  ↓
DOCUMENT_ACQUISITION
  ↓
Q_AND_A
  ↓
OPENING_PREPARATION
  ↓
BID_OPENING
  ↓
QUALIFICATION_REVIEW
  ↓
EVALUATION
  ↓
RESULT_CONFIRMATION
  ↓
CANDIDATE_PUBLICITY
  ↓
AWARD_ANNOUNCEMENT
  ↓
AWARD_NOTICE
  ↓
CONTRACT_AND_FOLLOWUP
  ↓
FILING
  ↓
SETTLEMENT
  ↓
ARCHIVING
  ↓
COMPLETED
```

不同采购方式可裁剪节点，但不得破坏历史可追溯性。

## 2. 关键异常事件

任何阶段均可根据规则触发：

- CORRECTION / 更正
- POSTPONEMENT / 延期
- SUSPENSION / 暂停
- TERMINATION / 终止
- OBJECTION / 异议
- COMPLAINT / 投诉处理
- WITHDRAWAL / 撤回
- FAILED_TENDER / 流标
- INVALID_TENDER / 废标
- RETENDER / 重新招标采购

异常事件不能只写在备注中，应形成独立业务记录。

## 3. TenderRound

重新招标或重新采购不得通过覆盖原项目历史实现。

```text
Project
├─ TenderRound 01
│  ├─ Notice
│  ├─ Bidders
│  ├─ Opening
│  ├─ Evaluation
│  └─ Result: FAILED
│
└─ TenderRound 02
   ├─ Notice
   ├─ Bidders
   ├─ Opening
   ├─ Evaluation
   └─ Result: AWARDED
```

Project 保持不变，批次记录每次采购活动的独立过程。

## 4. 状态迁移基本规则

每次状态迁移至少记录：

- from_state
- to_state
- operator
- timestamp
- reason
- related_documents
- approval / confirmation
- rule_version

关键状态不得直接“改字段”绕过迁移记录。

## 5. 节点完成条件

每个流程节点未来应支持：

- required_fields
- required_documents
- required_approvals
- deadline_rules
- validation_rules
- next_states

例如“进入开标”前，可以检查：

- 正式招标采购文件是否已发布
- 公告是否存在
- 截止时间是否配置
- 开标地点/方式是否明确
- 必备开标资料是否齐全

## 6. 地区与业务差异

工作流核心不把某一地区的具体天数、表格和程序要求硬编码。

地区规则、项目类型规则和采购方式差异应通过规则包扩展，并明确：

- 适用范围
- 来源
- 生效日期
- 失效日期
- 规则版本
