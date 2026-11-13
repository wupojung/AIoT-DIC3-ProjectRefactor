# agent.md — AI Agent 工作規範

人類入口：[README.md](README.md)；研究索引：[research/README.md](research/README.md)。

## 定位
- 本庫為公開的 NCHU AIoT DIC3 Project Proposal Refactor 研究材料庫。
- 老師五項要求按 README 原文保存；目前以**資料收錄、A–F / 5 cores / 3 trade-offs / 10 deep questions 與資訊圖表**為先。
- 未收到明確指示，不安裝 OpenSpec、不改主專題 Repo、不將 AI 分析自動當成已驗證成果。

## 必守原則
1. `research/originals/` 存放未改動的原始檔：**教師規範**與**本組正式 Proposal**分開登記，不可混淆或覆蓋。
2. `research/notebooklm/`、`research/perplexity/` 保存 AI 衍生分析，必須記錄提示詞、依據文件與修改歷程；AI 輸出不等於獨立查證。
3. `research/cross-validation/` 才存主張查證：必須區分「找到文獻」「DOI 核對」「全文閱讀」「主張有依據」。
4. `research/refactor/` 保存新寫內容，不直接改寫 `originals`；標記研究結果為 Expected 或 Observed。
5. 所有公開附件都要先檢查著作權、個資與憑證；不公開未獲再散布授權的付費論文 PDF。
6. 只有實際提交的檔案才能標成 UPLOADED；檔案不在 GitHub 時記錄 PENDING，不可捏造 Hash 或路徑。
7. 進行 GitHub 修改時保留變更依據，重要工作採 Issue → PR → Review → Squash Merge；不得無授權改寫歷史。

## 檔案登錄
每份資產應有：Asset ID、原檔名、路徑、來源分類、生成日期（不明則 UNKNOWN）、父來源、版本、審查狀態、公開權利、SHA-256 與提交 SHA（如可取得）。
