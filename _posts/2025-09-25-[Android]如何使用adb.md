# 📘 ADB 一頁筆記

## 🔍 ADB 是什麼？

**ADB = Android Debug Bridge**\
Android 官方提供的「偵錯橋接工具」，讓 **電腦 ↔ 手機**
之間建立溝通橋樑。

------------------------------------------------------------------------

## 🧩 三層架構

``` mermaid
graph TD
  A[adb client (電腦: 你打的命令)] --> B[adb server (電腦: port 5037)]
  B --> C[adbd daemon (手機: USB / TCP 5555)]
```

-   **adb client**：命令列工具（例：`adb devices`）
-   **adb server**：電腦上的背景程式，負責轉送命令
-   **adbd daemon**：手機上的守護程式，負責執行命令

------------------------------------------------------------------------

## 🔧 常用指令

  指令                           功能
  ------------------------------ ------------------
  `adb devices`                  顯示已連線的裝置
  `adb install app.apk`          安裝 APK
  `adb uninstall package.name`   移除 APP
  `adb shell`                    進入手機終端機
  `adb logcat`                   查看系統 log
  `adb push local remote`        電腦 → 手機傳檔
  `adb pull remote local`        手機 → 電腦取檔

------------------------------------------------------------------------

## 🔌 兩種工作模式

### 1. USB 模式（預設）

-   透過 USB 傳輸線連接
-   穩定、安全、快速
-   適合日常開發

### 2. Wi-Fi 模式（TCP/IP）

1.  插 USB，確認手機已連線：

    ``` bash
    adb devices
    ```

2.  切換到 TCP 模式：

    ``` bash
    adb tcpip 5555
    ```

3.  查手機 Wi-Fi IP：

    ``` bash
    adb shell ip addr show wlan0
    ```

4.  連線：

    ``` bash
    adb connect <手機IP>:5555
    ```

📌 注意：\
- 手機與電腦必須在**同一網段**\
- 重開機後需重新設定\
- 安全性較低（同網段的人都能嘗試連線）

------------------------------------------------------------------------

## ⚠️ 常見問題排查

-   **`device not found`**\
    → 檢查手機是否開啟「USB 偵錯」\
    → 是否有驅動程式\
-   **`10060 連線失敗`**\
    → 電腦與手機不在同網段\
    → 公司網路防火牆阻擋 TCP:5555\
-   **只能 USB，不行 Wi-Fi**\
    → 手機 `adbd` 沒成功開 port\
    → `adb connect` 前先確認：\
    `bash     adb shell netstat -an | findstr 5555`

------------------------------------------------------------------------

## ✅ 總結

-   **ADB = 橋樑**：電腦命令 → adb server → 手機 adbd → 執行
-   支援 **USB 模式**（最穩定）與 **Wi-Fi 模式**（最方便）
-   Android Studio 等 IDE 都是透過 ADB 來安裝、除錯 APP
