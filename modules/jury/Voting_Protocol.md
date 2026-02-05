# LDPC / ODRAF Jury Voting & Adjudication Protocol
Version: v1.0 (Final · Repo-ready)

# 1. 文件定位（Purpose & Scope）

本文件定義 Jury 的投票與裁定技術規範，適用於 AI 與自動化流程必須讓位於人類判斷的情境。

本文件僅回答一個問題：

裁定是如何形成的（HOW）

不回答以下問題（已由其他文件定義）：

誰有資格成為陪審員（見 Jury_Roles.md）

風險如何計算（見 Risk_Weights.md）

裁定資料如何封包（見 Jury_Decision_Packet.md）

# 2. Jury 觸發條件（Trigger Conditions）

當任一條件成立時，系統 必須中止自動化流程，並生成 Jury Decision Packet：

2.1 ODRAF 熔斷（Fuse）

OR-1：Opioid Red-Flag

VD-1：Verification Hold（例如 UDS pending）

2.2 Scarcity Traffic Light（STL）

STL 狀態 = RED

且資源屬於：

控制性藥物

生命關鍵資源

高外溢風險物資

2.3 尊嚴相關上訴

對 dignity_multiplier

或 access_compensation 結果提出異議

# 3. 投票人數與門檻（Quorum & Thresholds）

不同風險層級，必須使用不同共識強度：

決策場景	最低陪審人數	通過門檻	制度理由
一般補償 / 流程異議	5	多數決 51%	效率優先
稀缺資源分配	7	絕對多數 66%	涉及他人權益
高風險藥物 / 控制性資源	9	超絕對多數 75%	防濫用、集體責任

重要規則：

陪審人數不足 → 不得裁定

棄權票視為反對票

不允許「技術性通過」

# 4. 裁定維度（Adjudication Dimensions）

陪審員必須就以下三個維度 分開投票，避免單一價值覆蓋所有判斷。

4.1 Clinical Necessity Review

是否同意 AI 對 Urgency / Irreversibility 的判斷？

是否存在被低估或高估的臨床風險？

限制條款：
非 Clinical Juror 不得否決臨床救命判斷。

4.2 Dignity Justification Review

是否同意目前 dignity_multiplier？

是否存在 AI 無法捕捉的結構性脆弱或照護責任？

限制條款：

Dignity 不得攔截臨床救治

僅能影響補償形式、支援強度、review 週期

4.3 Alternative Viability Review

在稀缺或高風險下，替代方案是否：

醫學可行

不造成尊嚴崩塌

可實際執行（非象徵性方案）

# 5. 投票機制（Voting Mechanics）
5.1 盲測投票（Blind Voting）

投票期間不得查看他人選擇

防止從眾效應與權威壓力

5.2 角色權限限制

每票權重相等

但以下條款必須遵守：

項目	限制
Clinical 判斷	不得被非 Clinical Juror 否決
Dignity Multiplier Override	必須包含 LDPC Beneficiary 同意
程序瑕疵	Legal/Audit Juror 可要求重審
# 6. 判決輸出（Verdict Schema）

所有 Jury 裁定 必須輸出為機器可讀格式：


{
  "verdict_id": "VERDICT-2026-0042",
  "case_packet_ref": "JURY-PKT-2026-RED-001",
  "final_decision": "APPROVED_WITH_CONDITIONS",
  "approved_resources": [
    "NON_OPIOID_SERVICE_PACKAGE"
  ],
  "rejected_resources": [
    "OPIOID_DIRECT_DISPENSE"
  ],
  "mandatory_conditions": [
    "WEEKLY_CHECK_IN",
    "BEHAVIORAL_SUPPORT_REQUIRED"
  ],
  "dignity_multiplier_override": 1.35,
  "override_reason": "Additional caregiver burden not captured by AI",
  "jury_signature_count": 7,
  "justification_hash": "sha256:..."
}

# 7. 安全與反操控基礎（Baseline Integrity）

去識別化資料輸入（僅 index 與 score）

Jury 成員隨機抽樣

所有投票寫入不可變 Audit Ledger

Verdict 不可事後修改（僅可補充理由）

詳細防操控與偵測邏輯，定義於
modules/jury/Anti_Manipulation_Checks.md

# 8. 原則性總結（Protocol Statement）

Voting_Protocol 的目的不是讓「多數即正義」，
而是確保在高風險決策中：

沒有人可以躲在系統後面

沒有人可以用效率壓過責任

沒有人可以說「這不是我下的判斷」

文件狀態： FINAL
依賴文件：

Jury_Roles.md

Jury_Decision_Packet.md

Risk_Weights.md

本文件一經發布，即視為 LDPC 體系中不可被單方面弱化的裁定程序條款。
