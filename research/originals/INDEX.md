# Originals — 原始素材索引

本索引只登記來源原檔。教師文件（`TEACHER_ORIGINAL`）與本組成果（`TEAM_ORIGINAL`）分開管理。檔案完整性驗證不代表研究主張已獨立核對（`VERIFIED`），也不代表內容已正式採納（`FINAL`）。

## Teacher Originals — `TEACHER_ORIGINAL`

| Asset ID | File Name | Exact Repository Path | Category | Source / Parent Asset | SHA-256 | Size (bytes) | Publication Status | Verification Status | Git Commit / PR |
| --- | --- | --- | --- | --- | --- | ---: | --- | --- | --- |
| TCH-001 | `期末專題提案作業規範.pdf` | `research/originals/期末專題提案作業規範.pdf` (not published) | `ORIGINAL` | 老師提供；日期 UNKNOWN；原始版本 | `070d4d299a5da00b0c2b2395c2afff901e8318962c824db399af9043930b02a4` | 171407 | **Publication Hold** — 未找到明確公開再散布授權；本機原檔保留於 `incoming/` | 本機 PDF 可解析、非空、SHA-256 已計算；研究內容未核驗 | 未提交 |
| TCH-002 | `A2Z outline example.pdf` | `research/originals/A2Z outline example.pdf` (not published) | `ORIGINAL` | 老師提供；日期 UNKNOWN；原始版本 | `777386d58818577471bb02067cd01b7b14d24dc55b497a6d667febdb2c207ef4` | 140798 | **Publication Hold** — 未找到明確公開再散布授權；本機原檔保留於 `incoming/` | 本機 PDF 可解析、非空、SHA-256 已計算 | 未提交 |

本次檔案系統顯示 A2Z 檔名為 `A2Z outline example.pdf`（`.pdf` 前沒有空格）；先前索引記為 `A2Z outline example .pdf`。兩者 SHA-256 相同，代表只修正了登記檔名，原檔內容未變。兩份教師 PDF 均未複製到正式歸檔、未加入 Git，也未因本次要求推定授權。

## Team Originals — `TEAM_ORIGINAL`

| Asset ID | File Name | Exact Repository Path | Category | Source / Parent Asset | SHA-256 | Size (bytes) | Publication Status | Verification Status | Git Commit / PR |
| --- | --- | --- | --- | --- | --- | ---: | --- | --- | --- |
| TEAM-001 | `01_Proposal_Document.pdf` | `research/originals/01_Proposal_Document.pdf` | `ORIGINAL` | 本組既有 Proposal；日期 UNKNOWN；原始版本 | `bfb83ccb67b0b2935322e189aca02f3a5d82a077603dbe03296a64e55acd43e6` | 219499 | PR #1 Open；尚未進入 `main` | PDF 可解析、非空；PR 分支內 hash 與來源相同；研究主張未核驗 | `20a6e37fbe540de5aef6fc91b2f7708475758e97` / [PR #1](https://github.com/wupojung/AIoT-DIC3-ProjectRefactor/pull/1) |
| TEAM-002 | `02_Proposal_Presentation.pptx` | `research/originals/02_Proposal_Presentation.pptx` | `ORIGINAL` | 本組既有 Proposal；日期 UNKNOWN；原始版本 | `a0bf70d5ae624fe797f0027f230893a83511eb64bc88972b61d596ad50b579f9` | 729680 | PR #1 Open；尚未進入 `main` | OOXML ZIP 結構有效、非空；PR 分支內 hash 與來源相同；研究主張未核驗 | `20a6e37fbe540de5aef6fc91b2f7708475758e97` / [PR #1](https://github.com/wupojung/AIoT-DIC3-ProjectRefactor/pull/1) |
| TEAM-003 | `DESIGN.md` | `research/originals/DESIGN.md` | `ORIGINAL` | 本組既有 Proposal；日期 UNKNOWN；原始版本 | `896a485d3f9b20f4ee58daf50f8af1574a51e8e5408681104e24c67c7781a567` | 12476 | PR #1 Open；尚未進入 `main` | UTF-8 有效；PR 分支檔案與 incoming 位元組一致；研究主張未核驗 | `20a6e37fbe540de5aef6fc91b2f7708475758e97` / [PR #1](https://github.com/wupojung/AIoT-DIC3-ProjectRefactor/pull/1) |

PR #1 為 `docs/originals-intake-20261113` 對 `main` 的未合併 PR；此表明確區分 PR 分支可讀取與 `main` 已發布。沒有重複提交 PR #1 的檔案。原檔安全掃描與格式驗證範圍見 PR #1 及其提交紀錄。

回到 [Research Catalog](../README.md)。
