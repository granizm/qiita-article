---
title: OpenClaw(Clawdbot AI)入門｜簡単3stepで10分で構築
tags:
  - AWS
  - CloudFormation
  - AI
  - bedrock
  - OpenClaw
private: true
updated_at: '2026-02-08T08:56:48+09:00'
id: 86356680ec37702f491d
organization_url_name: null
slide: false
ignorePublish: false
---

## この記事で解決できること

高価なMac miniを購入せずに、クラウド環境でOpenClaw（旧Clawdbot AI）を運用したい方向けのガイドです。

## クラウド運用のメリット

- Mac mini不要
- どこからでもアクセス可能
- 24時間365日稼働
- セキュリティ面での分離

## 構築手順（3ステップ）

### Step 1: AWSへのデプロイ（約5分）

テンプレートを使用してCloudFormationスタックを作成します。約1〜3分でリソース作成が完了します。

### Step 2: 設定

1. EC2インスタンスに接続
2. コマンド `openclaw_setup` を実行
3. LLMモデル選択（OpenRouterを使用例として紹介）
4. `gpt-oss-120b:free` を選択

### Step 3: 使用開始

取得したURLにアクセスしてOpenClawと対話開始。

## コスト目安

| 利用パターン | コスト |
|-------------|--------|
| 1日（8時間） | 約10円（$0.07） |
| 1ヶ月 | 約320円（$2） |

:::note warn
この記事は検証向けの内容です。実務運用にはセキュリティ対策が別途必要です。LLM利用料金は別途発生します。
:::

## 参考リンク

- [元記事（note）](https://note.com/granizm/n/n83515660ed41)
