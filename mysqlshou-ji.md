# MySQL 常用指令
### 通过IP Port和网络协议访问

```bash
mysql -uroot -p123456 --host=127.0.0.1 --port=3306 --protocol=TCP
```

### dump

```bash
mysqldump -uroot -proot --all-databases >/tmp/all.sql
mysqldump -uroot -proot --databases db1 db2 >/tmp/user.sql
mysqldump -uroot -proot --databases db1 --tables a1 a2  >/tmp/db1.sql
mysqldump -uroot -proot --databases db1 --tables a1 --where='id=1'  >/tmp/a1.sql
# 只导出表结构
mysqldump -uroot -proot --no-data --databases db1 >/tmp/db1.sql
```

### load