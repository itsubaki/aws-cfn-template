# cfn-template

## 解決したいもの

1. 環境構築の再現性を100%にしたい
2. 開発環境と本番環境を同じにしたい

## 解決方法

1. 1ステップでゼロからシステムを構築する
 + 前提条件はAWSアカウントのみとする
2. システム全体のバージョンを管理する
 + 開発環境で任意のバージョンを構築する
 + 本番環境で複数のバージョンを同時に構築する

# 基本的な考え方

## 1ステップ

 + CloudFormationを利用する
  - YAML形式で記述する
  - JSON形式は人間には記述しにくい/コメントが書けない
 + 下のレイヤーから組み立てる

## レイヤー

 + システムを以下のレイヤーに分けて考える
  - ネットワーク
  - ミドルウェア
  - アプリケーション

## バージョン管理

 + システム全体のバージョンとは
  - AWSリソース+ミドルウェア+アプリケーション

# ネットワーク

 1. VPC
 2. Subnet
 3. SecurityGroup
 4. RouteTable
 5. LoadBalancer
 6. Route53

# ミドルウェア

 1. RDS
 2. S3Bucket/S3Endpoint
 3. ElastiCache
 4. 等

# アプリケーション

## EC2パターン

 1. EC2
 2. LaunchConfiguration
 3. AutoScalingGroup

### UserData

## ECSパターン
