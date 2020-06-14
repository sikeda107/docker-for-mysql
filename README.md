# docker-for-mysql

## 1. Docker + MySQL

```
$ cat docker-compose.yml
```

```
$ docker-compose up -d
Starting data-container ... done
Starting db-container   ... done
```

```
$ docker exec -it db-container bash
```

```
$ root@db-server:/# hostname
db-server
```

```
root@db-server:/# mysql -uroot -proot
```

```
mysql> create database mydb;
Query OK, 1 row affected (0.01 sec)
```

```
mysql> use mydb
Database changed
```

## 2. Sequel Pro

- Test Builds
  https://sequelpro.com/test-builds

```
DROP TABLE mytable;

CREATE TABLE mytable(
  id INT PRIMARY KEY AUTO_INCREMENT COMMENT '主キー',
  jp_name VARCHAR(255) COMMENT '和名',
  en_name VARCHAR(255) COMMENT '英名',
  hoby VARCHAR(255) COMMENT '趣味',
  updated_time DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新時間',
  created_time DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '作成時間',
  deleted_time DATETIME COMMENT '削除時間',
  version INT NOT NULL DEFAULT 0 COMMENT 'ロック用更新回数'
)AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;


BEGIN;
ROLLBACK;
-- COMMIT;

SELECT *
FROM mytable;

INSERT INTO mytable
(jp_name,en_name,hoby)
VALUES
('ほげ','hoge','foofoo'),
('森嶋','Moishima','Running'),
('天野','Amano','Cooking');

UPDATE mytable
SET hoby = 'piyopiyo',
version = version + 1
WHERE id = 1;

```

```
$ls -a ./data/mydb/
.           ..          db.opt      mytable.frm mytable.ibd
```

```
$mysql> exit
Bye
$root@db-server:/# exit
exit
```

## 3. 参考記事

### 3.1. Docker

- [docker – volume-from と volumes の違いは何ですか？ - コードログ](https://codeday.me/jp/qa/20190317/427614.html)
- [docker-compose の ports 指定まとめ - Qiita](https://qiita.com/tksugimoto/items/23fcce1b067661e8aa46)
- [busybox | Docker Documentation](https://docs.docker.com/samples/library/busybox/)
- [docker-compose で作成されるものの名前を明示的に指定する方法 - Qiita](https://qiita.com/satodoc/items/188a387f7439e4ec394f)

### 3.2. MySQL

- [MySQL ：： MySQL 8.0 Reference Manual ：： 3 Tutorial](https://dev.mysql.com/doc/refman/8.0/en/tutorial.html)
- [MySQL コマンドまとめ - Qiita](https://qiita.com/merrill/items/967884c02e10bd8f32f5)
- [MySQL がデータを保管する場所　〜　データディレクトリ - 日常メモ](https://financial-it-engineer.hatenablog.com/entry/20140911/1410450256)

### 3.3. その他

- [127.0.0.1 と localhost と 0.0.0.0 の違い - Qiita](https://qiita.com/1ain2/items/194a9372798eaef6c5ab)

# 修正 2020-06-14

## volumes_from の廃止

docker-compose version 3移行 のため、名前付きボリュームを利用したデータの永続化。

```bash
# ボリュームの確認
> docker volume ls
DRIVER              VOLUME NAME
local               docker-for-mysql_dbdata
# ボリュームの削除
> docker volume rm docker-for-mysql_dbdata
```

公式: [Compose ファイル バージョン 3 リファレンス #volumes | Docker Documentation](https://matsuand.github.io/docs.docker.jp.onthefly/compose/compose-file/#volumes)