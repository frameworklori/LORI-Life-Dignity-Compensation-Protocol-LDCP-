# ODRAF Core — Draft v1.0 (Repo-ready)

Metadata

Module: ODRAF (Original Design Risk Assessment Framework)

Sub-module: Risk_Weights

Layer: Decision Support / Logic Calculation

Primary Consumers: modules/ldpc/Healthcare_Safety_Net.md, modules/jury/

Purpose: 將定性「風險」量化為可計算、可審核、可封存的權重與熔斷規則。

## 0. 定義與範圍（Definitions & Scope）

本文件定義 ODRAF 在 LDPC / Healthcare_Safety_Net 情境下的權重結構，用於：

產出 Risk Score / Priority Score

觸發 Hard Constraints（熔斷） → 強制 Jury

生成可審計的 scoring trace（計算足跡）

重要邊界：

Clinical Triage（臨床救治先後）不受「尊嚴乘數」直接覆寫

AI_displaced 僅能影響 Access / Coverage，不得影響臨床 triage（與 Healthcare_Safety_Net 的 GR-4 一致）

## 1. 權重架構總覽（Weighting Framework）

ODRAF 最終風險得分由三個維度構成：

𝑇
𝑜
𝑡
𝑎
𝑙
_
𝑅
𝑖
𝑠
𝑘
=
(
𝐶
𝑙
𝑖
𝑛
𝑖
𝑐
𝑎
𝑙
_
𝐼
𝑚
𝑝
𝑎
𝑐
𝑡
×
𝛼
)
+
(
𝑆
𝑦
𝑠
𝑡
𝑒
𝑚
𝑖
𝑐
_
𝑅
𝑖
𝑠
𝑘
×
𝛽
)
+
(
𝐷
𝑖
𝑔
𝑛
𝑖
𝑡
𝑦
_
𝐸
𝑟
𝑜
𝑠
𝑖
𝑜
𝑛
×
𝛾
)
Total_Risk=(Clinical_Impact×α)+(Systemic_Risk×β)+(Dignity_Erosion×γ)

𝛼
+
𝛽
+
𝛾
=
1.0
α+β+γ=1.0

1.1 預設係數（Default Coefficients）

α (Clinical): 0.50

β (Systemic): 0.30

γ (Dignity): 0.20

設計理由：在醫療與生命風險場景下，臨床救治優先是制度底線；系統性風險（濫用/稀缺）次之；尊嚴侵蝕用於「補償形式與可近性」的治理修正，而非 triage 插隊。

## 2. 維度一：臨床與物理損害（Clinical Impact — α）

衡量對個體生理健康的直接損害。

Metric	Weight (within α)	Definition
Urgency（急迫性）	0.40	延遲處理是否造成立即生命危險
Irreversibility（不可逆性）	0.35	損害是否永久（殘疾、不可逆神經損傷等）
Evidence Grade（證據等級）	0.25	A=1.0, B=0.8, C=0.5, D=0.2（可由 config 調整）
## 3. 維度二：系統性風險（Systemic Risk — β）

衡量決策對社會系統、供應鏈與群體風險的外溢影響，特別對應：

藥物濫用（如 opioid）

稀缺資源（庫存、產能、物流）

長期公共成本

Metric	Weight (within β)	Definition
Abuse Potential（濫用/成癮潛力）	0.50	轉售、誤用、誘發流行性成癮風險
Resource Scarcity（稀缺度）	0.30	庫存/需求比 + 供應鏈可用性
Systemic Cost（系統成本）	0.20	長期公共資源消耗（醫療、治安、社會成本）
## 4. 維度三：尊嚴侵蝕度（Dignity Erosion — γ）

反映 LORI 原創精神：衡量「不作為」或「錯誤介入」對人的尊嚴傷害。

Metric	Weight (within γ)	Definition
Autonomy Loss（自主權喪失）	0.40	是否缺乏上訴/解釋/替代方案（程序正義）
Vulnerability Index（脆弱性）	0.40	失業、無健保、照護者責任、居住不穩等
Social Stigma（烙印風險）	0.20	介入是否導致污名化或歧視性標籤

限制條款（與 Healthcare_Safety_Net 一致）：
Dignity Erosion 不得直接攔截臨床救治，只能：

影響補償形式（voucher / transport / care coordination）

影響 review 週期與支援強度（mandatory_support 等）

## 5. 硬性約束規則（Hard Constraint Rules / Fuse）

ODRAF 設置熔斷機制，避免權重計算被「平均化」而失真。

OR-1：Opioid Red-Flag（鴉片類紅旗）

Condition

requested_resource.resource_code == "OPIOID_CLASS" AND Abuse_Potential > 0.70

Outcome

Total_Risk = CRITICAL

強制觸發 modules/jury/

並與 Healthcare_Safety_Net 的 GR-2_ADD_RISK_PLUS_OPIOID 對齊（禁止 direct dispense）

CP-1：Clinical Primacy（臨床優先原則）

Condition

Urgency > 0.80

Outcome

系統必須優先撥付臨床救治資源

Dignity Erosion 僅能影響「可近性補償」與「流程支援」
（例如：轉診交通、快速排程、住院協調），不得阻斷救命流程

VD-1：Verification Hold（驗證暫停）

（對齊你 Healthcare_Safety_Net 的 uds_result: pending 設計）

Condition

uds_result == "pending" AND addiction_risk_level in {"medium","high"} AND requested_resource == "OPIOID_CLASS"

Outcome

強制 Hold_Verification

只能走 Non_Opioid_First 或 Specialist Review

直到 uds_result ∈ {positive, negative, inconclusive}

## 6. 稀缺敏感度：建議採用「三色燈」而非單一 10% 觸發

Gemini 目前提議「庫存低於 10% 權重翻倍」是好起點，但單點閾值容易被批評為：

太粗糙

太容易被操控（把庫存維持在 11% 就避開規則）

建議：Scarcity Traffic Light（STL）

Green: inventory_ratio ≥ 0.30 → 正常權重

Yellow: 0.10 ≤ inventory_ratio < 0.30 → Scarcity weight × 1.5

Red: inventory_ratio < 0.10 → Scarcity weight × 2.0 + 可觸發 Jury（視資源類型）

你問「夠不夠敏感」：
我會選 三色燈，因為它更像制度（而不是單一數字魔法）。

## 7. 權重動態調整（Dynamic Adjustment）

當偵測到 System_Shock（供應鏈崩潰、災害、疫情）：

Resource_Scarcity 權重（within β）從 0.30 → 0.60

Systemic_Cost 影響下降（避免在緊急期過度計算長期成本）

同時啟用 Scarcity Traffic Light 的 Red 模式 作為預設

## 8. 計算足跡與可審計性（Scoring Trace & Auditability）

每次 ODRAF 計算必須輸出：

係數（α/β/γ）

每個 metric 的原值、正規化值、權重

觸發的 fuse 規則（OR-1 / CP-1 / VD-1）

最終建議（recommendation）與允許選項集合（allowed set）
