# AWS Lambda

## Lambdaとは

サーバの構築なしでアプリケーション（Functions）を実行できるサービス。

サービスの特徴として以下のことがあげられる

- EC2のような仮想サーバを立てる必要がない
  - **サーバレス**
- 処理件数が増えてくると自動的に実行ユニットを増やし並列にスケールアウトしてくれる
  - サーバレスの特徴の一つ
  - CPUやメモリの使用率に応じてオートスケール設定を調整する必要がない
- **稼働時間、リレーショナルデータベースとの相性なのでEC2で構築している全てのサービスをLambdaに置き換えられるわけではない！**
- API Gatewayをエンドポイント(URL)として組み合わせて利用することで簡単なWebサービスやAPIサービスを公開できる
  - 時間帯によってアクセス数の変動が多いサイトや、特別な日だけアクセス数が急増するサイトに効果的？



## ユースケース

以下のようなサービスだと向かない

- 1回の処理時間が長いタスク
- 24時間絶えずリクエストがあるWebサービス



## 使用できる言語

- python
  - 例が一番多い気がする
- Node.js
- Java
- Ruby
- C#
- Go

## 開発環境

### SAM CLI

Lambda関数をローカル環境で実行し、テストおよびデバックすることができる。

デプロイもできるらしい。

**要学習**



## 組み込みの耐障害性

引用

> Lambda は組み込みの耐障害性を備えています。AWS Lambda は各リージョンの複数のアベイラビリティーゾーン全体でコンピューティング性能を維持し、個別のマシンまたはデータセンター設備の故障からコードを保護します。AWS Lambda およびこのサービスで実行される関数は予測可能で信頼性の高い運用パフォーマンスを発揮します。AWS Lambda は、サービス自体とサービスによって運用される機能のために高可用性を発揮するよう設計されています。メンテナンスの時間帯や定期的なダウンタイムはありません。

[特徴 - AWS Lambda | AWS](https://aws.amazon.com/jp/lambda/features/)

EC2で過去発生した特定リージョンの障害にもLambdaには影響がないって解釈であってる？



## 学習

- [AWS Lambda、Amazon API Gateway、Amazon S3、Amazon DynamoDB、および Amazon Cognito を使用してサーバーレスウェブアプリケーションを構築する方法 | AWS](https://aws.amazon.com/jp/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/)
  - AWS公式サイトが紹介している学習サイト
- [JAWS-UG 初心者支部#24 でサーバーレスなハンズオン会をやりました #jawsug #jawsug_bgnr - log4ketancho](https://www.ketancho.net/entry/2020/02/20/080000)
  - 詳細は参考サイトを参照
  - ソース : https://github.com/ketancho/aws-serverless-quick-start-hands-on
  - 音声データ : https://aws.amazon.com/jp/polly/



## ~~LambdaとRDBは相性が悪い~~

~~[なぜAWS LambdaとRDBMSの相性が悪いかを簡単に説明する - Sweet Escape](https://www.keisuke69.net/entry/2017/06/21/121501)~~

↑上記サイトにも記載があるが、`Amazon RDS プロキシ`を利用することで課題が解消されている。

しかし`Amazon RDS プロキシ`はプレビュー版のため商用利用は控えた方がいいかも



[RDS Proxyを使ってAWS LambdaからRDBにコネクションプールで接続する | Developers.IO](https://dev.classmethod.jp/articles/using-relational-databases-with-aws-lambda-easy-connection-pooling/)

上記サイトに公式デモ動画へのリンクがある。



## 参考サイト

- [AWS Lambda とは - AWS Lambda](https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/welcome.html)
  - 公式サイト
- [AWS Lambda、Amazon API Gateway、Amazon S3、Amazon DynamoDB、および Amazon Cognito を使用してサーバーレスウェブアプリケーションを構築する方法 | AWS](https://aws.amazon.com/jp/getting-started/projects/build-serverless-web-app-lambda-apigateway-s3-dynamodb-cognito/)
- [AWS Lambda - Qiita](https://qiita.com/leomaro7/items/5b56ae9710d236545497#8%E3%83%A2%E3%83%87%E3%83%AB)
- [JAWS-UG 初心者支部#24 でサーバーレスなハンズオン会をやりました #jawsug #jawsug_bgnr - log4ketancho](https://www.ketancho.net/entry/2020/02/20/080000)
  - ページ下部のスライドショーが学習の教材になりそう
    - LambdaのでHello Warld
    - API Gateway <--> Lambda <--> Amazon Translateによる翻訳サービス実装
  - https://speakerdeck.com/ketancho/jaws-ug-bgnr-24-serverless-quick-start-hands-on
    - ↑スライドへの直リンク
- [なぜAWS LambdaとRDBMSの相性が悪いかを簡単に説明する - Sweet Escape](https://www.keisuke69.net/entry/2017/06/21/121501)
- [RDS Proxyを使ってAWS LambdaからRDBにコネクションプールで接続する | Developers.IO](https://dev.classmethod.jp/articles/using-relational-databases-with-aws-lambda-easy-connection-pooling/)

