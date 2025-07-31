# 🔁 自動推薦商品輪播模組

本專案用於每日自動化推送推薦商品資料，並整合進 Cyberbiz 的 `404.liquid` 錯誤頁中，以強化顧客導流與促購。

## 📌 專案功能特色

- 自動從 Google 試算表中的商品資料 (`feed` 工作表) 擷取符合條件的商品
- 過濾 `out of stock` 庫存狀態
- 支援多組商品分類 (`product_type`) 篩選（來自 `filtered` 工作表）
- 每日執行 Google Apps Script 自動產出 JSON，推送至 GitHub Pages
- 前端自動讀取 GitHub JSON 並顯示為 Swiper 輪播
- 完整響應式設計（RWD），支援桌機與手機
- 價格顯示邏輯：
  - 若有優惠價 → 顯示「原價（刪除線、灰字）」與「優惠價（紅字）」
  - 若無優惠價 → 僅顯示「原價（黑字）」

## 📂 JSON 結構說明

推送至 GitHub 的 `recommended_products.json` 檔案包含以下欄位：

| 欄位名稱       | 說明               |
|----------------|--------------------|
| `title`        | 商品名稱           |
| `image_link`   | 商品主圖網址       |
| `link`         | 加入 UTM 的跳轉連結 |
| `price`        | 原價（已處理為 NT$+整數） |
| `sale_price`   | 優惠價（同上）     |
| `brand`        | 品牌（可選）       |

## 📦 JSON 檔案位置

推送至 GitHub 倉庫：recommended_products.json

## 💻 前端整合方式

推薦商品輪播區塊已整合進 Cyberbiz 的 `404.liquid` 錯誤頁，具備以下特性：

- 使用 Swiper.js 製作左右滑動效果
- 每行顯示 1～6 商品（依螢幕寬度自動調整）
- 「你可能會喜歡的商品」標題置中顯示
- 商品卡片高度統一，風格與 Cyberbiz 商品頁一致
- 樣式與邏輯已寫入 `{% raw %}...{% endraw %}` 區塊中

## 🔁 自動化流程架構

```plaintext
Google Sheets
     ↓（Apps Script 每日自動執行）
GitHub Pages（recommended_products.json）
     ↓（前端自動 fetch）
Cyberbiz 404.liquid 顯示推薦商品

## 🛠 如何維護

- 若需新增篩選邏輯或欄位，請調整 Google Apps Script 中 \`exportFilteredJsonToGitHub()\` 函式
- 若需更改樣式，可直接編輯 \`404.liquid\` 中的 CSS 區塊與 JS 模板
- 可搭配 GitHub Actions 進一步實現 webhook 通知或每日定時觸發
