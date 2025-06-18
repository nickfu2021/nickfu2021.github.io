# IIS FREB 設定筆記：追蹤錯誤與慢請求

## 📌 前置條件：已安裝 FREB 模組
- 功能名稱：`Web-Http-Tracing`
- 安裝方式（可用 Server Manager 或 PowerShell）：

```powershell
Install-WindowsFeature Web-Http-Tracing
```

---

## ✅ 啟用 FREB（失敗要求追蹤）功能

1. 開啟 IIS 管理員
2. 點選目標網站（非應用程式集區）
3. 右側點選「**失敗要求追蹤...**」
4. 勾選啟用，並確認預設儲存路徑：
   ```
   C:\inetpub\logs\FailedReqLogFiles
   ```

---

## ✅ 建立 FREB 規則：追蹤 500 錯誤 + 超過 5 秒

### 步驟一：新增規則
1. 在網站功能頁中點「**失敗要求追蹤規則**」
2. 點右側「**新增...**」
3. 選擇「**所有內容**」
4. 點【下一步】

### 步驟二：設定條件
1. 狀態碼輸入 `500`
2. 下一步 → 模組與處理階段保持預設
3. 完成規則建立

### 步驟三：編輯規則，加上處理時間門檻
1. 回到 FREB 規則畫面，右鍵剛建立的規則 →「**編輯規則**」
2. 切換到「**條件**」頁籤 → 點【新增】
3. 設定以下條件：
   - **屬性名稱**：`TIME_TAKEN`
   - **比較方式**：大於
   - **值**：`5000`（單位毫秒 = 5 秒）

---

## ✅ FREB 記錄檔路徑與查看方式

- 預設儲存在：
  ```
  C:\inetpub\logs\FailedReqLogFiles\W3SVC<網站ID>\
  ```
- 使用 Edge / IE 開啟 `.xml` 檔，可視覺化顯示報表
- 樣式檔案：`freb.xsl`，請勿刪除

---

## 🔄 補充：清除 7 天前 FREB 檔案（不刪 freb.xsl）

```powershell
$logPath = "C:\inetpub\logs\FailedReqLogFiles"
$days = 7

Get-ChildItem -Path $logPath -File -ErrorAction SilentlyContinue |
    Where-Object {
        $_.LastWriteTime -lt (Get-Date).AddDays(-$days) -and
        $_.Extension -ne ".xsl"
    } |
    Remove-Item -Force -ErrorAction SilentlyContinue
```

---

## ✅ 建議

- 建議只開啟「錯誤 + 慢請求」條件以控制磁碟使用量
- 可搭配工作排程器定期清除 FREB 檔案
