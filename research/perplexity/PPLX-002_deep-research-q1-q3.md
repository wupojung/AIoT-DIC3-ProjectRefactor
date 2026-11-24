# PPLX-002 Deep Research：五個思維模式 / 三個爭論 / 十個深度題

> **狀態：AI_PERPLEXITY / 未獨立驗證（UNVERIFIED）。** 來源僅為搜尋結果摘要，尚未全文閱讀或 DOI 核對；進入正式 Proposal 前須依 `research/cross-validation/` 流程查核。生成日期：2026-11-25。依據：教師作業規範 Deep Research Q1–Q3、本組 DESIGN.md。

## Q1. 五個核心思維模式

| # | 思維模式 | 意義 | 證據（待查核） | 對系統設計的影響 |
| --- | --- | --- | --- | --- |
| 1 | 成交資料是樣本，不是市場母體 | 實價登錄有申報時點、揭露批次與案件類型限制，不等於即時行情 | 買賣於移轉登記後申報，內政部每月 1、11、21 日發布 [S6][S7]；官方研究指出從議價到公布存在時間落差 [S5] | 每個數字附快照版本、日期、樣本數；缺口時降級或安全停止 |
| 2 | 時間與制度會改變資料意義 | 制度改版與市場波動使舊規則、舊樣本失效 | 實價登錄 2.0 於 110 年修法，門牌地號完整揭露、預售屋納管 [S3]；政大論文以 RDD 比較 1.0 與 2.0 的市場影響 [S8] | RAG 文件需有版本、生效日、適用條件；回答標示資料期間 |
| 3 | 驗證必須防洩漏，不以單一誤差取勝 | 空間相鄰樣本同時出現在訓練與測試集會高估表現 | 2026 年 ScienceDirect 研究指出空間資料洩漏可能造成樂觀的績效 [S9]；2025 SCITEPRESS 論文採地點分組交叉驗證 [S10] | 評估採時間／空間切分；指標除 MAE/RMSE 外加引用支持率與拒答率 |
| 4 | 解釋不等於因果，不確定性要揭露 | SHAP 顯示模型依賴哪些特徵，不證明特徵造成價格 | 2026 Wiley 研究以可解釋空間 ML 做 hedonic 模型 [S11]；SCITEPRESS 論文以 SHAP 指出地段、面積、交通、屋齡為主要影響 [S10] | 結果頁標示「模型歸因，非因果」；模型僅在適用域檢核通過時顯示 |
| 5 | Agent 的可靠性來自邊界，不來自自由度 | 工具白名單、有界迴圈與錯誤可見，比更強的推理更能降低風險 | ReliabilityBench 報告在壓力條件下 Agent 可靠度會下降，且 ReAct 比 Reflexion 穩健 [S12]；Zillow Offers 因估價模型與市場波動失敗而關閉 [S1][S2] | LLM 不得自行計算房價；補問 ≤ 2 輪、可重試錯誤重試 1 次；Evidence Gate + Safe Stop |

## Q2. 三個最重要的專業爭論

### 爭論 1：固定 Workflow ↔ 靈活 ReAct Agent
- **觀點 A（固定流程）**：可預測、易測試、延遲低。**觀點 B（ReAct）**：能處理多樣提問與多步驟工具選擇。
- **Evidence**：ReliabilityBench 顯示 Agent 在擾動與壓力下可靠度下降，其中以速率限制影響最大 [S12]；Zillow 案例顯示高自動化決策遇到市場波動時風險會放大 [S1][S2]。
- **Consensus（暫定）**：高風險領域採「有界 Agent + 確定性工具」混合設計。
- **Unresolved**：何種查詢複雜度才值得 ReAct？本組以 Baseline A（固定流程）對 Proposed B（ReAct+RAG）的實驗回答，結果為 Expected，尚未觀察。

### 爭論 2：預測精度 ↔ 可解釋性與可追溯
- **觀點 A**：以最低誤差為目標。**觀點 B**：買方需要理解依據與限制，不只要一個數字。
- **Evidence**：Zillow 研究與教學案例指出速度與演算法估價可能削弱人工評估 [S1][S13]；CMU Tepper 2026-11-16 文章討論 Zillow iBuyer 關閉對房價的後續影響 [S2]；空間洩漏研究顯示高精度可能是驗證設計造成的假象 [S9]。
- **Consensus（暫定）**：決策支援場景優先可解釋與可追溯，ML 估價作為條件式候選。
- **Unresolved**：SHAP 在相關特徵下的穩定性；非預測型指標（引用支持率）如何量化。

### 爭論 3：精細 GIS／完整揭露 ↔ 資料品質與隱私限制
- **觀點 A**：地址與座標越精細，比較越準。**觀點 B**：精度不足、隱私與使用權利會使精細分析失真。
- **Evidence**：實價登錄 2.0 將門牌地號完整揭露並兼顧個資隱私 [S3][S4]；2026 Wiley 研究處理空間特徵於房價模型的角色 [S11]。
- **Consensus（暫定）**：GIS 為選配，位置精度、品質與權利許可時才啟用（對應 DESIGN FR-4）。
- **Unresolved**：臺中快照的地址／座標品質尚待 RQ 檢核；日期欄語義待確認。

## Q3. 十個深度理解問題

| # | 層次 | 問題 | 好答案應包含 |
| --- | --- | --- | --- |
| 1 | Why | 為什麼成交價不能直接當「現在的行情」？ | 申報時點、發布批次、樣本非母體 |
| 2 | Why | 為什麼不讓 LLM 自行計算單價或距離？ | 可重現、可追溯、幻覺風險；工具確定性計算 |
| 3 | What-if | 如果快照已過期或某區本月樣本很少，Agent 應如何回應？ | 揭露日期與樣本數、降級、安全停止 |
| 4 | What-if | 如果法規在查詢期間修訂，RAG 會出什麼問題？ | 版本、生效日、適用條件過濾 |
| 5 | Trade-off | 固定 Workflow 與 ReAct 各在何種查詢上較佳？ | 可預測性 vs 彈性、延遲、可測試性 |
| 6 | Failure Case | Zillow Offers 的失敗給本系統哪些設計啟示？ | 模型對市場波動的脆弱、自動化決策缺人工與邊界 |
| 7 | Evidence | 一個高準確率房價模型為何仍可能不可信？ | 空間／時間洩漏、驗證切分、適用域 |
| 8 | Comparison | SHAP 歸因與因果推論有何差別？ | 相關 vs 因果、特徵相關性 |
| 9 | Design Decision | 為何 GIS 設為選配而非必備？ | 位置精度、品質、使用權利、隱私 |
| 10 | Real-world | 使用者問「這間貴不貴」，Agent 應補問什麼、提供什麼、拒絕什麼？ | 地區、期間、型態、車位；比較統計與限制；不給投資／稅務／法律建議 |

## 來源清單（均為搜尋結果摘要層級，查核狀態：FOUND_ONLY）

- [S1] Stanford GSB, Flip Flop: Why Zillow's Algorithmic Home Buying Venture Imploded (2021-12-09) https://www.gsb.stanford.edu/insights/flip-flop-why-zillows-algorithmic-home-buying-venture-imploded
- [S2] CMU Tepper, Why Zillow's iBuyer Shutdown Drove Up Home Prices (2026-11-16) https://www.cmu.edu/tepper/news/stories/zillows-ibuyer-shutdown-drove-home-prices
- [S3] 內政部地政司，落實資訊公開透明（實價登錄 2.0） https://www.land.moi.gov.tw/chhtml/content/1123?mcid=5129
- [S4] 國發會，實價登錄2.0—促進交易資訊更即時透明（PDF，見搜尋結果）
- [S5] 新北市政府地政局，我國實價登錄制度與不動產交易資訊透明度之問題研析（官方研究 PDF）
- [S6] 內政部地政司，實價登錄2.0 https://www.land.moi.gov.tw/chhtml/content/1141?mcid=4254
- [S7] toestate.tw，實價登錄多久出來（民間整理，unrated；發布日期 1/11/21 須與官方核對） https://toestate.tw/learn/lvr-faq
- [S8] 政大學位論文，資訊透明化對房市的影響：以台灣實價登錄1.0與2.0政策為例 https://ah.lib.nccu.edu.tw/item?item_id=177307&locale=zh_TW
- [S9] ScienceDirect, Explainable machine learning for urban housing prices: Policy relevant ... (2026) https://www.sciencedirect.com/science/article/pii/S0264275126004981
- [S10] SCITEPRESS (2025), A Data-Driven Machine Learning Framework for Real Estate Valuation ... https://www.scitepress.org/Papers/2025/147100/147100.pdf
- [S11] Wiley (2026-05), Explainable spatial machine learning for hedonic real estate modeling https://onlinelibrary.wiley.com/doi/full/10.1111/1540-6229.70030
- [S12] ReliabilityBench: Evaluating LLM Agent Reliability Under Production-Like Stress Conditions (Semantic Scholar) https://www.semanticscholar.org/paper/ReliabilityBench:-Evaluating-LLM-Agent-Reliability-Gupta/681daebc067e7daa132b3405c3f9c4b34f189987
- [S13] JISE (2024) Teaching Case: Role of AI in the Closure of Zillow Offers https://www.jise.org/Volume35/n1/JISE2024v35n1pp67-72.html

## 查核備註

- Zillow 的虧損金額在不同來源不一致（搜尋結果出現 Q3 2021 損失 8.81 億美元的說法，來自 unrated 來源），引用前須回到 Zillow 財報或一手報導核對。
- [S7] 為民間整理，只能作線索；發布日須以內政部公告為準。
- 本檔所有結論均為「文獻說了什麼」，不代表本組系統已驗證任何效果（對應 agent.md：Expected vs Observed）。

## 提示詞與修訂歷程

- 2026-11-25 v0：依教師 Q1–Q3 規範與 DESIGN.md，以 Perplexity 搜尋生成；Owner 審查前不得標為 VERIFIED / FINAL。
