# PostgreSQL 筆記：關聯式 vs 物件關聯式資料庫設計比較

## 📦 應用情境

要設計一套系統來儲存「使用者（User）」及其多個「地址（Address）」的資料。

---

## 🔹 一、傳統關聯式資料庫設計（Relational Database）

### 📄 資料表設計：

```sql
-- 使用者表
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT,
  email TEXT
);

-- 地址表（與使用者為一對多關係）
CREATE TABLE addresses (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id),
  address_line TEXT,
  city TEXT,
  country TEXT
);
```

### 📌 特性：
- 每張表為獨立實體
- 使用外鍵（user_id）建立資料間關聯
- 所有查詢須透過 JOIN 完成

---

## 🔸 二、物件關聯式資料庫設計（Object-Relational DB）

PostgreSQL 支援以下進階設計方式：

### ✅ 方法一：使用複合型別（Composite Type）

```sql
-- 自訂型別
CREATE TYPE address_type AS (
  address_line TEXT,
  city TEXT,
  country TEXT
);

-- 使用者表，包含 address_type 陣列
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT,
  email TEXT,
  addresses address_type[]
);
```

#### 🧪 插入資料範例：

```sql
INSERT INTO users (name, email, addresses)
VALUES (
  'Alice', 'alice@example.com',
  ARRAY[
    ROW('住家地址1', 'Taipei', 'Taiwan')::address_type,
    ROW('公司地址2', 'Hsinchu', 'Taiwan')::address_type
  ]
);
```

---

### ✅ 方法二：使用 JSONB 儲存陣列資料

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT,
  email TEXT,
  addresses JSONB
);
```

#### 🧪 插入資料範例：

```sql
INSERT INTO users (name, email, addresses)
VALUES (
  'Bob', 'bob@example.com',
  '[
    {"address_line": "123 Main St", "city": "Taipei", "country": "Taiwan"},
    {"address_line": "456 Office Rd", "city": "Tainan", "country": "Taiwan"}
  ]'
);
```

---

## 🧾 小結比較

| 特性 | 關聯式資料庫 | 物件關聯式資料庫 |
|------|---------------|------------------|
| 多個地址儲存方式 | 多張表＋關聯外鍵 | 陣列、自定型別、JSON |
| 資料結構彈性 | 嚴謹但不靈活 | 彈性高、接近物件導向 |
| 查詢方式 | JOIN 多張表 | 可直接查詢巢狀欄位 |
| 優點 | 易管理、大型系統結構清晰 | 高彈性、適合中小型系統 |
| 適用場景 | 高度結構化系統 | 快速開發、複雜資料結構 |

---

## 📚 備註

- PostgreSQL 是典型的物件關聯式資料庫（ORDBMS），支援多種物件導向資料特性
- 自訂型別、陣列、JSON 可提升模型彈性與可讀性，但需注意資料正規化與維護成本
