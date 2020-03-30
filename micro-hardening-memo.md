---
description: Memos for Micro Hardening 2.2020 @kanazawa
---

# Micro Hardening Memo

### Skills you should probably have \(to some extent\)

* ssh接続やポートフォワーディングが出来る
* psコマンドでプロセスの死活を確認出来る
* Webサーバ、syslog、ログイン試行などの主要なログを探して読める
* 必要に応じてtop、free、netstatなどを使って問題を切り分け出来る
* SQLインジェクションやDoSなど基本的な攻撃手法の知識がある
* PHPに関する知識 \(なぜか大抵はWordpressやEC-Cubeなどphpなアプリが題材になる\) 
* MySQLやApacheなどのミドルウェアの熟知 \(ただし起動方法くらいは必須\)

### 発生する攻撃

* SQLインジェクション
* アクセス制御されていない設定ファイルへのアクセス
* 脆弱なパスワードのユーザにログイン
* DoSの脆弱性を突いた攻撃
* 管理ポートへの直接接続
* 管理画面へのログイン
* バックドアの操作
* 不要な情報の表示

### 防衛手段

* パスワード変更 `passwd <username>`
* 不審なユーザの削除 `sudo userdel <username>`
* 脆弱性のあるアプリのアップデート
* 不要なサービスの停止 `service <service> stop`
* 脆弱性のあるアプリの修正
* 管理画面のアクセス制御
* 攻撃元IPアドレスのブロック

```bash
vim /etc/sysconfig/iptables
iptables -A INPUT -s 65.55.44.100 -j DROP
iptables -A INPUT -s 65.55.44.100 -p tcp --destination-port 25 -j DROP
service iptables save
Unblock: iptables -D INPUT -s xx.xxx.xx.xx -j DROP
```

* 不要なプラグインの削除
* 管理ポートのアクセス制御



