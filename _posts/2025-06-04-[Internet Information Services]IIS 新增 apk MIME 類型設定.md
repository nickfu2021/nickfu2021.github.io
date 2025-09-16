# 📘 IIS 新增 `.apk` MIME 類型設定筆記

## 🧩 問題描述

在 IIS 中，如果使用者點擊 `.apk` 檔案連結時出現以下錯誤訊息：

HTTP 錯誤 404.3 - Not Found
因為網頁伺服器上設定的 MIME 對應原則，而無法提供您要求的網頁。

![HTTP 錯誤 404.3](https://i.ibb.co/NntWZHr7/2025-06-04-161551.jpg "HTTP 錯誤 404.3")

表示目前的 IIS 尚未支援該副檔名的 MIME 類型，需要手動新增設定。

---

## ✅ 解決方法

---

## 🔧 在 IIS 6 中新增 `.apk` MIME 類型

### 📍 步驟

1. 開啟「**Internet Information Services (IIS) 管理員**」
   - 開始 → 控制台 → 管理工具 → Internet Information Services

2. 在左側樹狀目錄中，**右鍵點選網站名稱**（例如 `Default Web Site`），選擇「**內容（Properties）**」

3. 點選「**HTTP Headers**」分頁

4. 點選中間的「**MIME Types...**」按鈕

5. 點選「**New...**」，輸入以下內容：
   - **副檔名（Extension）**：`.apk`
   - **MIME 類型（MIME type）**：`application/vnd.android.package-archive`

6. 點選「確定」，然後關閉內容視窗

7. **重新啟動 IIS** 或該網站：
   - 在 IIS 管理員中右鍵網站 → 停止 → 再啟動

---

## 🖥 在 IIS 10 中新增 `.apk` MIME 類型

### 📍 方法一：透過 IIS 管理工具設定

1. 開啟「**IIS 管理員（Internet Information Services Manager）**」

2. 在左側選擇你要設定的網站（例如 `Default Web Site`）

3. 在中間面板中，雙擊「**MIME 類型（MIME Types）**」

4. 右側操作欄點選「**新增（Add）**」

5. 輸入：
   - **副檔名（File name extension）**：`.apk`
   - **MIME 類型（MIME type）**：`application/vnd.android.package-archive`

6. 確認後儲存並重新啟動網站

---

### 📍 方法二：編輯 `web.config`

如果你使用的是 ASP.NET 或 MVC 專案，可以直接在 `web.config` 加入下列設定：

```xml
<system.webServer>
  <staticContent>
    <mimeMap fileExtension=".apk" mimeType="application/vnd.android.package-archive" />
  </staticContent>
</system.webServer>
重新部署應用或重啟網站即可生效。

✅ 驗證方式
開啟瀏覽器，點擊指向 .apk 的連結

正常情況下應會出現下載提示：「開啟或儲存檔案」

🧠 備註與建議
若要支援其他副檔名（例如 .json, .woff2, .mp4），也需個別設定 MIME 類型

IIS 6 已停止支援，建議儘早升級至 IIS 10 以上版本以確保安全與相容性

