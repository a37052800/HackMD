# 資料庫安裝及遷移
_by 楊婕_
## Redis
### 安裝
```bash
apt install redis-server
```
### 啟動
```bash
redis-server
redis-cli # 連結客戶端
```
### 備份
```
SAVE: 中斷連線，原始的程序直接做存儲的動作，直接到備份完成。
127.0.0.1:6379> SAVE 
OK
BGSAVE: 生產子程序來執行備份作業
127.0.0.1:6379> BGSAVE
後台保存開始
# 保存後檔案位置為:/var/lib/redis
```
## MongoDB
### 安裝
```bash
sudo apt install gnupg
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \
   --dearmor
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg] \
http://repo.mongodb.org/apt/debian bullseye/mongodb-org/6.0 main" | \
sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update
sudo apt install -y mongodb-org
```
```
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections

```
### 啟動
安裝成功後不需啟動server，直接進入資料庫即可
```bash
mongosh
# apt install systemctl

# 語法與sql不同:
db.people.insertOne( {
   user_id: "abc123",
   age: 55,
   status: "A"
} )
```
### 備份
```bash
mongodump -d {資料庫名稱} -o {../data/backup}
```
## PosterSQL
### 安裝
```bash
apt install postgresql postgresql-client
```
### 啟動
```bash
service postgresql start
```
建立資料庫
```bash
su - postgres # 不可使用root權限
createdb -U postgres -O <DB的OwnerName> <DatabaseName>
psql -l # 確認是否建立其資料庫，查看目前的資料庫
```
建立資料表(sql語言)
```sql
CREATE TABLE Customer
(First_Name char(50),
Last_Name char(50),
Address char(50),
City char(50),
Country char(25));
```
查看資料表欄位及內容
```sql
select * from 資料表名稱;
```
查看全部的資料表:
```sql
\dt+
SELECT * FROM Information_Schema.TABLES;
```
### 備份
```bash
pg_dump [資料庫名稱] -U [帳號] -f [備份檔案名稱.backup.sql]
```
備份後檔案位置在其根目錄/backup
## MariaDB
### 安裝
```bash
apt update
apt install mariadb-server
mysql_secure_installation
```
### 啟動
```bash
service mariadb start
```
### 備份
```bash
mysqldump -u root database_name > ./backup.sql
```
## MySQL
### 安裝
```bash
wget http://repo.mysql.com/mysql-apt-config_0.8.13-1_all.deb
sudo apt install ./mysql-apt-config_0.8.13-1_all.deb
sudo apt-key del A4A9 4068 76FC BD3C 4567  70C8 8C71 8D3B 5072 E1F5
# 一定要運行
sudo apt-key adv --keyserver pgp.mit.edu --recv-keys 467B942D3A79BD29       
# 會跑比較久，要等他，憑證要成功才可以安裝，否則debain會擋安裝程序
sudo apt update
sudo apt install mysql-server
sudo systemctl status mysql
# 檢查mysql是否啟動，docker內執行其他 systemctl 會error
```
### 啟動
```bash
mysqld --user=root &
```
### 備份
```bash
mysqldump -d 資料庫名稱 -u root > {備份的目錄位置}
```
出去後再重新進來要執行mysqld --user=root & 之前，需重啟資料庫
```bash
mysqld stop
mysqld start
# 執行完畢即可重新執行mysqld --user=root &
```
## SQLite
### 安裝
```bash
apt install sqlite3
```
### 啟動
```bash
sqlite3 {資料庫名稱}
```
創建資料表
```sql
sqlite> create table tbl1(one varchar(10), two smallint);
sqlite> insert into tbl1 values('hello!', 10);
sqlite> insert into tbl1 values('goodbye', 20);
sqlite> select * from tbl1;
hello!|10
goodbye|20
```
### 備份
```sql
sqlite> .output test.sql
sqlite> .dump
sqlite> .output stdout
```
[sqlite數據庫備份&還原、導出&導入](https://blog.51cto.com/u_12468355/3427716)
[Day-13 資料庫篇 - SQLite(1)](https://ithelp.ithome.com.tw/articles/10206146)
## Elasticsearch
### 安裝

### 啟動

### 備份
