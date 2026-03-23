# Node.js / npm

Node.js および npm（Node Package Manager）の主要コマンドをまとめたリファレンスです。

---

## インストール・アップデート・アンインストール

### Node.js のインストール（nvm 経由・推奨）

nvm（Node Version Manager）を使うとバージョン管理が容易になります。

#### nvm のインストール

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

#### Node.js の最新 LTS をインストール

```bash
nvm install --lts
```

#### Node.js の特定バージョンをインストール（nvm）

```bash
nvm install 20.11.0
```

#### インストール済みバージョン一覧

```bash
nvm ls
```

#### 使用するバージョンを切り替え

```bash
nvm use 20
```

#### デフォルトバージョンを設定

```bash
nvm alias default 20
```

---

### Node.js のインストール（公式バイナリ・Ubuntu/Debian）

NodeSource リポジトリを追加して Node.js 20.x をインストールします。

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

---

### Node.js のアップデート

#### nvm 経由でアップデート

```bash
nvm install node --reinstall-packages-from=node
```

#### n（バージョン管理ツール）経由でアップデート

```bash
sudo npm install -g n
sudo n stable
```

---

### Node.js のアンインストール

#### nvm 経由でアンインストール

```bash
nvm uninstall 20.11.0
```

#### apt 経由でアンインストール（Ubuntu）

```bash
sudo apt-get remove nodejs
sudo apt-get purge nodejs
```

---

### npm のアップデート

npm 単体を最新版にアップグレードします。

```bash
npm install -g npm@latest
```

#### 特定バージョンに固定

```bash
npm install -g npm@10.2.4
```

---

### バージョン確認

```bash
node -v
npm -v
```

---

## 基本コマンド

### プロジェクトの初期化

`package.json` を対話形式で作成します。

```bash
npm init
```

すべての質問をスキップしてデフォルト値で作成する場合：

```bash
npm init -y
```

---

### パッケージのインストール

#### `package.json` の依存関係をすべてインストール

```bash
npm install
```

短縮形：

```bash
npm i
```

#### 特定パッケージをインストール（依存関係として追加）

```bash
npm install express
```

#### 開発依存関係として追加（devDependencies）

```bash
npm install --save-dev eslint
```

短縮形：

```bash
npm i -D eslint
```

#### グローバルにインストール

```bash
npm install -g typescript
```

#### パッケージの特定バージョンをインストール

```bash
npm install react@18.2.0
```

#### バージョン範囲を指定してインストール

```bash
npm install lodash@">=4.0.0 <5.0.0"
```

---

### パッケージのアンインストール

```bash
npm uninstall express
```

#### devDependencies から削除

```bash
npm uninstall --save-dev eslint
```

#### グローバルパッケージを削除

```bash
npm uninstall -g typescript
```

---

### パッケージのアップデート

#### すべての依存パッケージを更新

```bash
npm update
```

#### 特定パッケージを更新

```bash
npm update lodash
```

#### グローバルパッケージを更新

```bash
npm update -g
```

---

### スクリプトの実行

`package.json` の `scripts` セクションに定義したコマンドを実行します。

```bash
npm run start
npm run build
npm run test
npm run dev
```

`start` / `test` は `run` を省略できます。

```bash
npm start
npm test
```

---

## よく使うコマンド

### インストール済みパッケージの一覧

#### ローカルパッケージ（直接依存のみ）

```bash
npm list --depth=0
```

#### グローバルパッケージ一覧

```bash
npm list -g --depth=0
```

#### 全ツリーを表示

```bash
npm list
```

---

### パッケージ情報の確認

#### npm レジストリ上のパッケージ情報を表示

```bash
npm info express
```

#### 最新バージョンのみ確認

```bash
npm info express version
```

#### 利用可能なバージョン一覧

```bash
npm info express versions
```

---

### 古いパッケージの確認

```bash
npm outdated
```

出力例：

```text
Package  Current  Wanted  Latest  Location
lodash   4.17.15  4.17.21 4.17.21 myproject
```

---

### キャッシュの管理

#### キャッシュをクリア

```bash
npm cache clean --force
```

#### キャッシュの検証

```bash
npm cache verify
```

---

### パッケージの検索

```bash
npm search axios
```

---

### パッケージのドキュメント・リポジトリを開く

#### ブラウザでドキュメントを開く

```bash
npm docs express
```

#### リポジトリを開く

```bash
npm repo express
```

#### バグトラッカーを開く

```bash
npm bugs express
```

---

### ログイン・公開（npm レジストリ）

#### npmjs.com にログイン

```bash
npm login
```

#### パッケージを公開

```bash
npm publish
```

#### スコープ付きパッケージを公開（パブリック）

```bash
npm publish --access public
```

#### 公開バージョンを削除（72時間以内）

```bash
npm unpublish mypackage@1.0.0
```

---

### `package-lock.json` を使ったクリーンインストール

CI 環境や本番環境で `package-lock.json` を厳密に再現します。

```bash
npm ci
```

---

## 上級者向けコマンド

### ワークスペース（monorepo）

#### `package.json` の設定例

```json
{
  "name": "my-monorepo",
  "workspaces": [
    "packages/*"
  ]
}
```

#### 特定ワークスペースでコマンドを実行

```bash
npm run build --workspace=packages/ui
```

#### すべてのワークスペースでコマンドを実行

```bash
npm run test --workspaces
```

#### すべてのワークスペースにパッケージをインストール

```bash
npm install lodash --workspaces
```

---

### ピア依存関係の解決（npm v7+）

npm v7 以降はピア依存関係を自動インストールします。無効化する場合：

```bash
npm install --legacy-peer-deps
```

---

### パッケージのリンク（開発中ローカルパッケージ）

#### ローカルパッケージをグローバルリンクとして登録

```bash
cd my-local-package
npm link
```

#### リンクを別プロジェクトで使用

```bash
cd my-project
npm link my-local-package
```

#### リンクを解除

```bash
npm unlink my-local-package
```

---

### スクリプトのライフサイクルフック

`pre` / `post` プレフィックスで前後処理を定義できます。

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

### `npx` でパッケージを一時実行

インストールせずにパッケージのコマンドを実行します。

```bash
npx create-react-app my-app
npx prettier --write .
npx eslint src/
```

#### 特定バージョンを指定して実行

```bash
npx create-react-app@5.0.1 my-app
```

#### キャッシュを無視して最新版を実行

```bash
npx --yes create-next-app@latest my-app
```

---

### セキュリティ監査

#### 脆弱性のスキャン

```bash
npm audit
```

#### 自動修正（安全なアップデートのみ）

```bash
npm audit fix
```

#### 破壊的変更も含めて強制修正

```bash
npm audit fix --force
```

#### JSON 形式で出力

```bash
npm audit --json
```

---

### パッケージのパック（tarball 作成）

```bash
npm pack
```

#### 特定ディレクトリを対象に

```bash
npm pack ./my-package
```

---

### `node_modules` のサイズを確認

```bash
du -sh node_modules
```

---

### 依存関係ツリーで特定パッケージを確認

```bash
npm list lodash
```

#### なぜそのパッケージがインストールされているか調べる

```bash
npm why lodash
```

---

### プロファイルの確認・更新

```bash
npm profile get
npm profile set email your@email.com
```

---

### 2要素認証（2FA）の設定

```bash
npm profile enable-2fa auth-and-writes
```

---

## 知ってると便利なコマンド

### Node.js をスクリプトなしで実行（REPL）

```bash
node
```

REPL 上での便利な操作：

```text
> .help       # ヘルプ表示
> .exit       # 終了
> .save file  # セッションをファイルに保存
> .load file  # ファイルをセッションに読み込む
```

---

### スクリプトをウォッチモードで実行（Node.js v18.11+）

```bash
node --watch app.js
```

---

### 環境変数を渡してスクリプトを実行

```bash
NODE_ENV=production npm start
```

Windows でも動作させるには `cross-env` を使います。

```bash
npx cross-env NODE_ENV=production npm start
```

---

### `package.json` のフィールドを確認

```bash
node -e "console.log(require('./package.json').version)"
```

---

### インストール済みバイナリのパスを確認

```bash
npm bin
```

#### グローバルバイナリのパス

```bash
npm bin -g
```

---

### ルートディレクトリ・グローバルディレクトリを確認

```bash
npm root
npm root -g
npm prefix
npm prefix -g
```

---

### 依存関係の重複を整理

```bash
npm dedupe
```

---

### パッケージの差分確認

```bash
npm diff --diff=lodash@4.17.15 --diff=lodash@4.17.21
```

---

### 公開時に除外されるファイルを確認（dry-run）

```bash
npm pack --dry-run
```

---

### ローカルパスからインストール

```bash
npm install ./path/to/local/package
```

#### Git リポジトリから直接インストール

```bash
npm install git+https://github.com/user/repo.git
npm install github:user/repo
npm install github:user/repo#branch-name
```

---

### タグ付きバージョンのインストール

```bash
npm install express@latest
npm install express@beta
npm install express@next
```

---

### ロックファイルのみ更新（インストールなし）

```bash
npm install --package-lock-only
```

---

### スクリプト一覧の確認

```bash
npm run
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
| `--frozen-lockfile` | ロックファイルと差異があればエラー（`npm ci` 推奨） |

---

### `npm run` のオプション

| オプション | 説明 |
| --- | --- |
| `--if-present` | スクリプトが存在しない場合もエラーにしない |
| `-- <args>` | スクリプトに引数を渡す（`--` の後に記述） |

スクリプトに引数を渡す例：

```bash
npm run build -- --watch
npm test -- --coverage
```

---

## 設定（npm config）

### 設定の確認

#### 全設定を表示

```bash
npm config list
```

#### 詳細な全設定を表示

```bash
npm config list -l
```

#### 特定のキーを確認

```bash
npm config get registry
npm config get prefix
npm config get cache
```

---

### 設定の変更

#### レジストリを変更（公式）

```bash
npm config set registry https://registry.npmjs.org/
```

#### プライベートレジストリを指定

```bash
npm config set registry https://my.private.registry.com/
```

#### スコープ付きパッケージのレジストリを指定

```bash
npm config set @myorg:registry https://my.private.registry.com/
```

---

### キャッシュディレクトリの変更

```bash
npm config set cache /path/to/custom/cache
```

---

### プロキシの設定

```bash
npm config set proxy http://proxy.example.com:8080
npm config set https-proxy http://proxy.example.com:8080
```

#### プロキシを解除

```bash
npm config delete proxy
npm config delete https-proxy
```

---

### グローバルインストール先の変更（権限エラー対策）

```bash
mkdir ~/.npm-global
npm config set prefix ~/.npm-global
```

`.bashrc` または `.zshrc` に追記します。

```bash
export PATH=~/.npm-global/bin:$PATH
```

---

### SSL 証明書の設定

#### カスタム CA 証明書を指定

```bash
npm config set cafile /path/to/ca.crt
```

#### SSL 検証を無効化（非推奨・開発環境のみ）

```bash
npm config set strict-ssl false
```

---

### `.npmrc` ファイルによる設定

プロジェクト直下の `.npmrc` に書くとプロジェクト固有の設定ができます。

```text
registry=https://registry.npmjs.org/
@myorg:registry=https://my.private.registry.com/
save-exact=true
engine-strict=true
```

ユーザーホームの `.npmrc` を直接編集する場合：

```bash
vim ~/.npmrc
```

---

### 設定の削除

```bash
npm config delete registry
```

---

### デフォルトの author 情報を設定

`npm init` 時に自動入力されます。

```bash
npm config set init-author-name "Your Name"
npm config set init-author-email "your@email.com"
npm config set init-author-url "https://yoursite.com"
npm config set init-license "MIT"
```

---

### `save-exact` で固定バージョンをデフォルトにする

```bash
npm config set save-exact true
```

---

### エンジン要件を厳格にチェック

`package.json` の `engines` フィールドのバージョン要件を強制します。

```bash
npm config set engine-strict true
```

---

## 付録：よく使うコマンド早見表

| 目的 | コマンド |
| --- | --- |
| プロジェクト初期化 | `npm init -y` |
| 依存パッケージインストール | `npm install` |
| パッケージ追加 | `npm install <pkg>` |
| 開発依存パッケージ追加 | `npm install -D <pkg>` |
| グローバルインストール | `npm install -g <pkg>` |
| パッケージ削除 | `npm uninstall <pkg>` |
| 全パッケージ更新 | `npm update` |
| 古いパッケージ確認 | `npm outdated` |
| スクリプト実行 | `npm run <script>` |
| セキュリティ監査 | `npm audit` |
| 監査の自動修正 | `npm audit fix` |
| キャッシュクリア | `npm cache clean --force` |
| CI 用クリーンインストール | `npm ci` |
| 一時パッケージ実行 | `npx <pkg>` |
| インストール済み一覧 | `npm list --depth=0` |
| バージョン確認 | `node -v && npm -v` |
| 設定確認 | `npm config list` |
| レジストリ変更 | `npm config set registry <url>` |
| パッケージ情報 | `npm info <pkg>` |
| パッケージ検索 | `npm search <keyword>` |
