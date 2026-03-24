# Node.js / npm

Node.js および npm（Node Package Manager）の主要コマンドリファレンスです。

---

## インストール・アップデート・アンインストール

### Node.js をインストールする

=== "nvm（推奨）"

    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
    nvm install --lts
    ```

=== "Ubuntu / Debian"

    ```bash
    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt-get install -y nodejs
    ```

!!! note "説明"
    nvm（Node Version Manager）を使うとバージョンの切り替えが容易になるため推奨します。
    Ubuntu/Debian 環境では NodeSource リポジトリ経由でインストールします。

#### 使用例

```bash
# 特定バージョンをインストール
nvm install 20.11.0

# インストール済みバージョン一覧
nvm ls

# 使用バージョンを切り替え
nvm use 20

# デフォルトバージョンを固定
nvm alias default 20
```

---

### Node.js をアップデートする

=== "nvm 経由"

    ```bash
    nvm install node --reinstall-packages-from=node
    ```

=== "n 経由"

    ```bash
    sudo npm install -g n
    sudo n stable
    ```

!!! note "説明"
    nvm 経由では既存のグローバルパッケージを新バージョンへ引き継げます。
    `n` はシンプルなバージョン管理ツールで `stable` / `latest` / `lts` などのエイリアスが使えます。

#### 使用例

```bash
# LTS の最新版に更新（nvm）
nvm install --lts --reinstall-packages-from=current

# 最新安定版に更新（n）
sudo n latest
```

---

### Node.js をアンインストールする

=== "nvm 経由"

    ```bash
    nvm uninstall 20.11.0
    ```

=== "apt 経由（Ubuntu）"

    ```bash
    sudo apt-get remove nodejs
    sudo apt-get purge nodejs
    ```

!!! note "説明"
    nvm 経由でインストールした場合は nvm で削除します。
    `purge` は設定ファイルも含めて完全に削除します。

#### 使用例

```bash
# インストール済みバージョンを確認してから削除
nvm ls
nvm uninstall 18.20.0
```

---

### npm をアップデートする

```bash
npm install -g npm@latest
```

!!! note "説明"
    npm 自体をグローバルに最新版へ更新します。特定バージョンに固定したい場合はバージョン番号を指定します。

#### 使用例

```bash
# 最新版にアップデート
npm install -g npm@latest

# バージョンを固定してインストール
npm install -g npm@10.2.4

# インストール後にバージョン確認
node -v && npm -v
```

---

## 基本コマンド

### プロジェクトを初期化する

```bash
npm init [オプション]
```

!!! note "説明"
    `package.json` を対話形式で作成します。`-y` を付けるとすべての質問をスキップしてデフォルト値で即時作成します。

#### 使用例

```bash
# 対話形式で初期化
npm init

# デフォルト値で即時作成（-y / --yes）
npm init -y
```

---

### パッケージをインストールする

```bash
npm install [パッケージ名]
```

!!! note "説明"
    引数なしで実行すると `package.json` の依存関係をすべてインストールします。
    パッケージ名を指定すると `dependencies` に追加します。短縮形は `npm i` です。

#### 使用例

```bash
# package.json の依存関係をすべてインストール
npm install

# 特定パッケージを追加
npm install express

# バージョンを指定してインストール
npm install react@18.2.0

# バージョン範囲を指定
npm install lodash@">=4.0.0 <5.0.0"

# タグを指定してインストール
npm install express@latest
npm install express@beta
```

---

### 開発依存としてインストールする

```bash
npm install --save-dev <パッケージ名>
```

!!! note "説明"
    `devDependencies` に追加します。テストツールやビルドツールなど本番環境では不要なパッケージに使います。短縮形は `-D` です。

#### 使用例

```bash
# ESLint を開発依存として追加
npm install --save-dev eslint

# 短縮形
npm i -D eslint prettier typescript

# バージョン固定（^ なし）で追加
npm install -D -E jest
```

---

### グローバルにインストールする

```bash
npm install -g <パッケージ名>
```

!!! note "説明"
    システム全体で使えるコマンドラインツールをインストールします。プロジェクトの依存関係には追加されません。

#### 使用例

```bash
# TypeScript コンパイラをグローバルにインストール
npm install -g typescript

# グローバルパッケージを最新版に更新
npm update -g

# グローバルパッケージを削除
npm uninstall -g typescript

# グローバルインストール済み一覧
npm list -g --depth=0
```

---

### パッケージをアンインストールする

```bash
npm uninstall <パッケージ名>
```

!!! note "説明"
    `node_modules` からパッケージを削除し、`package.json` と `package-lock.json` も更新します。短縮形は `npm un` または `npm rm` です。

#### 使用例

```bash
# 通常の依存パッケージを削除
npm uninstall express

# devDependencies から削除
npm uninstall --save-dev eslint

# グローバルパッケージを削除
npm uninstall -g typescript

# 複数パッケージを一括削除
npm uninstall lodash axios dayjs
```

---

### パッケージをアップデートする

```bash
npm update [パッケージ名]
```

!!! note "説明"
    `package.json` のバージョン範囲（`^` や `~`）を満たす最新版に更新します。引数なしですべての依存パッケージを更新します。

#### 使用例

```bash
# すべての依存パッケージを更新
npm update

# 特定パッケージのみ更新
npm update lodash

# グローバルパッケージをすべて更新
npm update -g

# 古いパッケージを事前確認してから更新
npm outdated
npm update
```

---

### スクリプトを実行する

```bash
npm run <スクリプト名>
```

!!! note "説明"
    `package.json` の `scripts` に定義したコマンドを実行します。`start` と `test` は `run` を省略できます。
    `pre` / `post` プレフィックスでライフサイクルフックも定義できます。

#### 使用例

```bash
# よく使うスクリプト
npm run dev
npm run build
npm run lint

# run を省略できるスクリプト
npm start
npm test

# スクリプトに引数を渡す（-- の後に記述）
npm run build -- --watch
npm test -- --coverage

# 定義済みスクリプト一覧を確認
npm run

# スクリプトが存在しない場合もエラーにしない
npm run deploy --if-present
```

`package.json` のライフサイクルフック設定例：

```json
{
  "scripts": {
    "prebuild": "echo ビルド前処理",
    "build": "tsc",
    "postbuild": "echo ビルド後処理"
  }
}
```

---

## よく使うコマンド

### インストール済みパッケージを一覧表示する

```bash
npm list [オプション]
```

!!! note "説明"
    インストール済みのパッケージとそのバージョンを表示します。`--depth=0` で直接依存のみ表示します。

#### 使用例

```bash
# ローカルパッケージ（直接依存のみ）
npm list --depth=0

# グローバルパッケージ一覧
npm list -g --depth=0

# 全ツリーを表示
npm list

# 特定パッケージの依存ツリーを確認
npm list lodash
```

---

### パッケージ情報を確認する

```bash
npm info <パッケージ名> [フィールド]
```

!!! note "説明"
    npm レジストリ上のパッケージ情報を取得します。フィールドを指定すると特定の情報のみ取得できます。

#### 使用例

```bash
# パッケージ全情報を表示
npm info express

# 最新バージョンのみ確認
npm info express version

# 利用可能な全バージョン一覧
npm info express versions

# 依存関係のみ確認
npm info express dependencies

# ブラウザでドキュメントを開く
npm docs express

# リポジトリを開く
npm repo express
```

---

### 古いパッケージを確認する

```bash
npm outdated
```

!!! note "説明"
    現在インストール済みのバージョン・`package.json` の許容バージョン（Wanted）・最新バージョン（Latest）を比較表示します。

#### 使用例

```bash
# ローカルパッケージの確認
npm outdated

# グローバルパッケージの確認
npm outdated -g

# JSON 形式で出力
npm outdated --json
```

出力例：

```text
Package  Current  Wanted  Latest  Location
lodash   4.17.15  4.17.21 4.17.21 myproject
```

---

### キャッシュを管理する

```bash
npm cache <サブコマンド>
```

!!! note "説明"
    インストール時にダウンロードされたパッケージのキャッシュを管理します。インストールが失敗するときはキャッシュクリアが有効です。

#### 使用例

```bash
# キャッシュをクリア
npm cache clean --force

# キャッシュの整合性を検証
npm cache verify

# キャッシュディレクトリを確認
npm config get cache
```

---

### パッケージを検索する

```bash
npm search <キーワード>
```

!!! note "説明"
    npm レジストリ上のパッケージをキーワードで検索します。

#### 使用例

```bash
npm search axios
npm search "date format"
```

---

### CI 用クリーンインストールを実行する

```bash
npm ci
```

!!! note "説明"
    `package-lock.json` を厳密に再現してインストールします。既存の `node_modules` を削除してから再インストールするため、CI 環境や本番環境に適しています。`npm install` とは異なり `package-lock.json` を更新しません。

#### 使用例

```bash
# CI 環境でのクリーンインストール
npm ci

# devDependencies を除外してインストール
npm ci --omit=dev
```

---

### パッケージを公開する

```bash
npm publish [オプション]
```

!!! note "説明"
    カレントディレクトリのパッケージを npm レジストリに公開します。事前に `npm login` が必要です。

#### 使用例

```bash
# ログイン
npm login

# パッケージを公開
npm publish

# スコープ付きパッケージをパブリックで公開
npm publish --access public

# 公開前に除外ファイルを確認（dry-run）
npm pack --dry-run

# 公開バージョンを削除（72時間以内）
npm unpublish mypackage@1.0.0
```

---

## 上級者向けコマンド

### ワークスペース（monorepo）を操作する

```bash
npm <コマンド> --workspace=<パス>
```

!!! note "説明"
    npm v7 以降のワークスペース機能で monorepo を管理します。`package.json` の `workspaces` フィールドで定義します。

`package.json` の設定例：

```json
{
  "name": "my-monorepo",
  "workspaces": [
    "packages/*"
  ]
}
```

#### 使用例

```bash
# 特定ワークスペースでスクリプトを実行
npm run build --workspace=packages/ui

# すべてのワークスペースでスクリプトを実行
npm run test --workspaces

# 特定ワークスペースにパッケージを追加
npm install lodash --workspace=packages/utils

# すべてのワークスペースに一括インストール
npm install --workspaces
```

---

### ローカルパッケージをリンクする

```bash
npm link [パッケージ名]
```

!!! note "説明"
    開発中のローカルパッケージをグローバルにシンボリックリンクし、他のプロジェクトから参照できるようにします。npm レジストリへ公開せずに動作確認できます。

#### 使用例

```bash
# 開発中パッケージをグローバルリンクとして登録
cd my-local-package
npm link

# 別プロジェクトでリンクを使用
cd ../my-project
npm link my-local-package

# リンクを解除
npm unlink my-local-package
```

---

### npx でパッケージを一時実行する

```bash
npx <パッケージ名> [引数]
```

!!! note "説明"
    ローカル・グローバルにインストールせずにパッケージのコマンドを一時実行します。`node_modules/.bin` のバイナリも実行できます。

#### 使用例

```bash
# プロジェクトを新規作成
npx create-react-app my-app
npx create-next-app@latest my-app

# フォーマット・Lint を一時実行
npx prettier --write .
npx eslint src/

# 特定バージョンを指定して実行
npx create-react-app@5.0.1 my-app

# 確認プロンプトをスキップして実行
npx --yes cowsay "hello"
```

---

### セキュリティ監査を実行する

```bash
npm audit [オプション]
```

!!! note "説明"
    インストール済みパッケージの脆弱性をスキャンします。`fix` サブコマンドで自動修正できます。

#### 使用例

```bash
# 脆弱性をスキャン
npm audit

# 安全なアップデートのみ自動修正
npm audit fix

# 破壊的変更も含めて強制修正
npm audit fix --force

# JSON 形式で出力（CI 連携に便利）
npm audit --json

# 重大度を絞って表示
npm audit --audit-level=high
```

---

### パッケージを tarball にパックする

```bash
npm pack [パス]
```

!!! note "説明"
    パッケージを `.tgz` ファイルに圧縮します。公開前の動作確認や、npm レジストリを使わない配布に使います。

#### 使用例

```bash
# カレントディレクトリをパック
npm pack

# 別パスのパッケージをパック
npm pack ./packages/my-lib

# 公開対象ファイルを確認するだけ（実ファイルは生成しない）
npm pack --dry-run
```

---

### パッケージの依存元を調べる

```bash
npm why <パッケージ名>
```

!!! note "説明"
    そのパッケージがなぜインストールされているかを、依存チェーンで表示します。

#### 使用例

```bash
# lodash がどこから要求されているか確認
npm why lodash

# 依存ツリーで確認
npm list lodash
```

---

### 依存関係の重複を整理する

```bash
npm dedupe
```

!!! note "説明"
    重複インストールされているパッケージをツリーの上位に移動して `node_modules` を最適化します。

#### 使用例

```bash
# 重複を整理
npm dedupe

# 整理後のサイズを確認
du -sh node_modules
```

---

### ピア依存関係のエラーを回避する

```bash
npm install --legacy-peer-deps
```

!!! note "説明"
    npm v7 以降は peer dependencies を自動インストールしますが、バージョン競合でエラーになることがあります。`--legacy-peer-deps` で npm v6 以前の動作に戻します。

#### 使用例

```bash
# ピア依存チェックを緩和してインストール
npm install --legacy-peer-deps

# 特定パッケージ追加時にも適用
npm install some-package --legacy-peer-deps
```

---

## 知ってると便利なコマンド

### REPL を起動する

```bash
node
```

!!! note "説明"
    Node.js の対話型シェル（REPL）を起動します。JavaScript を直接実行して動作確認できます。

#### 使用例

```bash
node
```

REPL 内の便利なコマンド：

```text
> .help       # ヘルプ表示
> .exit       # 終了（Ctrl+D でも可）
> .save file  # セッションをファイルに保存
> .load file  # ファイルをセッションに読み込む
> .clear      # コンテキストをリセット
```

---

### ウォッチモードでスクリプトを実行する

```bash
node --watch <ファイル>
```

!!! note "説明"
    Node.js v18.11 以降に標準搭載されたウォッチモードです。ファイルの変更を検知して自動再実行します。`nodemon` なしで使えます。

#### 使用例

```bash
# app.js をウォッチモードで実行
node --watch app.js

# 特定ファイルの変更のみ監視
node --watch-path=./src app.js
```

---

### 環境変数を渡してスクリプトを実行する

```bash
NODE_ENV=production npm start
```

!!! note "説明"
    スクリプト実行時に環境変数を設定します。Windows では `cross-env` を使うとクロスプラットフォームに対応できます。

#### 使用例

```bash
# 本番モードで起動
NODE_ENV=production npm start

# 複数の環境変数を渡す
PORT=8080 NODE_ENV=development npm run dev

# cross-env を使って Windows でも動作させる
npx cross-env NODE_ENV=production npm start
```

---

### package.json のフィールドを確認する

```bash
node -e "console.log(require('./package.json').<フィールド>)"
```

!!! note "説明"
    `package.json` の内容を Node.js のワンライナーで確認します。

#### 使用例

```bash
# バージョンを確認
node -e "console.log(require('./package.json').version)"

# 依存パッケージ一覧を確認
node -e "console.log(Object.keys(require('./package.json').dependencies))"

# スクリプト一覧を確認
node -e "console.log(Object.keys(require('./package.json').scripts))"
```

---

### Git リポジトリからインストールする

```bash
npm install <git-url>
```

!!! note "説明"
    npm レジストリに公開されていないパッケージを Git リポジトリから直接インストールできます。

#### 使用例

```bash
# HTTPS URL でインストール
npm install git+https://github.com/user/repo.git

# GitHub 省略記法
npm install github:user/repo

# ブランチ・タグ・コミットを指定
npm install github:user/repo#main
npm install github:user/repo#v1.2.3

# ローカルパスからインストール
npm install ./path/to/local/package
```

---

### ロックファイルのみ更新する

```bash
npm install --package-lock-only
```

!!! note "説明"
    実際のインストールは行わず `package-lock.json` だけを更新します。ロックファイルを再生成したいときに便利です。

#### 使用例

```bash
# package-lock.json だけ更新
npm install --package-lock-only

# 更新後に差分を確認
git diff package-lock.json
```

---

### パッケージの差分を確認する

```bash
npm diff --diff=<バージョンA> --diff=<バージョンB>
```

!!! note "説明"
    2 つのバージョン間のファイル差分を表示します。アップデート前の内容確認に便利です。

#### 使用例

```bash
# バージョン間の差分を確認
npm diff --diff=lodash@4.17.15 --diff=lodash@4.17.21

# 現在インストール済みバージョンとの差分
npm diff --diff=lodash@latest
```

---

### バイナリのパスを確認する

```bash
npm bin
```

!!! note "説明"
    ローカルまたはグローバルにインストールされたバイナリの格納先パスを表示します。

#### 使用例

```bash
# ローカルバイナリのパス
npm bin

# グローバルバイナリのパス
npm bin -g

# インストール先ルートを確認
npm root
npm prefix -g
```

---

## オプション一覧

### よく使うグローバルオプション

| オプション | 短縮形 | 説明 |
| --- | --- | --- |
| `--global` | `-g` | グローバルにインストール／操作 |
| `--save-dev` | `-D` | devDependencies に追加 |
| `--save-optional` | `-O` | optionalDependencies に追加 |
| `--save-exact` | `-E` | バージョンを固定（`^` なし） |
| `--no-save` | - | `package.json` を更新しない |
| `--dry-run` | - | 実際には実行せず結果を表示 |
| `--force` | `-f` | 確認なしで強制実行 |
| `--silent` | `-s` | 出力を抑制 |
| `--verbose` | - | 詳細ログを表示 |
| `--json` | - | JSON 形式で出力 |
| `--yes` | `-y` | 質問にすべて yes で回答 |
| `--legacy-peer-deps` | - | ピア依存の厳格チェックを無効化 |
| `--omit=dev` | - | devDependencies を除外してインストール |
| `--workspace` | `-w` | 対象ワークスペースを指定 |
| `--workspaces` | `-ws` | 全ワークスペースを対象 |

---

### `npm install` のオプション

| オプション | 説明 |
| --- | --- |
| `--production` | devDependencies を除外（`NODE_ENV=production` 相当） |
| `--ignore-scripts` | インストール時のスクリプトを実行しない |
| `--prefer-offline` | キャッシュを優先し、ネットワークアクセスを最小化 |
| `--prefer-online` | 常にネットワーク確認 |
| `--no-package-lock` | `package-lock.json` を生成しない |
| `--package-lock-only` | ロックファイルのみ更新（インストールなし） |

---

### `npm run` のオプション

| オプション | 説明 |
| --- | --- |
| `--if-present` | スクリプトが存在しない場合もエラーにしない |
| `-- <引数>` | スクリプトに引数を渡す（`--` の後に記述） |

---

## 設定（npm config）

### 設定を確認する

```bash
npm config list
```

!!! note "説明"
    現在の npm 設定を一覧表示します。`-l` で全デフォルト値を含む詳細一覧が表示されます。

#### 使用例

```bash
# 現在の設定を確認
npm config list

# 全設定（デフォルト含む）を表示
npm config list -l

# 特定キーの値を確認
npm config get registry
npm config get prefix
npm config get cache
```

---

### レジストリを変更する

```bash
npm config set registry <URL>
```

!!! note "説明"
    パッケージの取得・公開先レジストリを変更します。社内レジストリや Verdaccio などプライベートレジストリへの切り替えに使います。

#### 使用例

```bash
# 公式レジストリに戻す
npm config set registry https://registry.npmjs.org/

# プライベートレジストリを指定
npm config set registry https://my.private.registry.com/

# スコープ付きパッケージのみ別レジストリを使う
npm config set @myorg:registry https://my.private.registry.com/

# 設定を削除して元に戻す
npm config delete registry
```

---

### プロキシを設定する

```bash
npm config set proxy <URL>
```

!!! note "説明"
    社内ネットワーク環境などでプロキシが必要な場合に設定します。

#### 使用例

```bash
# HTTP / HTTPS プロキシを設定
npm config set proxy http://proxy.example.com:8080
npm config set https-proxy http://proxy.example.com:8080

# プロキシを解除
npm config delete proxy
npm config delete https-proxy

# SSL 検証を無効化（非推奨・開発環境のみ）
npm config set strict-ssl false
```

---

### グローバルインストール先を変更する

```bash
npm config set prefix <パス>
```

!!! note "説明"
    `sudo` なしでグローバルインストールできるよう、インストール先をホームディレクトリ配下に変更します。

#### 使用例

```bash
# ホームディレクトリにグローバルディレクトリを作成
mkdir ~/.npm-global
npm config set prefix ~/.npm-global
```

`.bashrc` または `.zshrc` に追記します：

```bash
export PATH=~/.npm-global/bin:$PATH
```

---

### .npmrc で設定する

```bash
vim ~/.npmrc
```

!!! note "説明"
    ユーザーホームの `~/.npmrc` またはプロジェクト直下の `.npmrc` に設定を書くことで、コマンドなしで設定を管理できます。プロジェクト固有設定はプロジェクト直下の `.npmrc` に記述します。

#### 使用例

プロジェクト直下の `.npmrc` 設定例：

```text
registry=https://registry.npmjs.org/
@myorg:registry=https://my.private.registry.com/
save-exact=true
engine-strict=true
```

---

### author 情報を設定する

```bash
npm config set init-author-name "<名前>"
```

!!! note "説明"
    `npm init` 実行時に自動入力される author 情報を設定します。

#### 使用例

```bash
npm config set init-author-name "Your Name"
npm config set init-author-email "your@email.com"
npm config set init-author-url "https://yoursite.com"
npm config set init-license "MIT"

# バージョンを常に固定（^ なし）でインストール
npm config set save-exact true

# engines フィールドのバージョン要件を強制
npm config set engine-strict true
```

---

## 頻出コマンド早引き

| 目的 | コマンド |
| --- | --- |
| プロジェクト初期化 | `npm init -y` |
| 依存パッケージ一括インストール | `npm install` |
| パッケージ追加 | `npm install <pkg>` |
| 開発依存パッケージ追加 | `npm install -D <pkg>` |
| グローバルインストール | `npm install -g <pkg>` |
| パッケージ削除 | `npm uninstall <pkg>` |
| 全パッケージ更新 | `npm update` |
| 古いパッケージ確認 | `npm outdated` |
| スクリプト実行 | `npm run <script>` |
| CI 用クリーンインストール | `npm ci` |
| セキュリティ監査 | `npm audit` |
| 監査の自動修正 | `npm audit fix` |
| キャッシュクリア | `npm cache clean --force` |
| 一時パッケージ実行 | `npx <pkg>` |
| インストール済み一覧 | `npm list --depth=0` |
| バージョン確認 | `node -v && npm -v` |
| 設定確認 | `npm config list` |
| レジストリ変更 | `npm config set registry <url>` |
| パッケージ情報確認 | `npm info <pkg>` |
| パッケージ検索 | `npm search <keyword>` |
| 依存元を調べる | `npm why <pkg>` |
| 差分確認 | `npm diff --diff=<pkg@ver>` |

---

!!! info "公式ドキュメント"
    - [npm Docs](https://docs.npmjs.com/)
    - [Node.js Docs](https://nodejs.org/en/docs/)
    - [nvm リポジトリ](https://github.com/nvm-sh/nvm)
