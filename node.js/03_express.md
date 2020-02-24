# Express

## 仕組み

- routing
  - 外部からのHTTP(S)リクエストに対して、内部のロジックをマッピングする
- middleware
  - routingの過程でなんらかの処理を差し込む仕組み
  - 共通処理（複合化、暗号化、認証、リクエストデータ加工）を本来のロジックから分離して、コードベースを健全に保つ

## コード修正時の即時反映

コードを変更してもすぐにブラウザに反映されない。反映するにはサーバの再起動が必要。

そこを楽にするため、コード変更時に自動でサーバの再起動を実施してくれる`nodemon`が便利。



### インストール

```
$ npm install -D nodemon
```

※基本は開発時に使用するので、`-D`をつける。

### 実行

`npm start`で実行できるようにpackage.jsonのscriptを変更する

```
“start”: “nodemon ./bin/www”
```

## ログ

本格的なロギングを行うには別途ライブラリが必要そう。

- `log4js`など

デバックで用いるログはjavascriptでおなじみの

```javascript
console.log("foooooo")
```

を用いれば、`npm start`している画面に出力でいる。

## 参照URL

- [express実践入門](https://gist.github.com/mitsuruog/fc48397a8e80f051a145)
- [【課題】expressでルーティング設定する【Router】 | Web白熱教室](https://tsuyopon.xyz/2019/02/28/js-excercise-for-backend-5/)
- [Node.jsでClassを別ファイルに分割する(2017) - Qiita](https://qiita.com/kznr_luk/items/e49b9a11872fae606e1a)
- [【APIサーバー】データ一覧取得するAPIを追加【Controller実装】 | Web白熱教室](https://tsuyopon.xyz/2019/03/08/create-a-controller-and-router-and-implement-get-todos/)
  - JSONの返却方法の処理が参考になる

