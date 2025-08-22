# ✅ postgresql 資料庫備份與匯入

## 🎯 問題描述
詳細說明 psotgresql 資料庫備份與匯入方式。

## 操作

``` bash
1. 開啟命令提示字元，輸入psql備份指令同時指定備份路徑
C:\Windows\system32> pg_dump -U postgres -d concert_test -f "D:\NickFu2021\TicketSystemApi\document\concert_test_backup.sql"

2. 進入 psql 模式，列出所有資料庫
C:\Windows\system32> psql -U postgres
postgres=# \l

3. 刪除資料庫
postgres=# DROP DATABASE concert_test;
(如有出現 ERROR:  database "concert_test" is being accessed by other users 請先執行 3.1)

3.1 剔除所有正在使用的連線，並關閉資料庫開發工具，例:pgAdmin4
postgres=# SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'concert_test' AND pid <> pg_backend_pid();

4. 離開psql模式，並建立資料庫
postgres=# \q 
C:\Windows\system32> createdb -U postgres concert_test

5. 匯入資料庫
C:\Windows\system32> psql -U postgres -d concert_test -f "D:\NickFu2021\TicketSystemApi\document\concert_test_backup.sql"


```
