# Healthcare_Safety_Net

Draft v1.0 (Regenerated)

Metadata

Module: Healthcare_Safety_Net

Layer: LDPC / Basic Universal Services (UBS-aligned)

Depends On: modules/odraf/ODRAF_Core.md, modules/jury/, specs/03_Governance.md

Purpose: Maintain life-dignity floor during AI-induced displacement and systemic transition.

# 1. 核心願景（Core Vision）

本模組落實 LORI（Life Dignity） 的底線原則：
在 AI / 自動化造成的大規模失業、社會轉型或制度斷裂期間，任何個體的 生命健康權 不得因支付能力消失而崩潰。

本模組的定位：

Healthcare Safety Net 是 LDPC 的「生存必須服務」接管層

對應 UBS（Universal Basic Services）精神

保障對象是「生命」而非「勞動身份」

醫療保障是尊嚴底線（Dignity Floor），不是商品，也不是績效獎勵。

# 2. 系統邊界與基本原則（Boundaries & Principles）
2.1 臨床排序與補償排序必須分離

Clinical Triage（臨床先後）：只依據臨床急迫性、不可逆性與證據等級

Access / Coverage（可近性與支付）：由 LDPC 補償處理（包含 AI_displaced 等結構因素）

硬規則：

employment_status = AI_displaced 不得直接提高臨床優先權

僅可用於：保險缺口補位、服務可近性擴張、交通/轉診支援、費用覆蓋

2.2 反市場暴力原則

本模組拒絕「價高者得」的資源分配方式。稀缺資源不得用競價決定。

2.3 反算法專政原則

當涉及稀缺爭議或高風險控制性藥物，本模組禁止 AI 單獨裁定，必須引入 Jury 裁定流程。

# 3. 觸發條件（Trigger Conditions）

Healthcare Safety Net 會在以下情境自動啟動或升級：

AI_Induced_Unemployment：因 AI / 自動化失業造成健保/保險中斷

Coverage_Gap：保險缺口超過閾值（例如 30 天）

System_Shock：疫情、災害、戰爭、供應鏈斷裂造成服務不可用

High_Risk_Request：成癮風險與控制性藥物請求的紅旗組合（見第 6 節）

# 4. 資源分配機制（Resource Allocation Logic）

本模組採用 ODRAF 權重 進行動態分配，但必須遵守「臨床排序與補償排序分離」原則。

4.1 優先級演算指標（Priority Dimensions）

臨床層（Clinical）

Urgency Index：急性生命威脅／急迫程度

Irreversibility Index：延遲造成不可逆傷害的程度

Evidence Grade：證據等級（A/B/C/D）

尊嚴可近性層（Access / Dignity）

Dignity Multiplier：只用於「覆蓋與可近性補償」，不得覆寫 triage

Prevention Weight：預防性干預權重（成癮預防、防疫、心理支持）

4.2 指標一致性檢查（Consistency Check）

避免 Grok 指出的「Urgency 高但 risk_level 卻 medium」的衝突，本模組加入一致性檢查。

硬規則：

若 urgency_index >= 0.75，系統不得將 health_risk_level 標為 low/medium

必須：升級風險等級，或標記 consistency_fail 並要求補齊臨床欄位

# 5. 質與量控管（Quality & Quantity Control）
5.1 質量監控（Quality Assurance）

列入補償清單的藥物與療法需通過 開源臨床數據審核

必須標注：

evidence_grade

known_risk_flags

conflict_of_interest_notes（若有）

5.2 按需定製（Demand-Based Allocation）

與 simulations/ 連動

用區域性需求預測降低：

缺藥風險

物流延誤

庫存浪費

不主張「無限供給」，而是 動態調度與可審計採購

# 6. 藥物濫用與風險防護欄（Substance Abuse Guardrails）

本模組承認：成癮是健康風險狀態，不是道德缺陷，因此介入機制必須 非懲罰性，但也必須 足夠嚴格。

6.1 風險分級（Risk Levels）

low：無高風險跡象

medium：有既往史或風險因子

high：已出現明確濫用/復發/過量等紅旗

6.2 紅旗組合（Hard Red-Flag Rule）

硬規則 GR-2：

若 addiction_risk_level in {medium, high} 且 requested_resource.resource_code == OPIOID_CLASS
→ 禁止 Direct Opioid Dispense（不得直接配發）
→ 必須強制進入 Non_Opioid_First 路徑

允許方案（allowed_options）：

非鴉片止痛藥

物理治療

心理/行為支持（CBT / addiction prevention）

專科複審（specialist review）

6.3 補償形式對照表（Non-Punitive）
風險狀態	補償形式	介入措施
Low	全額服務/藥物選擇權	定期健康追蹤
Medium	定向醫療券（Vouchers）	限制特定藥物；優先替代療法；短週期 review
High	康復服務包（Service Package）	暫停現金/高風險藥物；戒斷支援；專人照護

限制的目的不是控制人，而是讓人退出不可逆風險，回到可選擇狀態。

# 7. 尊嚴補償係數（Dignity Multiplier）透明規則

Grok 指出 dignity_multiplier 若無公開定義會被質疑不透明或歧視。
因此本模組規定：

硬規則 GR-3：

dignity_multiplier 必須包含：

formula_id

bounds（上下限）

explanation_short（短說明）

限制：

dignity_multiplier 不得影響臨床 triage

只能用於：

覆蓋範圍增補

可近性服務（轉診、交通、基礎門診）

支付缺口補位

# 8. 決策裁定（Jury-Based Judgment）
8.1 觸發條件（何時必須 Jury）

以下任一情況成立 → 必須觸發 Jury：

稀缺資源爭議：例如最後一份特效藥

高風險控制藥物請求：例如成癮風險下仍申請 opioid

一致性檢查失敗：臨床指標衝突或資料缺失但仍要求高優先

公平性爭議：dignity_multiplier 或 eligibility 被提出異議

8.2 陪審流程（不可 AI 單點決策）

調用 modules/jury/ 的合格角色與投票協議

依據 modules/odraf/ODRAF_Core.md 的後果預演資料投票

產出 Verdict 必須包含：

result

constraints

review_cycle_days

appeal_window_days

voucher_spec（若採券制）

8.3 記錄封存（Audit & Ledger）

裁定過程與摘要必須封存至 specs/03_Governance.md 指定的公開帳本（可去識別）

禁止將 PII 寫入公開帳本

# 9. Audit-Fail Conditions（審計不通過條款）

以下任一條成立 → 判定為 Audit Fail（不得執行或必須回退）：

缺少 Minimum Clinical Dataset（第 10 節）

dignity_multiplier 無公式 ID 或無上下限

employment_status 影響 clinical triage

addiction_risk_level != low 仍允許 direct opioid dispense

Verdict 未提供 voucher_spec / constraints / review cycle / appeal window

# 10. 最小臨床資料集（Minimum Clinical Dataset, MCD）

為避免「資料不足卻做高風險決策」，本模組要求至少提供：

age_band（去識別）

pain_duration_days 或症狀持續時間

prior_treatments（既往治療）

mental_health_risk

substance_use_history

pdmp_check（處方監測）

uds_result（尿檢或其他必要檢測的狀態）

## 11. 數據交互範例（Data Schema Interface）

本事件封包為 Healthcare Safety Net 在
AI 誘發失業 × 成癮風險 × 控制性藥物請求
情境下的標準可審計輸出範例。

本 JSON 為 嚴格格式（無註解），可直接用於：

schema 驗證

模擬（simulations/）

ledger 封存

Jury case packet

# A.1 Healthcare Safety Net — Event Packet (JSON)

 ```json
{
  "module": "Healthcare_Safety_Net",
  "version": "v1.0-rev1",
  "status": "Active",
  "trigger_condition": "AI_Induced_Unemployment",
  "safety_net_tier": "Basic_Universal_Services",
  "abuse_guardrail_active": true,

  "user_profile": {
    "user_id": "anon_7f92",
    "region": "US-CA",
    "employment_status": "AI_displaced",
    "eligibility": {
      "ldpc_covered": true,
      "coverage_scope": "healthcare_access_only"
    }
  },

  "minimum_clinical_dataset": {
    "age_band": "26-30",
    "pain_duration_days": 120,
    "prior_treatments": [
      "Nonsteroidal Anti-Inflammatory Drugs",
      "Physical Therapy"
    ],
    "mental_health_risk": "medium",
    "substance_use_history": "past",
    "pdmp_check": "completed",
    "uds_result": "pending"
  },

  "healthcare_request": {
    "request_id": "HSN-REQ-2026-00031",
    "condition": {
      "type": "chronic_pain",
      "severity_score": 0.61,
      "urgency_index": 0.41,
      "irreversibility_index": 0.22
    },
    "requested_resource": {
      "category": "medication",
      "resource_code": "OPIOID_CLASS",
      "alternatives_available": true
    }
  },

  "risk_flags": {
    "addiction_risk_level": "medium",
    "opioid_request_red_flag": true
  },

  "odraf_scoring": {
    "odraf_profile_id": "ODRAF-HP-2026-1190",
    "evidence_grade": "B",
    "computed": {
      "clinical_priority_score": 0.41,
      "access_compensation_score": 0.63,
      "dignity_multiplier": 1.3,
      "dignity_multiplier_formula_id": "LDPC-DM-001",
      "dignity_multiplier_bounds_ref": "LDPC_CONFIG.DIGNITY_MULTIPLIER_MAX",
      "explanation_short": "AI displacement affects access and coverage only, not clinical triage. Multiplier does not influence clinical_priority_score."
    }
  },

  "guardrail_decision": {
    "rule_hits": [
      "GR-2_ADD_RISK_PLUS_OPIOID",
      "GR-4_EMPLOYMENT_NOT_TRIAGE",
      "GR-5_UDS_PENDING_HIGH_RISK"
    ],
    "forced_pathway": "Non_Opioid_First",
    "allowed_options": [
      "Non_Opioid_Medication",
      "Physical_Therapy",
      "Behavioral_Support",
      "Specialist_Review"
    ],
    "disallowed_options": [
      "Direct_Opioid_Dispense"
    ],
    "pending_conditions": [
      {
        "condition": "UDS_pending",
        "action": "Hold_Verification",
        "required_update": "uds_result must be positive, negative, or inconclusive before proceeding"
      }
    ]
  },

  "jury_trigger": {
    "required": true,
    "reason": "High_Risk_Controlled_Substance_Request",
    "packet_id": "JURY-HSN-2026-0042"
  },

  "jury_verdict": {
    "verdict_id": "VERDICT-2026-0042",
    "result": "Non_Opioid_Voucher_Only",
    "voucher_spec": {
      "scope": [
        "Physical_Therapy",
        "Non_Opioid_Pain_Management"
      ],
      "opioid_allowed": false
    },
    "constraints": {
      "review_cycle_days": 14,
      "mandatory_support": {
        "type": "Behavioral_Support",
        "description": "Mandatory psychological or counseling intervention during high-risk period"
      }
    },
    "appeal": {
      "allowed": true,
      "window_days": 7,
      "pause_review_during_appeal": true
    }
  },

  "audit_record": {
    "ledger_id": "HSN-LEDGER-2026-000884",
    "pii_stored": false,
    "anonymization_level": "high"
  }
}

 ```

-----

## A.2 補充說明（Design Clarifications）

mandatory_support
已改為結構化物件，明確指定為 Behavioral / Psychological Support，
避免下游誤解為行政或懲罰性措施。

dignity_multiplier_bounds_ref
已移除硬編碼數值，改為引用設定常數，
方便未來在不動事件格式的情況下調整上限。

explanation_short
Multiplier does not influence clinical_priority_score
防止任何「尊嚴插隊醫療」的誤用或政治化指控。
