# sync-docs-to-wiki

GitHub Wiki への自動同期を行うためのサンプルリポジトリです。

このリポジトリは、`docs/wiki/` ディレクトリ配下の Markdown ファイルを GitHub Wiki に自動的に同期する GitHub Actions ワークフローのサンプルとして提供されています。

## 概要

`docs/wiki/` ディレクトリ内の Markdown ファイルを GitHub Wiki に自動同期します。以下の機能を提供します：

- 📄 `docs/wiki/` 配下の Markdown ファイルを Wiki ページとして自動作成・更新
- 🗑️ `docs/wiki/` から削除されたファイルに対応する Wiki ページを自動削除
- 📋 Wiki のサイドバー（`_Sidebar`）を自動生成
- 🔒 保護された Wiki ページ（`[private]` で始まるページなど）は削除対象外

## 構成

```
.
├── .github/
│   └── workflows/
│       └── sync-wiki.yml          # GitHub Actions ワークフロー定義
├── docs/
│   └── wiki/                       # Wiki に同期する Markdown ファイル
│       ├── Home.md
│       ├── 01_要件定義書/
│       │   ├── 開発要件.md
│       │   └── 顧客要件.md
│       └── 02_権限設計書/
│           ├── 一般ユーザー.md
│           └── 管理者ユーザー.md
└── scripts/
    └── sync-docs-to-wiki.js        # Wiki 同期スクリプト
```

## 使い方

### 1. リポジトリのセットアップ

1. `sync-wiki.yml`と`sync-docs-to-wiki.js`を任意のリポジトリに移植します。
2. GitHub リポジトリで Wiki 機能を有効化します（Settings → Features → Wiki を有効化）

### 2. ワークフローの動作

以下の条件で自動的に Wiki 同期が実行されます：

- `main` または `develop` ブランチにプッシュされたとき
- `docs/wiki/**/*.md` ファイルが変更されたとき
- GitHub Actions の手動実行（`workflow_dispatch`）

### 3. Wiki ページの追加・更新

`docs/wiki/` ディレクトリに Markdown ファイルを追加または更新すると、自動的に GitHub Wiki に反映されます。

ファイルパスは Wiki ページタイトルに変換されます：
- `docs/wiki/01_要件定義書/開発要件.md` → Wiki ページタイトル: `01_要件定義書-開発要件`

### 4. Wiki ページの削除

`docs/wiki/` からファイルを削除すると、対応する Wiki ページも自動的に削除されます。

ただし、以下のページは削除されません：
- `[private]` で始まるページ
- `PROTECTED_WIKI_PAGES` に指定されたページ

## カスタマイズ

### 同期対象ディレクトリの変更

`scripts/sync-docs-to-wiki.js` の `DOCS_DIR` を変更することで、同期対象のディレクトリを変更できます：

```javascript
const DOCS_DIR = path.join(process.cwd(), 'docs', 'wiki');
```

### 保護ページの設定

`scripts/sync-docs-to-wiki.js` の `PROTECTED_WIKI_PAGES` に保護したいページタイトルを追加できます：

```javascript
const PROTECTED_WIKI_PAGES = [
  '保護したいページ名',
];
```

### ワークフローのトリガー条件の変更

`.github/workflows/sync-wiki.yml` の `on` セクションを編集することで、実行条件を変更できます。

## 注意事項

- このスクリプトは GitHub Actions 環境でのみ実行可能です（ローカル実行は不可）
- Wiki 機能が有効になっている必要があります
- `GITHUB_TOKEN` は自動的に提供されるため、追加の設定は不要です

## ライセンス

このサンプルコードは自由に使用・改変できます。
