# ODRAF → Jury → Healthcare_Safety_Net Decision Interface
Version: v1.1 (Final · Repo-ready)

## 1. 文件目的（Purpose）

本文件定義當案件被移交至 Jury（人類陪審團） 時，
系統必須生成的 標準化決策封包（Decision Packet）。

設計目標：

資訊對等：所有陪審員看到相同的去識別化事實

程序正義：避免隱性偏見與資訊不對稱

可執行性：Jury 判決可直接轉化為系統指令

責任封存：每一項裁定可追溯、可審計、不可否認

Decision Packet 不是說服工具，
而是 承擔責任的載體。

## 2. 封包生成時機（Packet Generation Triggers）

當任一條件成立時，系統 必須立即中止自動化流程 並生成 Decision Packet：

2.1 ODRAF 熔斷（Fuse）

OR-1：Opioid Red-Flag

VD-1：Verification Hold（如 UDS pending）

2.2 Scarcity Traffic Light（STL）

STL = RED

且資源屬於：

控制性藥物

生命關鍵物資

高外溢風險資源

2.3 人類上訴觸發

對 dignity_multiplier

或 access / coverage 判定提出異議

## 3. 資訊對等與去識別原則（Information Parity & De-identification）
3.1 必須提供（Mandatory Fields）

正規化後的臨床分數（clinical_priority）

系統性風險分數（systemic_risk_score）

尊嚴影響分數（dignity_impact_score）

稀缺狀態（stl_status）

稀缺原因摘要（scarcity_reason）

允許 / 禁止的行動空間（allowed_action_space）

3.2 禁止提供（Prohibited Fields）

姓名、照片、精確地址

種族、宗教、政治立場

收入、資產、社會地位標籤

任何可回推個人身分的識別碼

## 4. Jury Decision Packet Schema（標準結構）

{
  "packet_id": "JURY-PKT-2026-RED-001",
  "trigger_source": "ODRAF_Risk_Weights_v1.0",
  "case_severity": "CRITICAL",

  "jury_composition": {
    "required_jurors": 7,
    "role_distribution": [
      "Clinical_Specialist",
      "Medical_Ethics",
      "Legal_Audit",
      "Community_Rep",
      "LDPC_Beneficiary"
    ]
  },

  "evidence_summary": {
    "clinical_priority": 0.41,
    "systemic_risk_score": 0.82,
    "dignity_impact_score": 0.65,
    "stl_status": "RED",
    "scarcity_reason": "Regional inventory ratio < 0.10"
  },

  "allowed_action_space": {
    "allowed_resources": [
      "NON_OPIOID_THERAPY",
      "PHYSICAL_THERAPY_VOUCHER"
    ],
    "disallowed_resources": [
      "OPIOID_DIRECT_DISPENSE"
    ]
  },

  "voting_threshold": "SUPER_MAJORITY_75",
  "status": "AWAITING_VERDICT",

  "governance_timeout": {
    "max_wait_hours": 72,
    "timeout_action": "DEFAULT_TO_NON_OPIOID_PATHWAY",
    "rationale": "Fail-safe to prevent harm from procedural delay. Applies lowest abuse risk and highest reversibility without pre-empting Jury authority."
  }
}

## 5. 封包與 Voting Protocol 的關係（Packet ↔ Voting）

jury_composition
→ 必須符合 Jury_Roles.md 的角色不可互換原則

voting_threshold
→ 由 Voting_Protocol.md 依風險層級自動指定

allowed_action_space
→ 限縮陪審團可裁定的選項集合，防越權裁決

Jury 只能在「被允許的行動空間」內行使主權。

## 6. 判決回傳與系統執行（Verdict → Execution）
6.1 Healthcare_Safety_Net

依 approved_resources / rejected_resources 執行

啟動 mandatory_conditions（如行為支持、定期追蹤）

6.2 ODRAF

封存 scoring trace

記錄是否存在 dignity override 或人類修正

6.3 Audit Layer

將 Packet + Verdict 寫入不可變 Ledger

標記責任承擔與時間戳

6.4 Governance Timeout（補充）

若達 max_wait_hours 未形成裁定：

自動套用 timeout_action

不視為最終裁定

Jury 仍保有後續覆核與修正權（retroactive review）

## 7. 程序正義聲明（Procedural Justice Statement）

Decision Packet 的存在，是為了確保：

沒有人可以躲在模型後面

沒有人可以說「只是照系統算的」

沒有人可以在傷害發生後否認責任

當系統傷害人時，
必須能指出：
是誰，依據什麼資訊，
做出了這個判斷。

## 8. 文件關聯與不可變條款（Immutability Clause）

依賴文件：

Jury_Roles.md

Voting_Protocol.md

Risk_Weights.md

Healthcare_Safety_Net.md

不可變條款：
本文件一經發布，不得在不經 Jury 系統本身裁定的情況下被弱化、繞過或靜默修改。

文件狀態： FINAL (v1.1)
Repo 建議路徑： modules/jury/Jury_Decision_Packet.md
