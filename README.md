# 台股 ETF 新手入門：LLM Wiki 與教學型 HTML 簡報

本專案透過 **LLM Wiki** 架構整理 5 篇台股 ETF 新手入門資料，並設計 **Graph + 雙檢查關卡 Loop** 的 AI 工作流程（詳見 `AGENTS.md`），自動生成結構清晰、風格一致且具備來源溯源的教學型 HTML 簡報。

## 專案亮點
- **完整的 LLM Wiki 結構**：包含 `raw/`（5 篇原始文章）、`wiki/`（5 篇結構化概念頁＋雙向關聯連結＋`index.md` 索引＋`log.md` 日誌）。
- **明確的資料對應**：簡報每一頁皆標註對應的 `wiki/*.md` 來源，杜絕 AI 幻覺。
- **可落地的檢查與反思紀錄**：完整保留 `workflow/outline.md` 與 `workflow/check_report.md`，記錄實際執行抓錯與優化流程的歷程。

## 檔案導覽
- 線上簡報入口：`index.html`
- AI 規則與流程定義：`AGENTS.md`
- 知識庫總索引：`wiki/index.md`
- 實際檢查紀錄：`workflow/check_report.md`
