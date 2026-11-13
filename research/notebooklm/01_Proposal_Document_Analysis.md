# Taiwan Housing Intelligence Agent：專題提案（01_Proposal_Document）深度解析與學術規範評估報告

本報告依據**「期末專題提案作業規範.pdf」**所設定之評分標準、架構要求與工程交付原則，並結合**「A2Z outline example .pdf」**之學術論文寫作標準作業程序（2020 A-Z SOP），對**《01_Proposal_Document.pdf》**（專題名稱：*Taiwan Housing Intelligence Agent: An Explainable Agentic AI Framework for Real-Estate Valuation and Decision Support*）進行全面性、結構化之批判性解析與對照評估。

---

## 1. 執行摘要與規範符合度總覽

本專題提案以臺灣住宅決策支援為應用場景，探討如何結合實價登錄資料、確定性統計分析、領域法規知識檢索與可解釋人工智慧，建構一個具備證據檢核與安全停止機制的領域智慧代理人（Domain AI Agent）。

### 核心定位檢核
* **符合 Domain AI Agent 定位**：提案未流於單純的模型訓練（如單純訓練 XGBoost/LightGBM 房價預測模型），亦非包裝商用 API 的純對話機器人（Chatbot）。其架構核心為由 ReAct 推理迴圈驅動、調用授權工具、檢索版本化知識庫，並透過「Evidence Gate」進行資料完整性與時效性稽核的決策代理系統，完全契合規範第一條「AI Agent + Domain Knowledge + Data Analytics + Real-World Application」之主旨。
* **秉持學術誠信與安全原則**：提案文件明確標記本階段為「研究提案（Proposal）」，將資料可行性評定為「有條件可行（Conditional Go）」，所有效能評估指標均誠實歸類為「待驗證之研究假設」，未預先虛構實驗成效；系統設計明確禁止語言模型自行運算房價，貫徹數值計算與生成的解耦。

---

## 2. 依「A2Z 論文架構 SOP」之逐層對照解析

依據 A2Z Outline 之五大章節與代號指引，本提案之落實狀況剖析如下：

### 2.1 Abstract 摘要（A–F 六段式故事結構）
作業規範與 A2Z 架構高度重合，均要求以 A 至 F 鋪陳完整研究故事。本提案之對應情況如下：
* **A. Attention Getter（動機引題）**：由臺灣住宅交易決策之複雜性切入，指出實價登錄雖提供公開成交紀錄，但因揭露樣本存在時間落差且非全市場母體，難以直接等同即時市場行情。
* **B. But（挑戰與現存限制）**：點出單價計算受車位拆分、建物移轉面積計算標準不一所扭曲；現有單一模型估價往往僅輸出孤立數值，無法呈現資料邊界、樣本代表性與估算不確定性。
* **C. Cure（解藥與提出系統）**：提出 *Taiwan Housing Intelligence Agent*，以 **ReAct + RAG** 作為系統核心架構。
* **D. Development（方法與模組設計）**：透過有界 Thought–Action–Observation 迴圈調用經授權的結構化資料工具，RAG 模組檢索帶有版本與法規生效日的領域知識，並由 Evidence Gate 檢驗 Schema、單位、時效與適用範圍。
* **E. Experiments（驗證規劃與資料切片）**：取得臺中市政府地政局 115 年第 3 季官方買賣案件 CSV 快照（10,638 筆、32 欄，篩選得 9,152 筆住宅候選），並設計「Baseline A（固定流程）」與「Proposed B（ReAct+RAG Agent）」之對照基準。
* **F. Findings / Expected Outcomes（預期成果）**：恪守規範第 2 條要求，明確註記尚未進行模型訓練與 Agent 實作，評定結論為「有條件可行」，所有預期效益均屬待檢驗假設，展現嚴謹之研究態度。

### 2.2 I. Introduction 緒論（G–L 結構）
* **G. General 現況 & H. However 挑戰**：詳述住宅「成交價」之條件相依性（受樓層、型態、車位影響），點出買方若缺乏資料限制之提示，極易誤判行情。
* **I. In literature 文獻解法 & J. Judgement 評析**：簡評既有房價預測文獻多聚焦於預測誤差最小化，但往往忽略時空資訊洩漏與模型歸因不等於因果推論之根本限制。
* **K. Keypoint 設計精神與貢獻**：確立「數值＋樣本範圍＋日期＋來源＋限制」的整體資訊揭露體系；強調在缺乏核心證據時主動觸發「安全停止（Safe Stop）」。
* **研究邊界與非目標（Non-Goals）**：明確界定先期以臺中市住宅成交比較為範圍，**不推測個人財務、不保證獲利或成交價、不提供法律與稅務意見**。
* *(優化建議 - L 項目)*：本節結尾宜補上一段章節組織交代（List the Organization），明確引導讀者後續第 2 至第 6 節的閱讀脈絡。

### 2.3 II. Related Work 相關研究（M–O 結構）
* **M. Methods 分類梳理**：文獻劃分為「時空房價預測與歸因方法」與「AI Agent、RAG 及領域評測」兩大脈絡。
* **N. New Proposed 新典範**：跳脫「模型預測競賽」，提出結合 Evidence Gate 之代理人決策檢驗架構。
* *(優化建議 - O 項目)*：提案已納入 23 篇權威文獻，建議依照 A2Z 之 O（Organize）規範，增補一張 **Article Summary Table**，以結構化呈現各文獻之 AI 技術與領域資料特性。

### 2.4 III. Proposed Scheme 系統設計（PA–PM 架構）
* **PA–PC（Aim / Based / Cure）**：明確界定系統核心在於提供「可追溯、知限制、防猜價」之住宅比較分析。
* **PD–PF（Design / Elements / Formulation）**：完整拆解為 8 大功能模組，實現推理決策、資料計算與文本生成之權責分離。
* **PG–PI（Graph / Flowchart / Pseudocode）**：具備完整資料流向圖與對照偽碼，詳細定義重試上限、資料隔離（Quarantine）與降級停機機制。

### 2.5 IV. Simulation / Evaluation 驗證規劃（Q–V 架構）
* **Q. Quality and Quantity 指標**：規劃雙層驗證指標——「資料/價格分析層」（MAE、RMSE、R²）與「Agent 回答可靠性層」（工具合法性、引用支持率、數值一致率、適當拒答率、執行延遲與工具成本）。
* **R. Related Method 基準對照**：以「Baseline A（固定查詢與統計流程）」對照「Proposed B（ReAct + RAG Agent）」，在同等任務下驗證 Agent 引入之邊際效益與系統開銷。

### 2.6 V. Conclusion 結論與展望（W–Z 架構）
* **W. What & X. eXcel**：重申可信住宅 Agent 的本質不是「會回答」，而是讓使用者明瞭數值背後的計算方法與資料限制。
* **Y. Yields & Z. Zen**：客觀總結臺中快照盤點成果，確立後續資料字典校核、中古屋規則釐清與原型建構之研發路徑。

---

## 3. 依「期末專題提案作業規範」評分維度深度剖析

作業規範所訂定之提案評分標準（占總成績 10%）分為六大範疇，本提案之具體表現與得分點如下：

```
評分項目與權重分佈：
- Introduction 緒論 (20%)
- Related Work 相關研究 (20%)
- Deep Research 深入研究 (20%)
- Proposed AI Agent + System Architecture 系統架構 (20%)
- Flowchart + Pseudocode + Technical Feasibility 技術可行性 (15%)
- GitHub + DESIGN.md + 完整度 (5%)
```

### 3.1 Introduction 緒論（配分：20%）
* **重要性與動機**：直擊不動產實價登錄之資訊不對稱與單價計算陷阱，具備強烈現實應用價值。
* **客觀證據支持**：引用內政部不動產交易實價查詢服務網之官方 FAQ，指出「揭露成交資料不代表市場全部行情」與「平均單價受面積與車位計算影響」之官方論據。
* **創新理念**：提出以「安全停止」替代「強行生成」，開創重視負責任決策支援之 Agent 新架構。

### 3.2 Related Work 相關研究（配分：20%）
規範明定文獻探討須兼具 **約 1/2 AI Technology + 1/2 Domain Know-how**，本提案文獻配置高度均衡：

| 領域範疇 | 代表文獻與技術主題 | 規範落實與批判性檢驗重點 |
| :--- | :--- | :--- |
| **AI Technology (約 50%)** | • Lewis et al. (2020) RAG<br>• Yao et al. (2023) ReAct<br>• Schick et al. (2023) Toolformer<br>• Kim et al. (2024) LLM Compiler<br>• Zhu & Han (2025) REAL Benchmark | 指出既有 Agent 與 RAG 基準非專為臺灣不動產法規設計，檢索機制本身無法保證事實絕對正確，技術成效不可直接外推。 |
| **Domain & Method (約 50%)** | • Roberts et al. (2017) 時空交叉驗證<br>• Lundberg & Lee (2017) SHAP 歸因<br>• Wang et al. (2021) 異質資料預測<br>• Chou et al. (2023) 時空大量估價<br>• Hung et al. (2025) 總體經濟與房價 | 區分「模型歸因」與「實質因果」，指出時空相依資料若未採時間/空間留出（Holdout）將產生嚴重資料洩漏。 |

### 3.3 Deep Research 深入研究（配分：20%）
文件完整回答規範所要求的領域深度研究三大核心問題：
1. **五個核心思維模式（Mindsets）**：
   * *先定任務再選驗證*：時間與空間切分方式必須對應實際推論任務，嚴防跨時空資訊洩漏。
   * *價格是有條件的觀察*：已揭露紀錄非全市場，必須註記房型、屋齡、車位與單價公式。
   * *空間粒度決定回答範圍*：行政區聚合統計數據絕不得直接等同特定個別房屋之精準估價。
   * *解釋不等於因果*：SHAP 數值僅代表模型特徵貢獻度，不可直接視為政策或物理條件之因果效應。
   * *證據須有來源與有效時間*：法規條文、稅率、補助門檻與價格行情必須保留版本與生效時間窗口。
2. **三個專業爭論與取捨（Controversies / Trade-offs）**：
   * *固定 Workflow vs. 靈活 Agent*：評估自然語言調度彈性與執行成本、延遲及重現性間的平衡。
   * *預測精度 vs. 可解釋性*：分析極小化預測誤差（RMSE）與使用者能否理解估價依據間的權衡。
   * *精細 GIS 分析 vs. 資料現實限制*：考量政府開放資料去識別化與門牌模糊化對空間分析造成的精度瓶頸。
3. **十個深度理解與口試驗收問題**：
   * 設計 10 題涵蓋母體代表性、車位拆算、時空洩漏防範、SHAP 邊界、法規時效稽核、防猜價機制等深度問題，能有效區分死記硬背與實質理解。

### 3.4 Proposed Scheme 系統架構（配分：20%）
提案架構嚴格落實規範建議之 8 大模組職責：
* **① User / Dashboard**：提交條件並視覺化檢視比較結果與證據。
* **② Query Parser / Validator**：自然語言解析為行政區、期間、房型等 typed 結構，歧義時發動補問（上限 2 輪）。
* **③ ReAct Orchestrator**：有界迴圈調用授權白名單工具（步數上限 6 步）。
* **④ Structured Transaction Retrieval**：僅查詢經授權來源與快照版本。
* **⑤ Deterministic Analytics Engine**：以確定性程式工具運算統計分布，嚴禁 LLM 自行計算。
* **⑥ RAG Knowledge Retrieval**：檢索帶有版本與法規生效日之領域知識文件。
* **⑦ Evidence & Safety Gate**：檢驗資料來源、Schema、單位、時效與適用範圍。
* **⑧ Cited Response / Safe Stop**：組裝附來源回答，核心證據不足時主動安全停止。

### 3.5 Flowchart、Pseudocode 與技術可行性（配分：15%）
* **演算法規格（Pseudocode）**：
  文件摘要核心控制流程，並於 Repository 與簡報中提供完整帶行號之規格化偽碼。其核心保護邏輯展現在：
  * 設定常數上限：`MAX_REACT_STEPS = 6`、`MAX_CLARIFICATION_ROUNDS = 2`、`MAX_RETRY = 1`。
  * 限制白名單工具（Allowlist Authorization），未授權操作立即終止。
  * 數值計算強制交由確定性工具執行：`calculate_with_deterministic_tool(query, observations)`，禁止 LLM 自行演算。
  * 核心交易資料缺失時強制執行安全停止：`Safe Stop, no price claim`。
* **資料可行性實證盤點**：
  * 資料源：臺中市政府地政局 115 年第 3 季買賣案件官方 CSV（政府資料開放平臺代碼 103038）。
  * 樣本規模：10,638 筆原始記錄、32 個欄位；篩選出住宅候選 9,152 筆（住宅大樓 5,933、華廈 1,553、透天厝 1,461、公寓 205）。
  * 資料限制識別：揭露「民國年月」編碼業務定義尚待官方字典核實，不直接等同於成交季度，展現深厚之資料敏感度。

### 3.6 GitHub Repository 與 DESIGN.md（配分：5%）
* **版本控管落實**：建立專屬 GitHub 私有儲存庫，依規範建立並維護 `DESIGN.md`。
* **架構決策追蹤**：完整記錄功能需求（FR-1 至 FR-7）、非功能需求（Traceability、Safety、Reliability、Reproducibility 等），並明訂待決策架構清單（Open Architecture Decisions），奠定可傳承（Handover）之工程基礎。

---

## 4. 總合評價矩陣與口試精進建議

### 4.1 綜合評量矩陣表

| 評核項目 | 權重 | 符合水準 | 核心優勢實證 | 後續交件與口試加強建議 |
| :--- | :---: | :---: | :--- | :--- |
| **Introduction 緒論** | 20% | 優等 | • 結合真實痛點，立論紮實<br>• 明確劃定情境邊界與非目標 | 於 1.3 節末段增補章節導覽段落（A2Z 之 L 要素）。 |
| **Related Work 相關研究** | 20% | 優等 | • 兼顧 50% AI 與 50% 領域知能<br>• 批判性論述，拒絕直推外在榜單 | 將 23 篇文獻補齊為標準 Article Summary Table。 |
| **Deep Research 深入研究** | 20% | 特優 | • 5 大思維與 3 大爭論論述精闢<br>• 10 道檢驗題極具實務鑑別度 | 口試簡報應針對 10 道深度問題進行防守演練。 |
| **Proposed Scheme 系統架構** | 20% | 優等 | • 8 大模組職責清晰<br>• 數值計算與語言生成嚴格解耦 | 在下一階段明確選定 Agent Orchestrator 實作框架。 |
| **Flowchart & Pseudocode** | 15% | 優等 | • 具備行號、重試限制與例外隔離<br>• 臺中快照實證盤點完備 | 於正文附錄完整印出 30 行偽碼以利對照。 |
| **GitHub 與 DESIGN.md** | 5% | 優等 | • 維護獨立可讀的 DESIGN.md<br>• 追蹤未決決策與版本狀態 | 持續維持程式碼 Commit 與 DESIGN.md 演進同步。 |

### 4.2 口試防守與 Final Handover 關鍵行動指南
1. **確立資料字典業務定義**：儘速向臺中市政府地政局或內政部確認「交易年月日」與「申報登錄年月日」之確切欄位定義，避免時間留出（Temporal Holdout）時發生資料洩漏。
2. **固化中古屋篩選條件**：明確化排除非住宅用途（如店面、辦公室、農舍）與特殊親友交易之資料清理規則，確保 Baseline A 與 Proposed B 之對照基礎絕對一致。
3. **貫徹 Handover 標準**：期末交付時，須確保包含完整原始碼、依賴環境設定檔（requirements.txt/Dockerfile）、測試腳本與超過 20 頁之簡報及 10 分鐘展示影片，使外部接手者能無縫復現本專題之驗證流程。
