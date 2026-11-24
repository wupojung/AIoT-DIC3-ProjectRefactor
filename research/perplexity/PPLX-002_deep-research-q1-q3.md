# PPLX-002 Deep Research：五個思維模式 / 三個爭論 / 十個深度題

> **狀態：AI_PERPLEXITY / UNVERIFIED。** 版本 v1（PR #5 審查修正，2026-11-25）。來源僅由代理讀取摘要或頁面，未經人工全文查核。本組尚未完成任何正式研究或實驗；文中「設計」「假設」皆為 Expected，尚無 Observed 結果。

## 適用性限制

- S11 為德國租屋刊登資料，S10 為印度 Bengaluru 資料，Zillow 為美國 iBuyer 案例；均不能直接外推到臺灣實價登錄，只作方法與風險參照。
- S10 為會議論文，S12 為單一作者 arXiv 預印本，證據強度有限。
- 「未找到」不等於「不存在」；缺口列於文末 Cross-validation 清單。

## Q1. 五個核心思維模式

| # | 思維模式與意義 | 證據與限制 | 對系統設計的影響 |
| --- | --- | --- | --- |
| 1 | 成交資料是揭露後的樣本，不是市場母體。實價登錄不等於區域全部行情。 | 內政部查詢網說明資料已篩除顯著異常與特殊交易，且不代表區域內所有不動產行情[S6]；自 106 年 12 月起每月 1、11、21 日發布（原每月 2 次）[S6][S7]；批次以登記日期而非交易日期切分[S6]；新北地政局研究指出議價到公布有時間落差[S5]（僅讀摘要）。 | 每個數字附快照版本、資料期間、樣本數；樣本不足時降級或 Safe Stop。 |
| 2 | 時間與制度會改變資料意義。 | 內政部說明 110 年推動實價登錄 2.0，涵蓋門牌地號完整揭露與預售屋納管[S3]；政大學位論文以 RDD 研究 1.0/2.0 的影響[S8]（作者與結論未核對）。 | RAG 文件保留版本、生效日與適用條件；回答標示資料期間。 |
| 3 | 驗證設計比單一誤差更重要。 | S11 以前六季訓練、最後一季測試，且插補在 train-test 切分內進行以避免洩漏；S10 採地點分組 10 折交叉驗證，並指出隨機 K-fold 可能高估表現；S9 摘要指空間鄰近樣本同時進入訓練與測試集會造成樂觀績效（頁面讀取失敗，中繼資料 UNAVAILABLE）。 | 評估採時間／空間切分；指標分層（見下表）。 |
| 4 | 模型歸因不等於因果；不確定性要揭露。 | S10 提醒空間自相關下解讀 ML 歸因須避免天真詮釋；S11 以 SHAP 描述位置特徵，並發現大量插補的空間變數幾乎無增益、可能引入雜訊。兩篇皆未宣稱因果。 | 結果頁標示「模型歸因，非因果」；ML 估價僅在適用域檢核通過時顯示。 |
| 5 | Agent 可靠性必須被量測；邊界與故障處理是設計假設。 | S12（arXiv:2601.06112）：2 個模型、2 種架構（ReAct、Reflexion）、4 個領域（排程、旅遊、客服、電商）、1,280 episodes；擾動強度 ε=0.2 使成功率由 96.9% 降至 88.1%；速率限制是最具破壞力的故障；組合壓力下 ReAct 比 Reflexion 穩健。**限制：未比較固定 Workflow，非房地產領域，不能證明 ReAct 優於固定 Workflow。** | 測試納入工具逾時、速率限制、schema drift；Workflow vs ReAct 列為 H1 待驗證假設（見 Q2-1）。 |

### 案例：Zillow Offers 關閉有多重因素

關閉不應單純歸因於 AI 估價失準。

| 面向 | 證據 | 與本系統關聯 |
| --- | --- | --- |
| 模型估價 | 公司稱房價預測的不可預測性遠超預期[S15]；2021 Q3 認列約 3.04 億美元庫存減值，因買入價高於當時對未來售價的估計[S14][S15]。Stanford 報導減值 5.69 億美元[S1]，範圍與 S14 不同，未調和。 | 估價不確定性須揭露。 |
| 營運容量 | 董事會決議提及容量限制與營運挑戰，受疫情、勞動與供應鏈環境加劇[S14]；Q3 營收低於預期，因轉售容量限制[S15]。 | 本系統不持有庫存，不適用。 |
| 庫存與流動性 | NBER 研究：iBuyer 僅在流動性高、易估價的房屋獲利；快速成交與精準估價之間有取捨並面臨逆選擇[S16]。Stanford 引述受訪教授認為 Zillow 擴張到非標準住宅並出價高於模型預測是錯誤[S1]（受訪者觀點）。 | 不交易，不適用；啟示是估價適用域要檢核。 |
| 市場環境 | 董事會文件稱受前所未有的住房市場與疫情影響[S14]。 | 資料期間需標示。 |
| 關閉後影響 | S2（Troncoso 等，2026）分析關閉後 Boston 與 Pittsburgh 共 27,908 筆刊登的市場反應。**研究對象是關閉後的影響，不是關閉原因的證據。** | 僅說明公開演算法失信會影響市場決策。 |

## Q2. 三個最重要的專業爭論

### 爭論 1：固定 Workflow ↔ ReAct Agent

- 觀點 A：固定流程可預測、易測試、延遲低。
- 觀點 B：ReAct 能處理多樣提問與多步驟工具選擇。
- Evidence：S12 只測 Agent 在壓力下的可靠性，不含固定 Workflow 對照。本次搜尋未找到房地產領域直接比較兩者的證據。
- Provisional Design Position：採有界 ReAct（白名單唯讀工具）並保留 Baseline A 作對照。這是本組設計選擇，不是已建立的學術共識。
- 待驗證假設 H1：在多步驟查詢上，Proposed B 的任務完成率與引用支持率不低於 Baseline A，且失敗率可接受。狀態：Expected，未觀察。
- Unresolved：何種查詢複雜度才值得 ReAct；延遲與成本代價。

### 爭論 2：預測精度 ↔ 可解釋性與可追溯

- 觀點 A：以低誤差為目標。S11 報告空間 GP 加樹提升在其德國租屋資料的 RMSE；S10 報告 stacking 在其 Bengaluru 資料表現最佳，兩者同時使用 SHAP，顯示在其資料集上精度與解釋可並存。
- 觀點 B：模型常漏掉買方在意的屬性，且快速估價有逆選擇風險[S1][S16]。
- Provisional Design Position：預設呈現確定性統計；ML 估價僅作適用域合格的條件式候選。
- Unresolved：臺灣資料上的實際表現；SHAP 在相關特徵下的穩定性（本次未找到證據）。

### 爭論 3：精細 GIS ↔ 資料品質限制

- 觀點 A：實價登錄 2.0 完整揭露門牌地號，使精細空間分析更可行[S3]。
- 觀點 B：S11 只有 48% 資料有精確位置、16% 為近似位置，其餘以行政區中心代替；大量插補的空間變數幾乎無增益。
- Provisional Design Position：GIS 為選配，位置精度、品質與使用權利許可時才啟用（對應 DESIGN FR-4）。
- Unresolved：臺中快照的地址／座標品質與日期欄語義尚待檢核。

## Q3. 十個深度理解問題

| # | 層次 | 問題 | 好答案應包含 |
| --- | --- | --- | --- |
| 1 | Why | 為什麼成交價不能直接當「現在的行情」？ | 篩除異常與特殊交易、依登記日期批次發布、樣本非母體 |
| 2 | Why | 為什麼不讓 LLM 自行計算單價或距離？ | 可重現、可追溯、幻覺風險；確定性工具計算 |
| 3 | What-if | 快照過期或某區樣本很少時，Agent 如何回應？ | 揭露日期與樣本數、降級、Safe Stop |
| 4 | What-if | 法規在查詢期間修訂，RAG 會出什麼問題？ | 版本、生效日、適用條件過濾 |
| 5 | Trade-off | 固定 Workflow 與 ReAct 各適合哪種查詢？ | 可預測性 vs 彈性、延遲、可測試性；H1 尚待驗證 |
| 6 | Failure Case | Zillow Offers 關閉涉及哪些因素？哪些與本系統相關？ | 區分模型估價、營運容量、庫存流動性、市場環境；本系統只與估價不確定性相關 |
| 7 | Evidence | 高準確率房價模型為何仍可能不可信？ | 空間／時間洩漏、驗證切分、適用域、跨資料集外推 |
| 8 | Comparison | SHAP 歸因與因果推論有何差別？ | 描述模型行為 vs 識別因果；特徵相關性 |
| 9 | Design Decision | 為何 GIS 設為選配？ | 位置精度、插補雜訊、使用權利、隱私 |
| 10 | Real-world | 使用者問「這間貴不貴」，Agent 應補問、提供、拒絕什麼？ | 補問地區、期間、型態、車位；提供比較統計與限制；不給投資、稅務、法律建議 |

## 指標分層（設計草案，均未量測）

| 層次 | 候選指標 | 備註 |
| --- | --- | --- |
| ML | MAE、RMSE、MAPE、R²；時間／空間切分 | 依 S10、S11 的做法；僅在適用域合格時評估 |
| Agent | 任務完成率、pass^k 一致性、擾動穩健度、工具故障容忍 | 概念取自 S12 |
| RAG | 引用支持率、版本與生效日正確率 | 本組自訂 |
| 安全 | Safe Stop 正確率、不支持主張比例、超範圍建議拒絕率 | 本組自訂 |
| 效率 | 延遲、工具呼叫次數、重試次數、成本 | 本組自訂 |

## 來源清單

查核層級：PAGE_READ＝代理讀取頁面內容；SNIPPET＝僅搜尋摘要；FAILED＝讀取失敗。皆未經人工全文或 DOI 查核。

| ID | 作者／機構、年份、題名、刊物 | DOI／連結 | 層級 |
| --- | --- | --- | --- |
| S1 | Kiger, P. J.（2021）Flip Flop: Why Zillow's Algorithmic Home Buying Venture Imploded，Stanford GSB Insights | [連結](https://www.gsb.stanford.edu/insights/flip-flop-why-zillows-algorithmic-home-buying-venture-imploded)；無 DOI | PAGE_READ |
| S2 | Miller, J.（2026）Tepper 新聞稿，摘述 Troncoso, Malik, Proserpio & Fu（2026），Quantitative Marketing and Economics 24, 12 | DOI 10.1007/s11129-026-09316-y（取自 Tepper 頁面，期刊頁未核對）；[連結](https://www.cmu.edu/tepper/news/stories/zillows-ibuyer-shutdown-drove-home-prices) | PAGE_READ |
| S3 | 內政部地政司，落實資訊公開透明 | [連結](https://www.land.moi.gov.tw/chhtml/content/1123?mcid=5129) | SNIPPET |
| S4 | 國發會，實價登錄2.0—促進交易資訊更即時透明 | [連結](https://ws.ndc.gov.tw/Download.ashx?u=LzAwMS9hZG1pbmlzdHJhdG9yLzEwL3JlbGZpbGUvMC8xNDM2Ni83MGNjNjFhOS05YWIxLTQwNGItYTdkNC03YjQ3NzQ5ZjIwN2EucGRm&n=6KuW6KGhMTktMV81LTIu54m55Yil5LyB5YqDX+WvpuWDueeZu+mMhDIuMC5wZGY=&icon=..pdf)；年份未確認 | SNIPPET（正文未引用） |
| S5 | 新北市政府地政局，我國實價登錄制度與不動產交易資訊透明度之問題研析（檔名標示 107 年自行研究，年份未核對） | [連結](https://www.rde.ntpc.gov.tw/userfiles/1160700/files/107%E8%87%AA%E7%A0%94_%E6%88%91%E5%9C%8B%E5%AF%A6%E5%83%B9%E7%99%BB%E9%8C%84%E5%88%B6%E5%BA%A6%E8%88%87%E4%B8%8D%E5%8B%95%E7%94%A2%E4%BA%A4%E6%98%93%E8%B3%87%E8%A8%8A%E9%80%8F%E6%98%8E%E5%BA%A6%E4%B9%8B%E5%95%8F%E9%A1%8C%E7%A0%94%E6%9E%90%EF%BC%BF%E5%9C%B0%E6%94%BF%E5%B1%80.pdf) | SNIPPET |
| S6 | 內政部，不動產交易實價查詢服務網 | [連結](https://lvr.land.moi.gov.tw/jsp/index.jsp) | SNIPPET |
| S7 | 內政部地政司，實價登錄專區（106 年 12 月起每月 3 次發布） | [連結](https://www.land.moi.gov.tw/chhtml/content/90?mcid=471) | SNIPPET |
| S8 | 政治大學學位論文，資訊透明化對房市的影響：以台灣實價登錄1.0與2.0政策為例（作者、年份未核對） | [連結](https://ah.lib.nccu.edu.tw/item?item_id=177307&locale=zh_TW) | SNIPPET |
| S9 | 題名 Explainable machine learning for urban housing prices: Policy relevant ...；作者、刊物、DOI 均 UNAVAILABLE | [連結](https://www.sciencedirect.com/science/article/pii/S0264275126004981) | SNIPPET；頁面讀取 FAILED |
| S10 | Reddy, Y. L. N. & Abhirami, M. K.（2025）A Data-Driven Machine Learning Framework for Real Estate Valuation and Prediction，ICCCSS 2025 Vol. 2, pp. 832-840，SCITEPRESS | DOI 10.5220/0014710000005024；[連結](https://www.scitepress.org/Papers/2025/147100/147100.pdf) | PAGE_READ |
| S11 | Gyger, T., Hauri, S., Bühlmann, S., Lehner, M., Schlesinger, J. & Sigrist, F.（2026）Explainable spatial machine learning for hedonic real estate modeling，Real Estate Economics 54(3), 761-806 | DOI 10.1111/1540-6229.70030；[連結](https://onlinelibrary.wiley.com/doi/full/10.1111/1540-6229.70030) | PAGE_READ |
| S12 | Gupta, A.（2026）ReliabilityBench: Evaluating LLM Agent Reliability Under Production-Like Stress Conditions，arXiv:2601.06112（預印本，2026-01-03 提交） | DOI 10.48550/arXiv.2601.06112；[連結](https://arxiv.org/abs/2601.06112) | PAGE_READ（摘要頁） |
| S13 | JISE（2024）Teaching Case: Role of AI in the Closure of Zillow Offers | [連結](https://www.jise.org/Volume35/n1/JISE2024v35n1pp67-72.html) | SNIPPET；未讀取，不作為證據 |
| S14 | Zillow Group, SEC Form 8-K（2021-11-02） | [連結](https://www.sec.gov/Archives/edgar/data/1617640/000161764021000085/z-20211102.htm) | SNIPPET |
| S15 | Zillow Group 新聞稿（2021-11-02）Third-Quarter 2021 Financial Results & Plan to Wind Down Zillow Offers | [連結](https://investors.zillowgroup.com/news-and-events/news/news-details/2021/Zillow-Group-Reports-Third-Quarter-2021-Financial-Results--Shares-Plan-to-Wind-Down-Zillow-Offers-Operations/default.aspx) | SNIPPET |
| S16 | Buchak, G., Matvos, G., Piskorski, T. & Seru, A.（2020）Why is Intermediating Houses so Difficult? Evidence from iBuyers，NBER Working Paper 28252 | DOI 10.3386/w28252；[連結](https://www.nber.org/papers/w28252) | SNIPPET |

已移除：民間網站 toestate.tw（改引內政部官方 S6、S7）；未經一手核對的 8.81 億美元虧損說法（v0 誤記為 Q3 損失，v1 不採用）。

## Cross-validation 待辦

- S9：取得作者、刊物、DOI，確認空間洩漏結論。
- S3、S4、S5、S6、S7、S14、S15、S16：讀取全文或官方頁面，核對引用句。
- S8：確認作者、年份與 RDD 結論。
- S2：核對期刊頁 DOI 與研究結論。
- S12：確認同行審查狀態；補查固定 Workflow 與 ReAct 的直接比較研究。
- Stanford 5.69 億美元與 SEC 3.04 億美元的範圍差異。
- 補查 SHAP 在相關特徵下的穩定性與臺灣實價登錄 ML 估價研究。

## 提示詞與修訂歷程

- 原始生成提示詞：UNAVAILABLE（未封存逐字檔）。使用者請求摘要（PARAPHRASE）：依教師 Q1–Q3 規範，查找最新研究與案例，寫入 repo。
- 2026-11-25 v0（commit cc0c501）：初稿。
- 2026-11-25 v1：依 PR #5 獨立審查修正。審查指示為 PARAPHRASE（逐字內容在對話中，未封存）。變更：修正 S12 對應（Q1-5、Q2-1）；Zillow 改為多因素並同步 Q3-6，S2 改為關閉後影響研究；三處「Consensus（暫定）」改為 Provisional Design Position；新增指標分層與適用性限制；補來源中繼資料與官方發布頻率；移除民間來源。
- 狀態仍為 UNVERIFIED，Owner 審查前不得標為 VERIFIED 或 FINAL。
