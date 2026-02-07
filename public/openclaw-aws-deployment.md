---
title: 【実践ガイド】OpenClaw(Clawdbot AI)をAWS + Amazon Bedrockでデプロイする方法
tags:
  - AWS
  - Bedrock
  - CloudFormation
  - AI
  - OpenClaw
private: true
updated_at: '2026-02-08T05:23:57+09:00'
id: 86356680ec37702f491d
organization_url_name: null
slide: false
ignorePublish: false
---

## はじめに

「AIエージェントを使ってみたいけど、高いMacを買わないといけないの？」

OpenClaw（旧Clawdbot AI）をAmazon AWSで動かすことで、専用ハードウェアなしでAIエージェントを利用できます。本記事では、**AWS公式サンプルとして提供されている「OpenClaw on AWS with Bedrock」**を使用したデプロイ方法を解説します。

:::note info
本記事は、AWS公式リポジトリ [aws-samples/sample-OpenClaw-on-AWS-with-Bedrock](https://github.com/aws-samples/sample-OpenClaw-on-AWS-with-Bedrock) の内容に基づいています。
:::

## この記事でできるようになること

- Mac mini等の専用機なしでOpenClawを動かす
- Amazon Bedrockを使ったAPIキー不要のAI環境を構築
- ワンクリックで約8分でデプロイ完了

## クラウドデプロイのメリット

| ローカル実行 | クラウドデプロイ |
|-------------|----------------|
| Mac mini等が必要 | ハードウェア不要 |
| APIキー管理が複雑 | IAMロールで認証 |
| 自宅からのみアクセス | どこからでもアクセス |
| セキュリティリスク | 分離された環境 |

## 必要なもの（前提条件）

1. **AWSアカウント**（必要な権限を持つもの）
2. **Bedrockモデルの有効化**（AWSコンソールから事前に設定）
3. **EC2キーペア**（SSH接続用、SSM使用時は不要）

## 使用するAWSサービス

| サービス | 用途 |
|---------|------|
| EC2 (t4g.medium) | OpenClaw実行環境（Graviton ARM） |
| Amazon Bedrock | AIモデルAPI |
| IAM | Bedrock認証用ロール |
| VPC Endpoints | プライベートネットワーク通信 |
| CloudTrail | API呼び出し監査 |
| SSM Session Manager | セキュアアクセス |

## 手順

### ステップ1: ワンクリックデプロイ（約8分）

1. [AWS公式リポジトリ](https://github.com/aws-samples/sample-OpenClaw-on-AWS-with-Bedrock)にアクセス
2. 「Launch Stack」ボタンをクリック
3. EC2キーペアを選択
4. 約8分待機

```bash
# CLIでデプロイする場合
aws cloudformation create-stack \
  --stack-name openclaw-bedrock \
  --template-url https://[S3-URL]/template.yaml \
  --capabilities CAPABILITY_IAM \
  --parameters \
    ParameterKey=KeyName,ParameterValue=your-keypair
```

### ステップ2: 出力値の確認

CloudFormationの出力タブから以下を取得：
- **アクセスURL**
- **認証トークン**

### ステップ3: SSM Session Managerでアクセス

```bash
# ポートフォワーディング設定
aws ssm start-session \
  --target i-xxxxxxxxx \
  --document-name AWS-StartPortForwardingSession \
  --parameters '{"portNumber":["18789"],"localPortNumber":["18789"]}'
```

ブラウザで `http://localhost:18789` にアクセスします。

## 選べるAIモデル

Amazon Bedrockで以下のモデルが利用可能：

- **Nova 2 Lite**（推奨・低コスト）
- **Nova Pro**
- **Claude Sonnet**
- **DeepSeek**
- **Llama**
- その他合計8つのモデル

## 料金の目安

:::note warn
従来の「月額300円」という情報は**誤り**でした。正確なコストは以下の通りです。
:::

### インフラストラクチャコスト（月額）

| 項目 | 費用 |
|------|------|
| EC2 (t4g.medium) | $24.19（約3,600円） |
| EBS (gp3, 30GB) | $2.40（約360円） |
| VPC Endpoints | $21.60（約3,200円） |
| データ転送 | $5-10（約750-1,500円） |
| **小計** | **$53-58（約8,000-8,700円）** |

### Bedrock使用料（モデル別）

| モデル | 入力 (100万トークン) | 出力 (100万トークン) |
|--------|---------------------|---------------------|
| Nova 2 Lite | $0.30 | $2.50 |
| Nova Pro | $0.80 | $3.20 |
| Claude Sonnet | $3.00 | $15.00 |

**月額総費用**: 軽量利用で約**$58-66（8,700-10,000円）**

## インスタンスタイプの選択肢

### Linux（推奨）

| タイプ | RAM | 月額 |
|--------|-----|------|
| t4g.small | 2GB | $12（約1,800円） |
| t4g.medium（デフォルト） | 4GB | $24（約3,600円） |
| t4g.large | 8GB | $48（約7,200円） |

### macOS（iOS/macOS開発向け）

| タイプ | チップ | RAM | 月額 |
|--------|--------|-----|------|
| mac2.metal | M1 | 16GB | $468（約70,000円） |
| mac2-m2.metal | M2 | 24GB | $632（約95,000円） |

## 対応メッセージングプラットフォーム

- **WhatsApp**（推奨）
- **Telegram**
- **Discord**
- **Slack**
- **Microsoft Teams**

## よくあるトラブルと解決方法

### スタック作成でエラーが出る

- IAM権限が不足している可能性があります
- `CAPABILITY_IAM`オプションを忘れていないか確認してください
- Bedrockモデルが有効化されているか確認してください

### SSM Session Managerで接続できない

- インスタンスにSSMエージェントがインストールされているか確認
- IAMロールにSSM権限が付与されているか確認

### Bedrockモデルが使えない

- AWSコンソール → Bedrock → モデルアクセスから使用するモデルを有効化
- リージョンがBedrockに対応しているか確認（us-east-1, us-west-2など）

## コスト削減のコツ

### 使わない時は停止する

AWSコンソールからインスタンスを停止すれば、EC2の料金はかかりません。
ただし、VPC Endpointsの料金は**インスタンス停止中も発生**する点に注意。

### 小さいインスタンスを使う

t4g.small（$12/月）でも動作可能です。RAM 2GBで軽量利用であれば問題ありません。

### Nova 2 Liteを使う

Bedrockモデルの中で最も低コストです。入力$0.30/100万トークンで利用できます。

## まとめ

OpenClawをAWS + Amazon Bedrockで動かすことで：

- ✅ 専用Macを買わなくてOK
- ✅ APIキー管理が不要（IAMロール認証）
- ✅ どこからでもアクセス可能
- ✅ エンタープライズグレードのセキュリティ

ただし、**月額$53程度（約8,000円）のコスト**が必要な点は理解しておきましょう。

ワンクリックで約8分でデプロイできるので、ぜひ試してみてください！

## 参考リンク

- [OpenClaw on AWS with Bedrock (AWS公式サンプル)](https://github.com/aws-samples/sample-OpenClaw-on-AWS-with-Bedrock)
- [Amazon Bedrock ドキュメント](https://docs.aws.amazon.com/bedrock/)
- [AWS CloudFormation 入門](https://docs.aws.amazon.com/ja_jp/cloudformation/index.html)
- [元記事（note）](https://note.com/granizm/n/n83515660ed41)
