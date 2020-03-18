# RDSで新しいユーザを作成する



MySQLを実行するAWSのRDSで新しくDBのユーザを作成する方法をまとめておきます。

新しくユーザを作成し、SELECTのみを許可するためには以下を実行します。

```sql
grant select on database_name.* to 'read-only_user_name'@'%' identified by 'password';
```

database\_name、read-only\_user\_name、passwordを適当なものに置き換えてください。

### 新しいユーザを作成

CREATE USER コマンドで新しくユーザを作成します。

```sql
CREATE USER 'new_user'@'%' IDENTIFIED BY 'password';
```

new\_userとpasswordを適当なものに置き換えてください。

### アクセス許可をユーザに付与

GRANTコマンドを利用して新しく作ったユーザにアクセス許可を付与します。 すべてのアクセスを許可するためには以下を実行します。

```sql
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION;
```

DBを見るだけ編集はできないユーザを作るためにはSELECTのみを許可します。

```sql
GRANT SELECT ON *.* TO 'new_user'@'%';
```

