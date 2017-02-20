# cfn-template

# 概要

## 解決したいもの

1. 環境構築の再現性を100%にしたい
2. 開発環境と本番環境を同じにしたい
3. (new!) opslessにしてopsエンジニアを救いたい

## 解決方法

1. 1ステップでゼロからシステムを構築する
 + 前提条件はAWSアカウントのみとする
2. システム全体のバージョンを管理する
 + 開発環境で任意のバージョンを構築する
 + 本番環境で複数のバージョンを同時に構築する

# 基本的な考え方

## CloudFormationを利用する

 + AWSリソースの一括生成
  - 失敗時にロールバック(理由も出力される)
  - リソースの一括アップデートもできる
 + 記法
  - YAML形式で記述する
  - JSON形式は人間には記述しにくい/コメントが書けない
 + サブコマンドを活用する
  - aws cloudformation validate-template
  - aws cloudformation create-change-set
 + 下のレイヤーから組み立てる
  - ネットワーク
  - ミドルウェア
  - アプリケーション
 + [注意]s3のバケット名
  - 名前は全ユーザで共通。
  - かつユニークでなければならない
  - よくある名前だと弾かれる
 + [注意]パスワード生成できない
  - ランダム文字列を生成したい場合がある
  - シェルと組み合わせる

## バージョン管理

 + システム全体のバージョンとは
  - AWSリソース+アプリケーション
  - どうやって管理するか?

# AWSリソース

## ネットワーク

 1. VPC
 2. Subnet
 3. SecurityGroup
 4. RouteTable
 5. LoadBalancer
 6. Route53

## ミドルウェア

 1. RDS
 2. S3Bucket/S3Endpoint
 3. ElastiCache
 4. 等

## アプリケーション

### EC2

 1. EC2
 2. LaunchConfiguration
 3. AutoScalingGroup

#### UserData

 - UserDataはEC2インスタンス起動後に自動実行されるスクリプト。
  + docker pull/runでコンテナを実行
  + ansible/chefとか
  + アプリがよくできているなら、aws s3 cpでアプリをダウンロードして実行とか。


### ECS

 - EC2上でdockerを動かすより、リソースを効率よく使える。
 - 罠もある

### API Gateway + Lambda

 - opslessにできる(!?)
 - LambdaにDLQが追加された。
  + やっと実用レベルになったと思う。
  + 今までのLambdaはエラー処理に弱すぎた。
