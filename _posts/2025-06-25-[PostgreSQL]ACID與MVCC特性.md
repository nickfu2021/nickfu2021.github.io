# PostgreSQL 筆記：ACID 與 MVCC

## 📘 一、ACID 概念

ACID 是資料庫交易（Transaction）處理的四個核心屬性，用來確保資料一致性與可靠性。

| 屬性 | 中文名稱 | 說明 |
|------|----------|------|
| A    | 原子性（Atomicity） | 交易中的所有操作要「全部成功」或「全部失敗」 |
| C    | 一致性（Consistency） | 交易執行後，資料須符合所有約束與規則 |
| I    | 隔離性（Isolation） | 多筆交易同時執行時彼此不干擾 |
| D    | 持久性（Durability） | 一旦交易成功提交，資料就永久保留，即使系統當機也不遺失 |

### 🔹 PostgreSQL 如何實現 ACID

- **原子性**：使用 `BEGIN` / `COMMIT` / `ROLLBACK` 控制整體交易
- **一致性**：透過 schema 限制（如 `NOT NULL`、`CHECK`、`FOREIGN KEY`）保障資料規則
- **隔離性**：透過 MVCC 與不同隔離等級實作
- **持久性**：使用 WAL（Write-Ahead Logging）寫入日誌以防止資料遺失

---

## 📗 二、MVCC（Multi-Version Concurrency Control）

### 📌 MVCC 是什麼？

MVCC 是一種併發控制技術，讓交易在不互相阻塞的情況下，同時安全地讀寫資料。

> PostgreSQL 使用 MVCC 作為核心並發控制機制，達成「讀不擋寫、寫不擋讀」。

---

### 📍 PostgreSQL 的 MVCC 實作原理

1. **每筆資料有多個版本**：
   - `UPDATE` 不會直接改資料，而是新增新版本，舊版本被標記為過期。
   - `DELETE` 將資料標記為刪除，但資料仍存在。

2. **每筆資料有隱藏欄位**：
   - `xmin`：資料建立者的交易 ID
   - `xmax`：資料終止者的交易 ID（如被刪除或更新）

3. **交易快照（Snapshot）**：
   - 每筆交易 `BEGIN` 時都會建立自己的快照。
   - 所有查詢僅看到與該快照相符的資料版本。

4. **VACUUM 清理過時資料**：
   - PostgreSQL 定期執行 `VACUUM` 或 `autovacuum`，移除無用資料版本以釋放空間。

---

### 🧪 MVCC 範例說明

假設 A、B 同時操作資料表：

- A：`BEGIN; SELECT * FROM product;`
- B：`BEGIN; UPDATE product SET price = 100; COMMIT;`
- A：再次 `SELECT` 時仍會看到更新前的資料版本

這代表 A 的交易快照不受 B 的交易影響。

---

### ✅ MVCC 優點

- 提升併發處理效能
- 讀不擋寫、寫不擋讀
- 保證交易快照一致性（Snapshot Isolation）

### ❌ MVCC 缺點

- 舊資料版本堆積造成資料膨脹（bloat）
- 需依賴 VACUUM 管理版本清除
