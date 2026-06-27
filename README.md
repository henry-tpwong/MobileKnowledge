# MobileKnowledge

行動裝置 Deep Link（Universal Links / App Links）完整知識庫與實作文件。

## 內容

| 文件 | 說明 |
|------|------|
| [Universal_Links_App_Links.md](./Universal_Links_App_Links.md) | iOS Universal Links + Android App Links 完整指南，含三大元件、流程圖、設定清單、微信掃碼處理、GA4 轉化率追蹤 |
| [GA4_QR_Code_TODO.md](./GA4_QR_Code_TODO.md) | GA4 + UTM QR Code 追蹤實作檢查清單 |

## 涵蓋主題

- URL Scheme vs Universal Links / App Links
- AASA / assetlinks.json 驗證機制
- Landing Page fallback 設計
- .well-known 路徑與 Core Router 設定
- 微信（WeChat）QR Code 掃碼喚起 App
- GA4 + UTM 參數追蹤 QR Code 據點轉化率
- Firebase Analytics（iOS/Android）App 內追蹤

## 對象

iOS / Android 開發者、後端工程師、產品經理，需要實作 QR Code / NFC / 分享連結喚起 App 並追蹤下載轉化。

## 快速導覽

1. **剛入門？** 從 [Universal_Links_App_Links.md](./Universal_Links_App_Links.md) 第二章「URL Scheme vs Universal Links」開始
2. **要實作設定？** 跳到第五章「iOS 設定清單」或第六章「Android 設定清單」
3. **要追蹤 QR Code 轉化率？** 看第十二章「GA4 + UTM」，搭配 [GA4_QR_Code_TODO.md](./GA4_QR_Code_TODO.md) 按步驟執行
4. **中國市場有微信需求？** 直接跳到第十一章「微信 QR Code 掃碼處理」
