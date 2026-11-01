# new-product-dev-system — 新品開發系統化工作坊

單檔靜態網頁（`index.html`＋`data.js`＋`manual.html`＋PWA）。依課程簡報〈新品發展的系統化方法〉（`SynologyDrive/簡報資料/2025/輔仁大學/20251216_Amazon電商平台銷售策略制定.pptx`）把整套流程做成 10 步驟（0–9）互動工具：產品資料 → 評論優缺點（A02）→ 客戶痛點（C04）→ 產品五層次（PR01）→ 七種創新方法（L10_4）→ 3C（A4）→ 曼陀羅（L18）→ 五星檢核表 → 創新類型（連續／動態連續／非連續）→ 概念提案＋新產品開發七階段。2026-11-01 建立。

- 本機預覽 port **8846**（`.claude/launch.json` 的 `new-product-dev-system`）。
- 與 `new-product-strategy-studio`（五層次＋SWOT＋定價＋競品）分工：本工具＝課程的「評論→痛點→創新發想→提案」系統化流程。
- 已上線：repo `M255525/new-product-dev-system`，GitHub Pages（Actions workflow，push master 自動部署）<https://m255525.github.io/new-product-dev-system/>；visitor-badge `m255525.newproductdevsystem`。

## 架構重點
- `data.js` 是規則引擎唯一真實來源：16 個評論面向 `ASPECTS`（kw／fix／unmet／checks／facet）、情緒詞（`NEUTRALIZE` 先把「不錯、不會漏」等轉正面再計數）、8 類產品模板 `CATEGORIES`（客群、五層次、屬性、逆向假設、結構維度、腦力激盪）、強迫關係／全新情境素材、曼陀羅 8 面向、創新類型、開發七階段、5 個虛構範例。
- **關鍵字不要放單一常用字**：「重」會抓到「重複／減重」、「裝」抓到「包裝」、「設計」抓到「營養師設計」，都踩過。
- 引擎：`analyzeReviews`（五星評論按逗號拆子句、回饋整行判斷）→ `buildPains` → `buildLevels` → `buildMethods` → `buildC3` → `buildMandala` → `buildChecklist` → `buildProposal`，`ORDER`＋`rebuildFrom(k)` 依序重建；下游一律讀 `eff(k)`（AI 結果優先、否則規則式）。
- **AI（BYOK）**：`aiPromptFor(k)`＝課程提示詞＋「只輸出 JSON，結構同此範例」＋規則式結果當 schema；回傳用 `conform(rule, got)` 逐鍵型別對齊，缺鍵退回規則式。檢核表另外處理（重算 id）。重新按「開始分析」會清掉 AI、勾選與手動修改。
- 使用者手動狀態：`picks`（勾選構想，null＝預設前 3 個）、`mandEdit`（格子 key `面向_序號`）、`checks`（星數／完成／備註，key＝項目文字 hash）、`extraChecks`、`itype`、`road`、`propEdit`。
- 報告：`buildPrint()` 填 `#printReport`＋`@media print` 只印報告；`buildMd()` 下載 Markdown。
- 儲存鍵前綴 `npdSys.`：settings／draft（自動保存）／projects（最多 40）／warned／setOpen／marquee。
- 標準配備：跑馬燈（工作區公告 GAS、滑鼠移入暫停）、⚙ 常用設定（收合式＋關於本工具）、警語（頁面橫幅＋首訪彈窗）、manual.html#creator 完整版、PWA、visitor-badge。
- 測試掛鉤 `window._npd`。曼陀羅 ≤700px 改為清單版；導覽 ≤980px 變橫向捲動列。

## 驗證（2026-11-01，Python Playwright）
5 個範例都能分析、10 步驟渲染、勾選構想／換一批／星數／打勾／自訂項目／創新類型／提案修改／重整後保留、9 段提示詞、Markdown 與列印 PDF（5 頁）；360／414／768 無水平捲動；無 console 錯誤。AI 真實金鑰未測。
