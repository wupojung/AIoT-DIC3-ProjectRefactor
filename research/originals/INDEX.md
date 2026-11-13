# Originals — 原始素材索引

此處只保存來源原檔。教師規範（`TEACHER_REQUIREMENT`）與本組原始提案（`TEAM_PROPOSAL_ORIGINAL`）分開登記；AI 分析與研究重寫成果不屬於 originals。

**Original File Verification 不等於 Research Claim Verification。** 本頁的格式、可讀性、位元組及 SHA-256 查核不代表文件中的研究主張已獲學術驗證。

## Teacher Originals — `TEACHER_REQUIREMENT`

| Asset ID | 實際原始檔名 | Source / 日期 / 父來源 / 版本 | SHA-256 | Size (bytes) | GitHub Path | Publication Status | Verification Status |
| --- | --- | --- | --- | ---: | --- | --- | --- |
| TCH-001 | `期末專題提案作業規範.pdf` | 老師提供；日期 UNKNOWN；父來源 UNKNOWN；原始版本 | `070d4d299a5da00b0c2b2395c2afff901e8318962c824db399af9043930b02a4` | 171407 | `research/originals/期末專題提案作業規範.pdf`（PENDING） | **Publication Hold**：尚未確認老師或權利人允許公開再散布 | 本機 PDF 可讀、非空、Hash 已比對；未公開；研究主張未驗證 |
| TCH-002 | `A2Z outline example .pdf` | 老師提供；日期 UNKNOWN；父來源 UNKNOWN；原始版本 | `777386d58818577471bb02067cd01b7b14d24dc55b497a6d667febdb2c207ef4` | 140798 | `research/originals/A2Z outline example .pdf`（PENDING） | **Publication Hold**：尚未確認老師或權利人允許公開再散布 | 本機 PDF 可讀、非空、Hash 已比對；未公開；研究主張未驗證 |

先前索引預填名稱為 `期末專題提案作業規範(1).pdf`，本次檔案系統實際名稱是 `期末專題提案作業規範.pdf`；先前預填 SHA-256 與實際檔案一致。依實際檔名登記，未更名。教師參考頁：<https://github.com/huanchen1107/2026-11-11-DIC3-ProjectRefactor>。Git LFS 或其他儲存方式都不構成著作權授權。

## Team Originals — `TEAM_PROPOSAL_ORIGINAL`

| Asset ID | 實際原始檔名 | Source / 日期 / 父來源 / 版本 | SHA-256 | Size (bytes) | GitHub Path | Publication Status | Verification Status |
| --- | --- | --- | --- | ---: | --- | --- | --- |
| TEAM-001 | `01_Proposal_Document.pdf` | 本組既有 Proposal；日期 UNKNOWN；父來源 UNKNOWN；原始版本 | `bfb83ccb67b0b2935322e189aca02f3a5d82a077603dbe03296a64e55acd43e6` | 219499 | `research/originals/01_Proposal_Document.pdf` | 可公開收錄（本組提供作公開專案原始成果） | PDF 可讀、非空、複製後 Hash 相同；未發現憑證、聯絡方式或身分識別號樣式，作者 metadata 為團隊名稱；研究主張未驗證 |
| TEAM-002 | `02_Proposal_Presentation.pptx` | 本組既有 Proposal；日期 UNKNOWN；父來源 UNKNOWN；原始版本 | `a0bf70d5ae624fe797f0027f230893a83511eb64bc88972b61d596ad50b579f9` | 729680 | `research/originals/02_Proposal_Presentation.pptx` | 可公開收錄（本組提供作公開專案原始成果） | OOXML ZIP 結構有效、非空、複製後 Hash 相同；未發現憑證、聯絡方式或身分識別號樣式，作者 metadata 為團隊名稱；研究主張未驗證 |
| TEAM-003 | [`DESIGN.md`](DESIGN.md) | 本組既有 Proposal；日期 UNKNOWN；父來源 UNKNOWN；既有原始版本 | `896a485d3f9b20f4ee58daf50f8af1574a51e8e5408681104e24c67c7781a567` | 12476 | `research/originals/DESIGN.md` | 已公開（既有 Git blob `7a3403a9cd5c502b41e42ea8c5821518655210d7`） | UTF-8 可讀；incoming 與既有檔逐位元組相同，未重複提交；研究主張未驗證 |

先前索引預填名稱為 `01_Proposal_Document(1).pdf`；本次檔案系統實際名稱是 `01_Proposal_Document.pdf`，SHA-256 與預填值一致。依實際檔名登記，未更名。`TEAM-002` 名稱及 SHA-256 與先前索引一致。

## 原件處理及驗證範圍

1. 原始檔保留檔名、內容與位元組；不覆寫或自動改寫。
2. 本次完整性檢查涵蓋檔案存在、非零大小、格式可讀及 SHA-256。PDF 以 Poppler 解析及文字抽取；PPTX 以 ZIP 測試並抽取 XML 文字；Markdown 以嚴格 UTF-8 解碼。
3. 憑證與個資掃描是明顯模式檢查（憑證關鍵詞、Email、電話、身分識別號樣式及作者 metadata）；無命中不代表保證沒有所有敏感內容。
4. NotebookLM、Perplexity、ChatGPT 分析及交叉驗證記錄應存入對應研究分類，不屬於 originals。
5. `DESIGN.md` 是研究規格草稿，不代表 OpenSpec 已執行。

回到 [研究總索引](../README.md)。
