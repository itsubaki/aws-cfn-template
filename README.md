# aws-cfn-template

# 概要

## 課題


## やりたいこと

1. 環境構築の再現性を100%にしたい
2. 開発環境と本番環境を同じにしたい
3. opslessにしたい
  + 全部フルマネージドサービス使えばある程度はなくなる。
  + BeanstalkやGoogleAppEngineですむならそれが一番いい選択肢
  + ある程度自由にリソースを使いたい場合はCloudFormation、Terraform等。
  + 設定変更やマイグレーション等のopsはlessにできない(？)

## 解決方法

1. 1ステップでゼロからシステムを構築する
  + 前提条件はアカウントのみとする
2. システム全体のバージョンを管理する
  + 開発環境で任意のバージョンを構築する
  + 本番環境で複数のバージョンを同時に構築する

# CloudFormation

 + AWSリソースの一括生成
   - 失敗時にロールバック(理由も出力される)
   - リソースの一括アップデートもできる
 + 記法
   - YAML形式で記述する
   - JSON形式は人間には記述しにくい/コメントが書けない
   - policyは相変わらずjson...
 + サブコマンドを使う
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

## AWSリソース

### ネットワーク

 1. VPC
 2. Subnet
 3. SecurityGroup
 4. RouteTable
 5. LoadBalancer
 6. Route53

### ミドルウェア

 1. RDS
 2. S3Bucket/S3Endpoint
 3. ElastiCache
 4. 等

### アプリケーション

#### EC2

 1. EC2
 2. LaunchConfiguration
 3. AutoScalingGroup

 - UserData
 - UserDataはEC2インスタンス起動後に自動実行されるスクリプト。
   + docker pull/runでコンテナを実行
   + ansible/chefとか
   + aws s3 cp → rpm -ivhとか

#### ECS

 - EC2上でdockerを動かすよりリソースを効率よく使える。
 - 管理が大変そうではある。

#### API Gateway + Lambda

 - GAEに近い。
 - opslessにできる(?)
 - LambdaにDLQが追加された。
   + 実用レベルになったと思う。
   + 今まではエラー処理に弱すぎた。
