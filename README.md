# AIoT DIC3 — Project Proposal Refactor

> NCHU AIoT · 2026-11-11 DIC3 · Research-first, evidence-traceable workflow

本 Repository 用於整理 **Taiwan Housing Intelligence Agent** 的原始 Proposal、AI 輔助研究報告、交叉驗證證據與重構成果。它是課堂 DIC3 的獨立研究工作區，**不是**主程式開發 Repository；不得將本庫的「研究規劃」誤標成主專題已完成的實驗結果。

- 主專題：[Taiwan-Housing-Intelligence-Agent](https://github.com/wupojung/Taiwan-Housing-Intelligence-Agent)
- 老師參考：[2026-11-11-DIC3-ProjectRefactor](https://github.com/huanchen1107/2026-11-11-DIC3-ProjectRefactor)
- 教師要求與本庫執行狀態：以本 README 為入口；具體檔案以 [research/README.md](research/README.md) 為索引。
- AI 工作規則：[agent.md](agent.md)（並由 [AGENTS.md](AGENTS.md) 引導支援該慣例的開發工具）。

## 01. Teacher's DIC3 requirements（老師原文）

1. **install openspec and be familiar with spec-driven development**
2. **make clear of your project A to F (abstract) 5 cores, 3 trade-off and 10 details**
3. **convert design.md to workflow in openspec**
4. **Introduction refactor**
5. **related work refactor**

以上五項按課堂公告原文保存；是否有追加期限、繳交格式，須另外以老師的正式公告或可核對錄音確認，勿自行推定。

課堂另有示範提示詞：**「請分析內容 做成 資訊圖表 A4 橫式 高密度 NUMBERING」**。本庫將此列為目前第 2 項的**視覺化產出**，保留和五項公告要求的區分。

## 02. Current checkpoint（2026-11-13）

| Item | State | Notes |
| --- | --- | --- |
| 1. OpenSpec 安裝與 SDD | NOT STARTED | 尚未安裝，先記錄要求 |
| 2. A–F / 5 cores / 3 trade-offs / 10 details | IN PREPARATION | 待匯入原始報告、整理並核對 |
| 2a. A4 landscape high-density numbered infographic | AI DRAFT — REVIEW PENDING | INFO-001 已提交 PR；圖表內容與文字尚待驗證，不是最終驗收版本 |
| 3. DESIGN.md → OpenSpec workflow | NOT STARTED | 等 1 及設計基線確認後 |
| 4. Introduction refactor | NOT STARTED | 需要可查證證據 |
| 5. Related work refactor | NOT STARTED | 需要 DOI / 全文證據檢核 |
| 原始檔與研究材料匯入 | IN REVIEW | TEAM 原件已在 `main`；NotebookLM 分析與 AI 資訊圖表在 PR #2；教師已授權的兩份參考 PDF 由本次 PR 提交。詳見 [研究總索引](research/README.md) |

**目前工作焦點：資料盤點、分類、來源紀錄與 Research Refactor 的準備。** 表格狀態僅描述本 Repository，不代表原主專題的實作進度。

## 03. Repository map

| Location | Purpose |
| --- | --- |
| [research/originals/](research/originals/INDEX.md) | 原始來源：你上傳的原始 PDF、PPTX、DOCX、課程材料；只新增、不覆寫 |
| [research/notebooklm/](research/notebooklm/INDEX.md) | NotebookLM 產出的分析、摘要與引用，標記為 AI 衍生材料 |
| [research/perplexity/](research/perplexity/INDEX.md) | Perplexity 輸出、提示詞、修改歷程及執行限制紀錄 |
| [research/cross-validation/](research/cross-validation/INDEX.md) | 多來源交叉比對、主張核對與差異裁定 |
| [research/refactor/](research/refactor/INDEX.md) | 經整理的 A–F、Introduction、Related Work 與研究修訂 |
| [research/infographic/](research/infographic/INDEX.md) | A4 橫式高密度 NUMBERING 圖表與可編輯來源 |
| [papers/](papers/INDEX.md) | 文獻 metadata、DOI 索引與 BibTeX；不收未授權的論文全文 |
| [research/README.md](research/README.md) | 研究檔案索引與各產出狀態 |

## 04. Evidence and publication rules

- **Original / AI-derived / Verified / Final** 是不同角色，不等同於「品質高低」；不可用檔名推測其真實性。
- 原始檔保留原檔名與位元組；若內容改動，另存新版本、建立來源連結與 SHA-256；不要覆寫。
- 每筆研究資產至少記錄：`Asset ID`、檔名/路徑、類別、來源、產生日期（未知時標記 UNKNOWN）、基於哪些輸入、驗證狀態、是否可公開。
- 「已找到 DOI」不代表「全文已讀」或「該論文支持某主張」；不同查核層次需分開記錄。
- 本 Repository **目前公開**。禁止上傳無公開再散布權的付費論文 PDF、個資、憑證或不允許公開的素材。學校授權下載不等同於 GitHub 公開散布授權。
- PDF、簡報與其他二進位檔案如需 LFS，先檢查大小與 `.gitattributes`；LFS 不處理著作權問題。
- 暫不將 AI 草稿自動升級為正式研究結論；實驗尚未驗證者只能標示 `Expected Outcomes`。

## 05. Collaboration

研究原始資料先由 Owner 匯入；AI 整理與查核採可追溯增量工作。重要變更透過 GitHub Issue / PR 留下說明，合併原則為 **Squash Merge**；不得因要求進度而省略證據紀錄。這個庫不自動啟動 OpenSpec 或對主專題進行寫入。

## 06. Intake status

NotebookLM 分析與 AI 資訊圖表已依來源分類提交 PR #2 審查；AI 內容尚未獨立核驗。Proposal 原件已由 PR #1 合併進 `main`。Owner 已確認授課教師允許公開本次 DIC3 兩份參考 PDF，現由本次 PR 審查。所有 AI 衍生內容、驗證結果與正式採納狀態分開記錄於各 [研究索引](research/README.md)。
