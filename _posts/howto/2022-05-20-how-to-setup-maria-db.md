---
layout: post
title: "MariaDBのセットアップ"
categories: "howto"
---

開発環境のための構築手順をまとめる


## 初期設定

1. 設定ファイルを変更する
    - C:\Users\your user\scoop\apps\mariadb\current\data\my.ini
    ```
    [mysqld]
    datadir=C:/Users/ioiso/scoop/apps/mariadb/current/data
    [client]
    plugin-dir=C:/Users/ioiso/scoop/apps/mariadb/current/lib/plugin
    ```
   ↓
    ```
    [mysqld]
    datadir=C:/Users/ioiso/scoop/apps/mariadb/current/data
    port=3306
    innodb_buffer_pool_size=2004M
    character-set-server=utf8
    [client]
    port=3306
    plugin-dir=C:/Users/ioiso/scoop/apps/mariadb/current/lib/plugin
    ```
2. 管理者権限でコマンドラインから下記コマンドを実行する
    ```
    mysqld --install
    ```
3. サービス開始する
4. rootパスワードを設定する
    ```
    mysql -u root
    ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';
    ```
5. 設定を確認する
    ```
    mysql --help
    ```

---

- 「テーブルが破損している可能性があります」というエラーがでたら
    ```
    #1548 - Cannot load from mysql.proc. The table is probably corrupted
    ```
  MySQLサーバーのバージョンとデータとの不整合を正す必要がある。

  コマンド
    ```
    mysql_upgrade -u root -p
    ```
  <https://3.1415.jp/fovoljgm/>

## fundanalyzer

### ユーザ

```
create user 'fundanalyzer'@'localhost' identified by 'fundanalyzer';
grant all privileges on * . * to 'fundanalyzer'@'localhost';
```

### データベース

```
create database fundanalyzer;
```

### インポート

```
mysql -u fundanalyzer -p fundanalyzer < xxxxxxxx_fundanalyzer_backup.txt
```
