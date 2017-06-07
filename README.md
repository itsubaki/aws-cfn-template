# aws-cfn-template

## About CloudFormation

 - AWSリソースの一括生成が可能
   + 失敗時にロールバックが可能
 - 一括アップデートが可能
   + change-setを作成できる。
   + テンプレート更新>change-set作成>レビュー>適用
 - CFNでできないことはシェルなどと組み合わせる。
 - CFNを覚えたらコンソールからリソースの操作はやめよう。
 - 一般的なWebアプリケーション構成ならbeanstalkを使おう。

## 全般

 - YAMLで記述する。
   + policyは内部構造がJSONなのでJSON表記もあり。
 - アカウントリソースの作成から始める。
 - 下のレイヤーから作成する。
   + アカウント>ネットワーク>サービス(RDS, ElastiCache, ...)>アプリ
 - 環境（develop、staging、production）の切り替えができるようにする。
   + Mappings
 - バージョンの切り替えができるようにする。
   + Parameters
   + 任意のバージョンを構築できるようにする。
   + 複数のバージョンを構築できるようにする。
   + アプリケーションのバージョン管理に依存する。
 - create stack前にサブコマンドでテンプレートを確認する。
   + validate-template
   + create-change-set
 - 名前付けに気をつける。
   + スタック名は重複不可能。
   + s3のバケット名は重複不可能。全ユーザ間で共有。
   + ランダム文字列の生成はできない。(初期パスワード生成などで困る)
 - テンプレートは適度に分割する。
   + !Subなどで依存関係を解決する。
 - ステートレスなEC2リソースではディスクの自動削除をONにする。
   + デフォルトはOFF
   + OFFだとオートスケールするたびにディスクリソースが増えていく。

## AWSリソース

### ネットワーク

 | リソース名        | 説明 |
 |:----------------|:-----|
 | VPC             |      |
 | Subnet          |      |
 | SecurityGroup   |      |
 | RouteTable      |      |
 | LoadBalancer    |      |
 | Route53         |      |

### サービス

 | リソース名        | 説明 |
 |:----------------|:-----|
 | RDS             |      |
 | ElastiCache     |      |
 | S3Bucket        |      |
 | S3Endpoint      |      |
 | ...             |      |

### アプリケーション

#### EC2

 | リソース名            | 説明 |
 |:--------------------|:-----|
 | EC2インスタンス       |      |
 | LaunchConfiguration |      |
 | AutoScalingGroup    |      |

 - UserData
   + UserDataはEC2インスタンス起動後に自動実行されるスクリプト。
   + UserDataを使ってアプリのデプロイなどを行う。
 - アプリのデプロイ
   + docker pull/run。
   + ansible/chef。
   + aws s3 cp → rpm -ivh。
   + ...

#### ECS

 - コンテナは実装が複雑怪奇すぎ。
   + プロダクションでは使いたくない。
   + まともに運用できるのか謎。

#### API Gateway + Lambda

 - DLQ追加で実用レベルになった?
