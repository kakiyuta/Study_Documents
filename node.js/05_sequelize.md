# Sequelize

# Sequelizeとは

npmのORM。

# Getting started

本体インストール

```
$ npm install --save sequelize
```

DBの対応したドライバーをインストール

以下mysqlの場合

```
$ npm install --save mysql2
```



## ツール

- `sequelize-cli`
  - CLIツール
  - 構成ファイル作成
    - config
    - models
    - migrations
    - seeders
  - モデル作成
    - 既存テーブルから自動で作成するには別ツール(`sequelize-auto`)が必要
  - マイグレーション
    - 作成
    - 実行
    - ロールバック
  - シーダー
    - 作成
    - 実行
- ``sequelize-auto``
  - モデル、シーダーを既存テーブルから自動作成

## sequelize-cli

インストール

```
$ npm install sequelize-cli
```

初期化（デフォルトの構成ファイル作成）

```
$ ./node_modules/sequelize-cli/lib/sequelize init
```

configフォルダ内の設定ファイル(json)を環境に合わせて編集する

## sequelize-auto

グローバルにインストール

```
$ npm install -g sequelize-auto
```

グローバルにドライバーインストール

```
$ npm install -g mysql
$ npm install -g mysql2
```

モデル作成

```
$ sequelize-auto -h <ホスト名> -d <データベース名> -u <ユーザ名> -x <パスワード> --dialect mysql -o './models'
```



## 参考ページ

- [Manual | Sequelize](https://sequelize.org/v5/)
  - 公式サイト

