# 台股 ETF 新手入門知識庫與簡報生成指南 (AGENTS.md)

## 1. 專案目的與定位
本專案建立一個「台股 ETF 新手入門」主題的 **LLM Wiki 知識庫**，並透過標準化的 **Graph + Loop 工作流程**，由 AI 嚴格依據 Wiki 內容產出可供完全新手理解的「教學型 HTML 簡報」。

### 相較於標準 Karpathy LLM Wiki 的客製化調整
1. **新手友善翻譯原則**：將原始金融術語轉化為生活化比喻（如水果籃、微笑曲線），避免過多艱澀專有名詞堆砌。
2. **嚴格禁絕外部幻覺（Strict Grounding）**：禁止 AI 自行補腦 Wiki 裡沒有的即時股價或未經證實的殖利率數字（例如不可憑空捏造「年化配息 8%」）。
3. **強制來源溯源**：每一頁簡報底部都必須標註對應的 `wiki/*.md` 檔名，確保知識庫與簡報 100% 對應。
4. **法遵防呆機制**：全站及結尾頁必須強制附上「非投資建議」警語。

---

## 2. 專案資料夾結構與命名規則
```text
ETF_LLM_Wiki_Slides/
├── AGENTS.md                 # AI 開發憲法與工作流程規範（本檔案）
├── README.md                 # 給人類閱讀的專案簡介與瀏覽說明
├── index.html                # 最終產出的教學型 HTML 簡報（GitHub Pages 入口）
├── raw/                      # 原始輸入資料（至少 5 篇，只讀不改）
│   ├── 01_what_is_etf.md
│   ├── 02_market_cap_vs_dividend.md
│   ├── 03_costs_and_tracking.md
│   ├── 04_dca_and_compounding.md
│   └── 05_risks_and_myths.md
├── wiki/                     # AI 整理維護的知識庫（結構化概念頁＋雙向關聯）
│   ├── index.md              # 知識庫總索引目錄
│   ├── log.md                # Ingest 與查詢執行日誌
│   ├── etf_basics.md
│   ├── etf_types.md
│   ├── hidden_costs.md
│   ├── dca_compounding.md
│   └── risk_management.md
└── workflow/                 # 流程中間產物與品質檢查紀錄
    ├── outline.md            # 簡報大綱與逐頁來源對應表
    └── check_report.md       # 實際執行與 Loop 修正檢查紀錄
```

---

## 3. 工作流程設計：Graph + 雙檢查關卡 Loop

```text
[Node 1: Ingest 建庫]
讀取 raw/*.md ──▶ 拆解並寫入 wiki/*.md、更新 wiki/index.md 與 wiki/log.md
       │
       ▼
[Node 2: 查詢 Wiki 規劃大綱]
僅讀取 wiki/ 內容 ──▶ 產出 workflow/outline.md（標註每頁來源與字數上限）
       │
       ▼
《Check Gate 1：大綱與知識忠實度審查 Loop（最多重試 3 輪）》
檢查項：教學由淺入深？有無 Wiki 外的幻覺數字？單頁要點 ≤ 4 點？
   ├── ❌ 未通過 ──▶ 退回 Node 2（僅修正未通過項目）
   └── ✅ 通過
       │
       ▼
[Node 3: 生成教學 HTML 簡報]
依 outline.md 與視覺規範產出 index.html
       │
       ▼
《Check Gate 2：成品質檢與排版審查 Loop（最多重試 3 輪）》
檢查項：單頁是否爆版？每頁底部有無 wiki 來源標籤？結尾有無非投資建議聲明？
   ├── ❌ 未通過 ──▶ 退回 Node 3（局部微調並記錄於 check_report.md）
   └── ✅ 通過
       │
       ▼
[Node 4: 輸出交付與紀錄]
寫入 workflow/check_report.md ──▶ 完成並準備部署 GitHub Pages
```

### 停損點設定 (Stop Condition)
- 任一 Check Gate 若連續修改超過 **3 輪** 仍無法通過，立即停止自動迴圈，並在 `workflow/check_report.md` 標註卡關原因，交由人類開發者介入判斷。

---

## 4. 簡報視覺與排版規範 (UI/UX Spec)
- **風格主題**：專業台股金融儀表板風（深藍底 `#0b0f19`＋原生內嵌擬真 K 線與均線背景 SVG＋毛玻璃卡片）。
- **台股色彩語彙**：正向／偏多／重點強調使用紅色 (`#ef4444`)；風險／提醒／下跌使用綠色 (`#22c55e`)；輔助資訊使用科技藍 (`#38bdf8`)；主文字亮白 (`#f8fafc`)。
- **防爆版規則**：每頁簡報限制在單一視窗高度內可完整閱讀，重點條列不超過 4 點，每點精簡扼要。
- **互動操作**：必須支援鍵盤左右方向鍵（`←` / `→`）、按鈕翻頁、上方進度條與快速跳頁選單。
- **強制溯源標籤**：每頁簡報底部必須顯示 `📚 知識來源：wiki/對應檔名.md`。
