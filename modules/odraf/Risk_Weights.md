
# ODRAF Core — Draft v1.1 (Kindness Boundary Integrated)

Metadata

Module: ODRAF (Original Design Risk Assessment Framework)

Sub-module: Risk_Weights

Layer: Decision Support / Logic Calculation

Primary Consumers: modules/ldpc/Healthcare_Safety_Net.md, modules/jury/

Purpose: 量化風險並引入「善良邊界」機制，防止濫用、維持系統可持續性，同時保障程序正義。

# 0. 核心原則（Non-Negotiables）

Clinical Primacy：救命優先，尊嚴不得插隊臨床 triage

Non-Punitive：不以求助次數「懲罰」患者；邊界只限制高風險資源的形式與強度

Accountable & Appealable：任何邊界觸發都必須可解釋、可上訴、可由 Jury 覆核

Support-First：邊界的目的不是拒絕，而是把支援形式從「高風險資源」轉為「可持續支持」

1. 權重架構總覽（Weighting Framework）


Total_Risk=(Clinical_Impact×α)+(Systemic_Risk×β)+(Dignity_Erosion×γ
dynamic)

𝛼
+
𝛽
+
𝛾
𝑑
𝑦
𝑛
𝑎
𝑚
𝑖
𝑐
=
1.0
α+β+γ
dynamic
=1.0




1.1 預設係數（Baseline Coefficients）

α (Clinical): 0.45

β (Systemic): 0.30

γ (Dignity): 0.25 (baseline)

說明：比 v1.0 略提高尊嚴權重，但仍維持臨床與系統風險優先的治理結構。

2. 維度一：臨床與物理損害（Clinical Impact — α）
Metric	Weight (within α)
Urgency	0.40
Irreversibility	0.35
Evidence Grade	0.25

Evidence Grade mapping (default): A=1.0, B=0.8, C=0.5, D=0.2

3. 維度二：系統性風險（Systemic Risk — β）
Metric	Weight (within β)
Abuse Potential	0.50
Resource Scarcity	0.30
Systemic Cost	0.20
4. 維度三：尊嚴侵蝕度（Dignity Erosion — γ_dynamic）
Metric	Weight (within γ)
Autonomy Loss	0.40
Vulnerability Index	0.40
Social Stigma	0.20

注意：Social Stigma 權重維持 0.20（不建議任意提高），因為污名化是治理風險，但不可取代臨床救治。

5. 硬性約束規則（Hard Constraint Rules / Fuse）
OR-1：Opioid Red-Flag

Condition: resource_code == OPIOID_CLASS AND Abuse_Potential > 0.70

Outcome: Total_Risk = CRITICAL + 強制 Jury + 禁止 direct dispense

CP-1：Clinical Primacy

Condition: Urgency > 0.80

Outcome: 臨床救治優先；尊嚴只影響 access/support，不得攔截救命流程

VD-1：Verification Hold

Condition: uds_result == pending AND addiction_risk ≥ medium AND opioid request

Outcome: Hold_Verification + Non_Opioid_First + Specialist Review required

6. 權重動態調整（Dynamic Adjustment）
6.1 Scarcity Traffic Light（沿用三色燈）

Green: inventory_ratio ≥ 0.30 → normal

Yellow: 0.10–0.30 → scarcity weight × 1.5

Red: <0.10 → scarcity weight × 2.0 (+可觸發 Jury 視資源類型)

6.2 善良邊界（Kindness Boundary）— 可辯護版本

Grok 的直覺是對的：善良若無邊界會造成濫用與系統崩壞。
但「只看 request_count」會在醫療倫理上被攻擊，因此本框架採用：

γ_dynamic 的調整依據：合規（compliance）× 風險改善（improvement）× 是否屬於高風險資源（controlled resource）
而不是單純求助次數。

定義三個治理參數

request_count：同類型請求次數（僅作參考，不作單獨懲罰）

compliance_rate：對 mandatory_support、review、檢測（如 UDS/PDMP）的遵守率（0–1）

risk_trend：風險趨勢（improving / stable / worsening）

γ_dynamic 設計（核心）

γ_base = 0.50（首次高同理）

γ_min = 0.20（最低尊嚴底線）

當且僅當「高風險資源」情境成立（例如 opioid / controlled substances），才啟動邊界衰減：

𝛾
𝑑
𝑦
𝑛
𝑎
𝑚
𝑖
𝑐
=
𝑐
𝑙
𝑎
𝑚
𝑝
(
𝛾
𝑚
𝑖
𝑛
,
 
𝛾
𝑏
𝑎
𝑠
𝑒
×
(
1
−
𝑑
)
,
 
𝛾
𝑏
𝑎
𝑠
𝑒
)
γ
dynamic
	​

=clamp(γ
min
	​

, γ
base
	​

×(1−d), γ
base
	​

)

其中衰減 
𝑑
d 定義為：

若 compliance_rate >= 0.80 且 risk_trend == improving → d = 0.00（不衰減）

若 compliance_rate < 0.80 或 risk_trend != improving →

𝑑
=
𝑚
𝑖
𝑛
(
0.30
,
 
0.10
×
𝑚
𝑎
𝑥
(
0
,
𝑟
𝑒
𝑞
𝑢
𝑒
𝑠
𝑡
_
𝑐
𝑜
𝑢
𝑛
𝑡
−
1
)
)
d=min(0.30, 0.10×max(0,request_count−1))

解釋：

有配合、有改善 → 不削尊嚴權重

不配合、又反覆高風險請求 → 邊界逐步收緊

但永遠不低於 γ_min，避免把人踢出系統

7. 新增硬性邊界觸發（Boundary Trigger, BT-1）
BT-1：Dependency Warning & Mode Shift（依賴警告與模式切換）

Condition: request_count > 3 AND compliance_rate < 0.80 AND controlled_resource_request == true

Outcome:

降低 γ_dynamic（依 6.2）

強制模式切換：Medication_Voucher → Support_Package（以支持包替代高風險資源）

通知使用者：「邊界警告：持續依賴會降低未來高風險資源可得性」

必須允許上訴並可由 Jury 覆核

8. JSON Snippet（Interface）
