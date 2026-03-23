# MkDocs

MkDocs の主要コマンドを網羅的にまとめたリファレンスです。

---

## 基本コマンド

### バージョン確認

MkDocs がインストールされているか・バージョンを確認する。

```bash
mkdocs --version
```

#### 出力例

```text
mkdocs, version 1.6.0 from /usr/lib/python3/dist-packages/mkdocs (Python 3.12)
```

---

### ヘルプの表示

使えるコマンドの一覧とオプションを表示する。

```bash
mkdocs --help
```

#### サブコマンドのヘルプを見る場合

```bash
mkdocs serve --help
mkdocs build --help
mkdocs new --help
mkdocs gh-deploy --help
```

---

### 新規プロジェクトの作成

`my-project` という名前のプロジェクトを新規作成する。

```bash
mkdocs new my-project
```

#### 作成されるディレクトリ構成

```text
my-project/
├── docs/
│   └── index.md
└── mkdocs.yml
```

#### カレントディレクトリに作成したい場合

```bash
mkdocs new .
```

---

## よく使うコマンド

### ローカル開発サーバーの起動

ブラウザで `http://127.0.0.1:8000` を開くとプレビューできる。ファイルを保存すると自動でリロードされる。

```bash
mkdocs serve
```

#### ポートを変更したい場合

```bash
mkdocs serve --dev-addr 127.0.0.1:8080
```

#### ホストを 0.0.0.0 にして LAN 内の他デバイスからアクセスする場合

```bash
mkdocs serve --dev-addr 0.0.0.0:8000
```

---

### サイトのビルド

`site/` ディレクトリに静的 HTML を生成する。

```bash
mkdocs build
```

#### site/ を一度クリーンしてからビルドする場合

```bash
mkdocs build --clean
```

#### ビルド先ディレクトリを変更する場合

```bash
mkdocs build --site-dir public
```

---

### GitHub Pages へのデプロイ

`gh-pages` ブランチにビルド済みサイトをプッシュする。

```bash
mkdocs gh-deploy
```

#### コミットメッセージを指定してデプロイ

```bash
mkdocs gh-deploy --message "docs: update documentation"
```

#### 強制プッシュ（リモートの gh-pages を上書き）する場合

```bash
mkdocs gh-deploy --force
```

---

## 上級者向けコマンド

### 設定ファイルを指定してビルド

デフォルト（`mkdocs.yml`）以外の設定ファイルを使ってビルドする。

```bash
mkdocs build --config-file mkdocs.prod.yml
```

#### サーバー起動時に別の設定ファイルを使う場合

```bash
mkdocs serve --config-file mkdocs.dev.yml
```

---

### ビルド時の厳格チェック（警告をエラーとして扱う）

壊れたリンクや警告があるとビルドを失敗扱いにする。CI/CD での品質チェックに有効。

```bash
mkdocs build --strict
```

#### サーバー起動でも使える

```bash
mkdocs serve --strict
```

---

### デプロイ先ブランチを指定する

`gh-pages` 以外のブランチにデプロイしたい場合。

```bash
mkdocs gh-deploy --remote-branch main
```

---

### デプロイ先リモートを指定する

`origin` 以外のリモートにデプロイしたい場合。

```bash
mkdocs gh-deploy --remote-name upstream
```

---

### ライブリロードを無効にして起動

ファイル変更の監視を無効にする。重いプロジェクトで動作が遅い場合に有効。

```bash
mkdocs serve --no-livereload
```

---

### ウォッチ対象ディレクトリを追加する

`docs/` 以外のディレクトリ（例：カスタムテーマや共有ファイル）も変更監視する。

```bash
mkdocs serve --watch overrides/ --watch theme/
```

---

### サイトを事前に削除してからデプロイ

デプロイ前に既存の `gh-pages` の内容を削除してクリーンな状態にする。

```bash
mkdocs gh-deploy --clean
```

---

## 知ってると便利なコマンド

### ドライランでビルド内容を確認（エラー検出のみ）

実際にファイルを出力せず、設定・リンクのエラーチェックだけ行う。

```bash
mkdocs build --no-directory-urls
```

---

### ビルド出力を静かにする（ログ抑制）

`--quiet` を付けると警告・情報ログを抑制する。スクリプト組み込み時に便利。

```bash
mkdocs build --quiet
```

---

### 詳細ログを出力する

```bash
mkdocs build --verbose
```

#### サーバー起動にも使える

```bash
mkdocs serve --verbose
```

---

### サイトディレクトリを指定してビルド

出力先を `dist/` など任意のフォルダに変更する。

```bash
mkdocs build --site-dir dist
```

---

### ドキュメントディレクトリを指定してビルド

`docs/` 以外のフォルダをソースにする場合（通常は `mkdocs.yml` の `docs_dir` で設定するが、CLI からも一部指定可能）。

```bash
mkdocs build --docs-dir src/docs
```

---

### デプロイ時にカスタムドメインを設定する

`CNAME` ファイルを自動生成して GitHub Pages のカスタムドメインを設定する。

```bash
mkdocs gh-deploy --cname docs.example.com
```

---

### デプロイ前にビルドをスキップ（既存の site/ をそのまま使う）

```bash
mkdocs gh-deploy --no-build
```

---

## オプション一覧

### `mkdocs serve` オプション

| オプション | 短縮形 | 説明 |
| --- | --- | --- |
| `--dev-addr HOST:PORT` | `-a` | サーバーのアドレスとポートを指定（デフォルト: `127.0.0.1:8000`） |
| `--no-livereload` | | ライブリロードを無効にする |
| `--watch PATH` | `-w` | 監視対象ディレクトリを追加する |
| `--config-file FILE` | `-f` | 設定ファイルを指定する（デフォルト: `mkdocs.yml`） |
| `--strict` | `-s` | 警告をエラーとして扱う |
| `--verbose` | `-v` | 詳細ログを出力する |
| `--quiet` | `-q` | 警告・情報ログを抑制する |
| `--open` | | ブラウザを自動で開く |
| `--theme THEME` | `-t` | テーマを指定する（例: `material`, `mkdocs`, `readthedocs`） |
| `--use-directory-urls` | | ディレクトリ URL を有効にする（デフォルト: 有効） |
| `--no-directory-urls` | | ディレクトリ URL を無効にする |

---

### `mkdocs build` オプション

| オプション | 短縮形 | 説明 |
| --- | --- | --- |
| `--clean` | `-c` | ビルド前に `site/` を削除する |
| `--config-file FILE` | `-f` | 設定ファイルを指定する |
| `--site-dir DIR` | `-d` | 出力先ディレクトリを指定する（デフォルト: `site`） |
| `--docs-dir DIR` | | ドキュメントソースディレクトリを指定する |
| `--strict` | `-s` | 警告をエラーとして扱う |
| `--verbose` | `-v` | 詳細ログを出力する |
| `--quiet` | `-q` | 警告・情報ログを抑制する |
| `--no-directory-urls` | | ディレクトリ URL を無効にする |
| `--theme THEME` | `-t` | テーマを指定する |

---

### `mkdocs gh-deploy` オプション

| オプション | 短縮形 | 説明 |
| --- | --- | --- |
| `--clean` | `-c` | デプロイ前にリモートブランチをクリアする |
| `--message MESSAGE` | `-m` | コミットメッセージを指定する |
| `--remote-branch BRANCH` | `-b` | デプロイ先ブランチを指定する（デフォルト: `gh-pages`） |
| `--remote-name REMOTE` | `-r` | デプロイ先リモートを指定する（デフォルト: `origin`） |
| `--force` | `-f` | 強制プッシュする |
| `--no-history` | | コミット履歴なしで単一コミットとして上書きする |
| `--no-build` | | ビルドをスキップし、既存の `site/` をそのままデプロイする |
| `--config-file FILE` | | 設定ファイルを指定する |
| `--strict` | `-s` | 警告をエラーとして扱う |
| `--verbose` | `-v` | 詳細ログを出力する |
| `--quiet` | `-q` | 警告・情報ログを抑制する |
| `--cname CNAME` | | カスタムドメインを設定する（CNAME ファイルを生成） |

---

## 設定

### 最小構成

```yaml
site_name: My Docs
```

---

### 基本構成

```yaml
site_name: My Documentation
site_url: https://example.com
site_description: プロジェクトのドキュメント
site_author: Your Name
docs_dir: docs
site_dir: site

theme:
  name: material

nav:
  - ホーム: index.md
  - ガイド:
    - はじめに: guide/getting-started.md
    - インストール: guide/installation.md
  - API リファレンス: api/reference.md
```

---

### テーマの指定

```yaml
# 標準テーマ（mkdocs / readthedocs / material）
theme:
  name: material
```

#### 利用可能な標準テーマ一覧

```bash
# インストール済みテーマを確認
mkdocs serve --theme mkdocs
mkdocs serve --theme readthedocs
mkdocs serve --theme material   # pip install mkdocs-material が必要
```

---

### Material テーマの詳細設定

```yaml
theme:
  name: material
  language: ja
  palette:
    scheme: slate          # ダークモード（default でライト）
    primary: indigo
    accent: cyan
  font:
    text: Roboto
    code: Roboto Mono
  features:
    - navigation.tabs        # タブナビゲーション
    - navigation.top         # トップに戻るボタン
    - navigation.expand      # サイドバーを展開
    - navigation.sections    # セクション表示
    - navigation.indexes     # セクションのインデックスページ
    - toc.integrate          # 目次をサイドバーに統合
    - search.suggest         # 検索候補
    - search.highlight       # 検索ハイライト
    - content.code.copy      # コードブロックのコピーボタン
    - content.tabs.link      # タブリンク
```

---

### プラグインの設定

```yaml
plugins:
  - search:
      lang: ja              # 日本語検索
  - tags                    # タグ機能（Material テーマ）
  - minify:                 # HTML 圧縮（pip install mkdocs-minify-plugin）
      minify_html: true
```

---

### Markdown 拡張の設定

```yaml
markdown_extensions:
  - admonition              # 注意・警告ブロック（!!! note など）
  - pymdownx.details        # 折りたたみブロック（??? note など）
  - pymdownx.superfences    # コードブロック拡張（Mermaid など）
  - pymdownx.highlight:     # シンタックスハイライト
      anchor_linenums: true
      linenums: true
  - pymdownx.inlinehilite   # インラインコードハイライト
  - pymdownx.tabbed:        # コンテンツタブ
      alternate_style: true
  - pymdownx.emoji:         # 絵文字サポート
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
  - attr_list               # HTML 属性の追加
  - md_in_html              # HTML 内に Markdown を記述
  - toc:                    # 目次の設定
      permalink: true
      title: 目次
  - tables                  # テーブル
  - footnotes               # 脚注
  - abbr                    # 略語
  - def_list                # 定義リスト
  - meta                    # メタデータ
```

---

### extra / カスタム設定

```yaml
extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/yourname
  analytics:
    provider: google
    property: G-XXXXXXXXXX
  version:
    provider: mike               # バージョン管理プラグイン

extra_css:
  - stylesheets/extra.css        # カスタム CSS

extra_javascript:
  - javascripts/extra.js         # カスタム JavaScript
```

---

### 複数バージョン管理（mike プラグイン）

```bash
# mike のインストール
pip install mike

# バージョン 1.0 をデプロイ
mike deploy 1.0

# デフォルトバージョンの設定
mike set-default 1.0

# バージョン一覧の確認
mike list

# ローカルプレビュー
mike serve
```

---

## 頻出コマンド早見表

```bash
# 新規プロジェクト作成
mkdocs new my-project

# ローカルサーバー起動
mkdocs serve

# ポート指定でサーバー起動
mkdocs serve --dev-addr 127.0.0.1:8080

# ビルド
mkdocs build

# クリーンビルド
mkdocs build --clean

# GitHub Pages へデプロイ
mkdocs gh-deploy

# クリーン＋コミットメッセージ付きデプロイ
mkdocs gh-deploy --clean --message "docs: update"

# 厳格チェック付きビルド（CI 向け）
mkdocs build --strict --clean

# 詳細ログでサーバー起動
mkdocs serve --verbose

# バージョン確認
mkdocs --version
```

---

最終更新: 2026-03-23
