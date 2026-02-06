# qiita-article

Qiita記事公開用リポジトリ

## 概要

このリポジトリはQiitaへの記事公開を管理します。
- **PR作成**: 記事を限定公開（private: true）で投稿
- **PRマージ**: 記事を公開（private: false）に変更

## ディレクトリ構成

```
qiita-article/
├── public/             # 記事
├── .github/workflows/  # GitHub Actions
└── package.json
```

## ワークフロー

| イベント | アクション | Qiita状態 |
|----------|-----------|----------|
| PR作成・更新 | private: true で投稿 | 限定公開 |
| PRマージ | private: false で投稿 | 公開 |

## 記事の作成

```bash
npm install
npm run new
```

## Frontmatter

```yaml
---
title: "記事タイトル"
tags:
  - タグ1
  - タグ2
private: true  # PRマージ時にfalseになる
updated_at: ""
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
```

## 必要な設定

### GitHub Secrets
- `QIITA_TOKEN`: Qiita APIトークン（必須）
- `DISCORD_WEBHOOK`: Discord通知用Webhook URL（オプション）

### トークン取得方法
1. [Qiita](https://qiita.com) にログイン
2. [アクセストークン発行ページ](https://qiita.com/settings/tokens/new) を開く
3. スコープ: `read_qiita`, `write_qiita` を選択
4. 発行されたトークンをGitHub Secretsに設定

## 関連リンク

- [granizm-blog](https://github.com/granizm/granizm-blog) - アイデア・下書き管理
- [Qiita CLI](https://github.com/increments/qiita-cli)
