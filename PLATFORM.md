# Platform Configuration - Qiita

## Basic Settings

- **Platform**: Qiita
- **Language**: ja (日本語)
- **Primary Audience**: 日本人エンジニア・開発者

## Writing Guidelines

- 技術的に正確で実践的な内容を重視
- コードサンプルは動作確認済みのものを使用
- 初心者にも分かりやすい解説を心がける
- 実務で即座に活用できるTipsを含める

## Tone & Style

- 丁寧語（です・ます調）を使用
- 専門用語は適宜解説を付ける
- 絵文字は控えめに使用
- 読みやすさを重視した構成

## Content Structure

- 導入：問題提起や背景説明
- 本文：段階的な解説とコード例
- まとめ：要点の整理と次のステップ

## Tags Strategy

- 日本語タグを優先
- 技術スタック名（React, Docker等）は英語のまま
- 最大5つまでのタグを選定

## Target Topics

- Web開発（フロントエンド/バックエンド）
- インフラ・クラウド（AWS, GCP, Azure）
- DevOps・CI/CD
- プログラミング言語のTips
- 開発環境・ツール

## Platform-Specific Notes

- Qiitaは技術記事特化のプラットフォーム
- LGTMによる評価システム
- 組織アカウントとの連携も可能
- Markdown記法をフル活用

## Frontmatter Requirements（重要）

Qiita CLI v0.5.0以降、以下のフィールドが**必須**です。新規記事作成時は必ずこれらを含めてください。

```yaml
---
title: "記事タイトル"
tags:
  - タグ1
  - タグ2
private: true           # true=限定公開, false=公開
updated_at: ''          # 空文字列（新規時）
id: null                # null（新規時、投稿後にIDが付与される）
organization_url_name: null  # null または 組織URL名
slide: false            # スライドモード
ignorePublish: false    # true=公開しない
---
```

### テンプレートファイル

新規記事は `public/_template.md` をコピーして作成してください。
必須フィールドがすべて含まれています。

### よくあるエラー

以下のエラーが出た場合は、frontmatterに必須フィールドが不足しています：

```
updated_atは文字列で入力してください
idは文字列で入力してください
slideの設定はtrue/falseで入力してください
```

### privateフィールドの意味

- `private: true` → 限定公開（URLを知っている人のみ閲覧可能）
- `private: false` → 公開（誰でも閲覧可能）
