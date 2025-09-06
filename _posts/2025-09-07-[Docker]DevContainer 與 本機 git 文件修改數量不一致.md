# Git 行尾問題筆記

## 什麼是 CRLF 和 LF？

### LF (Line Feed)
- 符號：`\n`
- 作用：換行（跳到下一行）
- 使用系統：Linux、macOS、Unix 系統
- 檔案表現：
  ```
  Hello\nWorld\n
  ```

### CRLF (Carriage Return + Line Feed)
- 符號：`\r\n`
- 作用：回到行首 (CR) + 換行 (LF)
- 使用系統：Windows
- 檔案表現：
  ```
  Hello\r\nWorld\r\n
  ```

### 差異原因
- Windows 和 Linux 對「換行」的定義不同。
- 同一份程式碼，如果在 Windows 存檔 → CRLF；
  在 Linux 打開 → LF。
- Git 會把行尾不同視為「檔案有修改」，導致 `git status` modified 數量不一致。

---

## 為什麼要用 .gitattributes？
- `.gitattributes` 可以告訴 Git：**強制所有文字檔在 commit 時統一成指定的行尾格式**（通常是 LF）。
- 好處：
  - 不論在 Windows 或 Linux，最終進 Git repo 的檔案一律一致。
  - 解決 Dev Container 和本機看到的修改數量不同問題。

---

## 定稿流程指令

1. 新增 `.gitattributes`：

   ```gitattributes
   * text=auto

   # 一律用 LF（跨平台一致）
   *.cs      text eol=lf
   *.csproj  text eol=lf
   *.sln     text eol=lf
   *.json    text eol=lf
   *.yml     text eol=lf
   *.yaml    text eol=lf
   *.xml     text eol=lf
   *.md      text eol=lf
   *.txt     text eol=lf
   *.sql     text eol=lf
   *.sh      text eol=lf
   *.Dockerfile text eol=lf
   docker-compose.* text eol=lf

   # Windows 腳本保留 CRLF
   *.bat     text eol=crlf
   *.cmd     text eol=crlf
   *.ps1     text eol=crlf

   # 二進位檔不處理
   *.png binary
   *.jpg binary
   *.jpeg binary
   *.gif binary
   *.pdf binary
   *.zip binary
   *.7z  binary
   *.ttf binary
   *.woff binary
   *.woff2 binary
   ```

2. 在本機執行：

   ```bash
   git add .gitattributes
   git add --renormalize .
   git commit -m "chore: add .gitattributes and normalize line endings"
   git push
   ```

3. 在 Dev Container 中同步設定：

   ```bash
   git config --global core.filemode false
   git config --global core.autocrlf false
   git config --global core.eol lf
   git pull
   ```

---

## 結論
- **CRLF = Windows 換行**，**LF = Linux/macOS 換行**。
- `.gitattributes` 能統一行尾規則，讓 repo 乾淨一致。
- 完成定稿後，本機和 Dev Container 的 `git status` 就不會再跳動。
