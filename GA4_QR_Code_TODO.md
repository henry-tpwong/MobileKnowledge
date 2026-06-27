# GA4 + UTM QR Code 轉化率追蹤 — 實作檢查清單

> 以下步驟對照 `Universal_Links_App_Links.md` 第十二章執行，章節編號為該文件內對應說明。

---

- [ ] **1. 建立 GA4 帳號與資源**（見第十二章 `## 4.`）
  - 到 [analytics.google.com](https://analytics.google.com) 以 Google 帳號登入
  - 建立新帳戶 → 建立新資源（名稱：QR Code 著陸頁）
  - 選擇「網站」平台 → 輸入網址 `galaxyresortsapp.com` → 建立串流
  - 取得 **Measurement ID**（格式 `G-XXXXXXXXXX`），後面步驟會用到

---

- [ ] **2. 建立 DL Landing Page**（見第十二章 `## 5.`）
  - **方案 A**（靜態 HTML）：建立 `/dl/index.html` 部署在 `galaxyresortsapp.com` 的 `/dl/` 路徑下，內容含 GA4 追蹤碼（gtag.js）、`qr_scan` + `download_click` 事件、OS 偵測跳轉商店
  - **方案 B**（Drupal，公司已有 CMS 可選用）：安裝 `google_tag` 模組 → 後台填入 Measurement ID 自動注入 gtag.js → 新增 Basic Page 貼上自訂事件 JS → 設定 alias 為 `/dl`（詳見第十二章 `## 5. III.`）
  - 注意：此頁面獨立於 Clock-in QR Code 使用的 `/clock-in/` fallback 頁面

---

- [ ] **3. 更新 AASA / assetlinks.json 與 Android intent-filter**（見第十二章 `## 7.`）
  - **iOS AASA**：將 `components` 從 `"/*"` 改為具體業務路徑列表（如 `/clock-in/*`、`/booking/*`），**排除 `/dl/`**
  - **Android assetlinks.json** + **AndroidManifest.xml intent-filter**：加上 `android:pathPrefix` 限制 deep link 範圍，不覆蓋 `/dl/`
  - 目的：確保 `/dl/` 不被 Universal Links / App Links 攔截，瀏覽器一定載入此頁面

---

- [ ] **4. 為各地點產生帶 UTM 參數的 QR Code**（見第十二章 `## 6.`）
  - 每個據點使用獨立 URL，格式：
    ```
    https://galaxyresortsapp.com/dl/?utm_source=qr&utm_medium=offline&utm_campaign=app_download&utm_content=地點代號
    ```
  - 用任何 QR Code 產生器產生對應 QR Code 圖片
  - 建議印刷尺寸至少 2cm × 2cm

---

- [ ] **5. 部署後測試驗證**（見第十二章 `## 8.`）
  - 先建立一個測試 QR Code（`utm_content=test`）
  - 用手機掃描，打開 GA4 後台 → 即時報表 → 確認 `qr_scan` 事件在數秒內出現
  - 確認點擊商店連結後 `download_click` 事件也被記錄
  - 測試完成後才大量印製各地點 QR Code

---

- [ ] **6. 建立 GA4 報表查看轉化率**
  - **各地點掃碼數**：探索 → 自由形式 → 列 = 「工作階段手動內容」、值 = 事件計數（`qr_scan`）
  - **各地點下載數**：同上，篩選器改為事件 `download_click`
  - **轉化漏斗**：探索 → 漏斗探索 → 步驟 1 = `qr_scan`、步驟 2 = `download_click`，依 `utm_content` 區隔各地點
  - 轉化率公式：`download_click ÷ qr_scan × 100%`

---

## 注意事項

| 項目 | 說明 |
|------|------|
| Clock-in QR vs DL QR | Clock-in QR（`/clock-in/`）是業務功能，不需 GA4、不需 UTM。DL QR（`/dl/`）才是追蹤用 |
| 轉化率偏低 | `qr_scan` 包含已裝 App 者，他們不會觸發 `download_click`，轉化率數字為保守估計 |
| GA4 延遲 | 標準報表需 24-48 小時，先用「即時」確認事件正確 |
