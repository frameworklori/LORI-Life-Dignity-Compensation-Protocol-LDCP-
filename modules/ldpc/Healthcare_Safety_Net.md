# Healthcare_Safety_Net.md

# 1. 核心願景（Core Vision）

本模組旨在落實 LORI（Life Dignity）原則，確保在 AI / 自動化引發的大規模失業或社會結構轉型期間，個體的生命健康權不因經濟能力喪失而崩潰。

醫療保障在本協議中被明確定義為：

生命尊嚴的底線保障（Dignity Floor），而非市場商品。

Healthcare Safety Net 並非取代現有醫療體系，而是作為 LDPC 在系統性衝擊下的自動接管層，確保：

醫療權利與「生命身分」綁定

而非與「就業狀態」或「支付能力」綁定

# 2. 資源分配機制（Resource Allocation Logic）

本模組拒絕「價高者得」與「市場競價」邏輯，
所有醫療與藥物資源配置，皆須經 ODRAF（Outcome-Driven Risk Assessment Framework） 評估。

2.1 優先級演算指標（Priority Scoring）

資源分配依以下權重進行動態排序：

a. 生存急迫性（Urgency Index）

急性生命威脅 ＞ 慢性病管理

不可逆傷害 ＞ 可延後處理

b. 尊嚴補償係數（Dignity Multiplier）

若個體因 AI / 自動化 / 制度轉型 而喪失原有社會保障

其優先級將獲得補正

目的在於避免「二次結構性傷害」

c. 預防性干預權重（Preventive Weight）

成癮預防

傳染病防控

基礎心理健康支持

原則：預防性資源優先於事後高成本補救

2.2 質與量控管（Quality & Quantity Control）
質量監控（Quality Assurance）

所有列入 LDPC 補償清單之藥物

必須通過 開源臨床數據審核

排除「單一藥廠主導」與「利益導向偏誤」

按需定製（Demand-Based Allocation）

與 simulations/ 模組連動

預測區域性疾病與藥物需求

動態調整原料、物流與製造節點

降低庫存浪費與供應鏈斷裂風險

# 3. 藥物濫用與風險防護欄

Substance Abuse Guardrails（Non-Punitive）

本協議承認：
成癮風險屬於健康狀態，而非道德缺陷。

因此，LDPC 不採取懲罰性手段，而是依風險狀態動態調整補償形式。

風險狀態	補償形式	介入措施
低風險（Low）	全額藥物選擇權	定期健康追蹤
中風險（Medium）	定向醫療券（Vouchers）	限制特定藥物劑量，轉換為物理治療或替代療法
高風險（High）	強制康復服務包	暫停現金 / 藥物直發，改為專人照護與戒斷支援

限制的唯一目的：恢復未來的自主選擇能力。

# 4. 決策裁定機制（Jury-Based Judgment）

當發生 稀缺資源爭議（例如：最後一份特效藥的分配）時：

4.1 禁止單點決策

不得由 AI 單獨裁定

不得由行政或市場力量逕行處理

4.2 啟動陪審裁定流程

從 modules/jury/ 調用合格陪審角色

依 ODRAF_Core 提供之後果預演資料進行投票

4.3 可審計性

所有裁定流程、證據摘要、投票結果

必須封存於 specs/03_Governance.md 定義之公開帳本

支援事後審計與申訴

# 5. 數據交互範例（Data Schema Interface）
