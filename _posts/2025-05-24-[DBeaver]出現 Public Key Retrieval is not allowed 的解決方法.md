# 🛠️ DBeaver 出現 Public Key Retrieval is not allowed 的解決筆記

---

## ❓ 問題描述

當你使用 DBeaver 連線 MySQL 8（含以上版本）時，會出現：

```
Public Key Retrieval is not allowed
```

這是因為 MySQL 8 的帳號驗證方式預設為 `caching_sha2_password`，而 JDBC 連線預設不允許自動擷取加密金鑰。

---

## ✅ 解決方法：修改 DBeaver 連線屬性

### 步驟：

1. 開啟你要連線的 MySQL 連線設定
2. 點選右上角「Driver Properties」分頁
3. 點「新增屬性」（Add Property）
4. 新增以下兩個屬性：

| 名稱 | 值 |
|------|----|
| `allowPublicKeyRetrieval` | `true` |
| `useSSL` | `false`（可選） |

5. 儲存並重新測試連線

---

## 🧪 替代解法（不建議正式使用）

若你無法修改 DBeaver 驅動屬性，也可以改用舊版加密方式：

```sql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '你的密碼';
```

> ⚠️ 不建議在生產環境中使用 `mysql_native_password`，因安全性較低。

---

## ✅ 小提醒

- 這是 **JDBC 驅動與 MySQL 8 驗證方式不相容造成的**
- 改用 `allowPublicKeyRetrieval=true` 是官方解法之一

---

以上即可解決 DBeaver 的連線問題 ✅