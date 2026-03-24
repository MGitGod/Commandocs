# Bun

Bun は JavaScript / TypeScript のオールインワンツールキットです。  
パッケージマネージャー・バンドラー・テストランナー・ランタイムをすべて含んでいます。

---

## インストール・アップデート・アンインストール

### インストール

=== "macOS / Linux"

    ```bash
    curl -fsSL https://bun.sh/install | bash
    ```

=== "npm 経由"

    ```bash
    npm install -g bun
    ```

=== "Homebrew（macOS）"

    ```bash
    brew tap oven-sh/bun
    brew install bun
    ```

=== "Windows（PowerShell）"

    ```powershell
    powershell -c "irm bun.sh/install.ps1 | iex"
    ```

!!! tip "インストール確認"

    ```bash
    bun --version
    ```

---

### アップデート

```bash
bun upgrade
```

!!! note "説明"
    Bun 自体を最新バージョンに更新します。

#### 特定バージョンへの更新

```bash
bun upgrade --version 1.1.0
```

#### カナリア（開発版）への更新

```bash
bun upgrade --canary
```

---

### アンインストール

=== "macOS / Linux"

    ```bash
    rm -rf ~/.bun
    ```

    `.bashrc` / `.zshrc` から以下の行も削除してください：

    ```bash
    export BUN_INSTALL="$HOME/.bun"
    export PATH="$BUN_INSTALL/bin:$PATH"
    ```

=== "Homebrew（macOS）"

    ```bash
    brew uninstall bun
    ```

=== "npm 経由でインストールした場合"

    ```bash
    npm uninstall -g bun
    ```

---

## 基本コマンド

### ヘルプを表示する

```bash
bun --help
```

!!! note "説明"
    使用可能なすべてのコマンドとオプションの一覧を表示します。

#### サブコマンドのヘルプ

```bash
bun install --help
bun run --help
bun build --help
```

---

### バージョンを確認する

```bash
bun --version
```

---

### プロジェクトを初期化する

```bash
bun init
```

!!! note "説明"
    対話形式で `package.json` と `tsconfig.json` を生成します。

#### 非対話モードで即時初期化

```bash
bun init -y
```

---

### スクリプトを実行する

```bash
bun run <スクリプト名>
```

#### 使用例

```bash
# package.json の "dev" スクリプトを実行
bun run dev

# package.json の "build" スクリプトを実行
bun run build

# package.json の "test" スクリプトを実行
bun run test
```

!!! tip "省略形"
    `bun run` は多くの場合 `bun` だけでも動作します。

    ```bash
    bun dev
    bun build
    ```

---

### ファイルを直接実行する

```bash
bun <ファイル名>
```

#### 使用例

```bash
# TypeScript ファイルをそのまま実行
bun index.ts

# JavaScript ファイルを実行
bun app.js

# REPL（対話モード）を起動
bun repl
```

---

## パッケージ管理コマンド

### パッケージをインストールする

```bash
bun install
```

!!! note "説明"
    `package.json` に記載されたすべての依存関係をインストールします。`bun.lockb` を利用して高速にインストールします。

#### 使用例

```bash
# 依存関係をすべてインストール
bun install

# 本番依存のみインストール（devDependencies を除外）
bun install --production

# lockfile を無視して再インストール
bun install --no-cache

# インストール後の postinstall スクリプトを無効化
bun install --ignore-scripts
```

---

### パッケージを追加する

```bash
bun add <パッケージ名>
```

!!! note "説明"
    新しいパッケージを `dependencies` に追加します。

#### 使用例

```bash
# 通常の依存関係として追加
bun add express

# 複数パッケージを一度に追加
bun add react react-dom

# 特定バージョンを指定して追加
bun add lodash@4.17.21

# 開発依存として追加（devDependencies）
bun add --dev typescript

# 省略形
bun add -d eslint

# ピア依存として追加（peerDependencies）
bun add --peer react

# オプション依存として追加（optionalDependencies）
bun add --optional fsevents

# グローバルにインストール
bun add --global typescript

# 省略形
bun add -g bun-types
```

---

### パッケージを削除する

```bash
bun remove <パッケージ名>
```

#### 使用例

```bash
# パッケージを削除
bun remove lodash

# 複数パッケージを一度に削除
bun remove eslint prettier

# グローバルパッケージを削除
bun remove --global typescript
```

---

### パッケージを更新する

```bash
bun update
```

!!! note "説明"
    `package.json` のバージョン範囲に従ってパッケージを更新します。

#### 使用例

```bash
# すべてのパッケージを更新
bun update

# 特定のパッケージだけ更新
bun update react

# 最新バージョンに強制更新（バージョン範囲を無視）
bun update --latest
```

---

### インストール済みパッケージ一覧を確認する

```bash
bun pm ls
```

#### 使用例

```bash
# インストール済みパッケージを表示
bun pm ls

# グローバルパッケージを表示
bun pm ls --global
```

---

### lockfile を確認・管理する

```bash
# lockfile の内容をテキスト形式で表示
bun pm hash

# lockfile を再生成
bun install --frozen-lockfile
```

---

## テストコマンド

### テストを実行する

```bash
bun test
```

!!! note "説明"
    `*.test.ts` / `*.spec.ts` などのテストファイルを自動検出して実行します。

#### 使用例

```bash
# すべてのテストを実行
bun test

# 特定のファイルのみテスト
bun test src/utils.test.ts

# ファイルパターンを指定してテスト
bun test --testPathPattern="src/.*"

# ウォッチモードでテスト（ファイル変更時に自動再実行）
bun test --watch

# カバレッジを取得
bun test --coverage

# タイムアウトを設定（ミリ秒）
bun test --timeout 10000

# 特定のテスト名を絞り込み
bun test --test-name-pattern "ログイン"

# 並列実行数を指定
bun test --rerun-each 3
```

---

## ビルドコマンド

### ビルドを実行する

```bash
bun build <エントリーポイント>
```

!!! note "説明"
    ファイルをバンドル・ビルドしてブラウザや Node.js 向けに最適化します。

#### 使用例

```bash
# index.ts をビルドして ./dist に出力
bun build ./index.ts --outdir ./dist

# ブラウザ向けにビルド
bun build ./src/index.ts --outdir ./dist --target browser

# Node.js 向けにビルド
bun build ./src/index.ts --outdir ./dist --target node

# Bun ランタイム向けにビルド
bun build ./src/index.ts --outdir ./dist --target bun

# ミニファイ（圧縮）して出力
bun build ./index.ts --outdir ./dist --minify

# ソースマップを生成
bun build ./index.ts --outdir ./dist --sourcemap=external

# 単一ファイルにバンドル
bun build ./index.ts --outfile bundle.js

# 外部モジュールを除外
bun build ./index.ts --outdir ./dist --external react --external react-dom

# コード分割を有効化
bun build ./index.ts --outdir ./dist --splitting
```

---

## 実行・スクリプト関連コマンド

### パッケージのバイナリを実行する（bunx）

```bash
bunx <コマンド>
```

!!! note "説明"
    `npx` の Bun 版です。パッケージをインストールせずに一時的に実行します。

#### 使用例

```bash
# create-react-app を一時実行
bunx create-react-app my-app

# prettier を一時実行
bunx prettier --write .

# eslint を一時実行
bunx eslint src/

# 特定バージョンを指定して実行
bunx typescript@5.0.0 --version
```

---

### シェルスクリプトを実行する（Bun Shell）

```bash
bun -e "<シェルスクリプト>"
```

#### 使用例

```bash
# ファイル一覧を表示
bun -e "const {$} = require('bun'); await $`ls -la`"
```

---

## 上級者向けコマンド

### 実行可能ファイルを作成する（コンパイル）

```bash
bun build --compile <エントリーポイント>
```

!!! note "説明"
    アプリケーションを単一の実行可能バイナリにコンパイルします。Node.js や Bun のインストールなしで実行可能になります。

#### 使用例

```bash
# ./index.ts を単一バイナリにコンパイル
bun build --compile ./index.ts --outfile myapp

# Windows 向けにクロスコンパイル
bun build --compile ./index.ts --target bun-windows-x64 --outfile myapp.exe

# macOS (Apple Silicon) 向け
bun build --compile ./index.ts --target bun-darwin-arm64 --outfile myapp

# Linux (x64) 向け
bun build --compile ./index.ts --target bun-linux-x64 --outfile myapp

# ミニファイしてコンパイル
bun build --compile --minify ./index.ts --outfile myapp
```

---

### ワークスペースを管理する

```bash
# ワークスペース内のすべてのパッケージにコマンドを実行
bun run --filter "*" <スクリプト名>
```

#### 使用例

```bash
# すべてのワークスペースで build を実行
bun run --filter "*" build

# 特定のワークスペースのみ実行
bun run --filter "packages/utils" test

# ワークスペースにパッケージを追加
bun add lodash --filter "packages/api"
```

---

### キャッシュを管理する

```bash
# キャッシュの場所を確認
bun pm cache

# キャッシュを削除
bun pm cache rm
```

---

### 依存関係ツリーを確認する

```bash
bun pm ls --all
```

---

### パッケージのパスを確認する

```bash
bun pm bin
```

#### 使用例

```bash
# ローカルの .bin ディレクトリを表示
bun pm bin

# グローバルの bin ディレクトリを表示
bun pm bin --global
```

---

### trust（信頼済みスクリプト）を管理する

```bash
# 信頼済みパッケージを一覧表示
bun pm trust --all

# 特定パッケージを信頼済みに追加
bun pm trust <パッケージ名>
```

---

## 知ってると便利なコマンド

### ホットリロードで実行する

```bash
bun --hot <ファイル名>
```

!!! note "説明"
    ファイルの変更を検知してプロセスを再起動せずにモジュールをリロードします。開発中に便利です。

#### 使用例

```bash
bun --hot server.ts
```

---

### ウォッチモードで実行する

```bash
bun --watch <ファイル名>
```

!!! note "説明"
    ファイルの変更を検知してプロセスを再起動します。`--hot` より確実な再起動が必要なときに使います。

#### 使用例

```bash
bun --watch index.ts
```

---

### 環境変数ファイルを指定して実行する

```bash
bun run --env-file=<ファイル名> <スクリプト>
```

#### 使用例

```bash
# .env.local を読み込んで dev を実行
bun run --env-file=.env.local dev

# 複数の env ファイルを読み込む
bun run --env-file=.env --env-file=.env.local start
```

---

### TypeScript の型チェックを行う

```bash
bun tsc
```

!!! note "説明"
    TypeScript コンパイラを呼び出して型エラーをチェックします（実行はしません）。

---

### パッケージ情報を確認する

```bash
bun pm pack
```

!!! note "説明"
    パッケージを `.tgz` ファイルとしてパックします（公開前の確認に便利）。

---

### シェバン付きスクリプトを作成する

```ts title="myscript.ts"
#!/usr/bin/env bun
// Bun で直接実行できるスクリプト
console.log("Hello from Bun!");
```

#### 実行権限を付与して直接実行

```bash
chmod +x myscript.ts
./myscript.ts
```

---

### REPL（対話モード）を使う

```bash
bun repl
```

#### 使用例

```bash
$ bun repl
> 1 + 1
2
> const greet = (name: string) => `Hello, ${name}!`
> greet("World")
"Hello, World!"
```

---

## よく使うオプション

### グローバルオプション

| オプション | 省略形 | 説明 |
| --- | --- | --- |
| `--help` | `-h` | ヘルプを表示 |
| `--version` | `-v` | バージョンを表示 |
| `--cwd <ディレクトリ>` | - | 作業ディレクトリを変更 |
| `--env-file <ファイル>` | - | 環境変数ファイルを指定 |
| `--silent` | - | ログ出力を抑制 |
| `--verbose` | - | 詳細なログを出力 |
| `--no-install` | - | 自動インストールを無効化 |
| `--hot` | - | ホットリロードを有効化 |
| `--watch` | - | ウォッチモードを有効化 |
| `--smol` | - | メモリ使用量を削減するモード |
| `--inspect` | - | デバッガを有効化 |
| `--inspect-brk` | - | 最初の行で一時停止してデバッグ |

---

### `bun install` のオプション一覧

| オプション | 説明 |
| --- | --- |
| `--production` | devDependencies を除外してインストール |
| `--frozen-lockfile` | lockfile を変更せずにインストール（CI 向け） |
| `--no-cache` | キャッシュを無視してインストール |
| `--ignore-scripts` | postinstall スクリプトを実行しない |
| `--dry-run` | 実際にはインストールせず確認だけ |
| `--backend` | インストールバックエンドを指定（`hardlink`, `symlink`, `copyfile`） |
| `--concurrent-scripts <数>` | 並列実行するスクリプト数を指定 |
| `--global` / `-g` | グローバルにインストール |

---

### `bun build` のオプション一覧

| オプション | 説明 |
| --- | --- |
| `--outdir <ディレクトリ>` | 出力先ディレクトリを指定 |
| `--outfile <ファイル>` | 出力ファイル名を指定 |
| `--target <ターゲット>` | 実行環境（`browser`, `node`, `bun`）を指定 |
| `--minify` | すべての最小化を有効化 |
| `--minify-whitespace` | 空白のみ最小化 |
| `--minify-identifiers` | 識別子のみ最小化 |
| `--minify-syntax` | 構文のみ最小化 |
| `--sourcemap` | ソースマップの種類（`none`, `inline`, `external`） |
| `--splitting` | コード分割を有効化 |
| `--external <パッケージ>` | 外部モジュールとして扱う |
| `--compile` | 単一実行バイナリを生成 |
| `--define <key=value>` | グローバル定数を定義 |
| `--loader <拡張子:ローダー>` | カスタムローダーを指定 |
| `--format <形式>` | 出力形式（`esm`, `cjs`, `iife`） |
| `--banner <テキスト>` | 出力ファイルの先頭にテキストを追加 |
| `--footer <テキスト>` | 出力ファイルの末尾にテキストを追加 |

---

### `bun test` のオプション一覧

| オプション | 説明 |
| --- | --- |
| `--watch` | ウォッチモードでテスト |
| `--coverage` | コードカバレッジを取得 |
| `--bail <数>` | 指定数の失敗で停止 |
| `--timeout <ミリ秒>` | テストのタイムアウトを設定 |
| `--test-name-pattern <正規表現>` | テスト名で絞り込み |
| `--rerun-each <数>` | 各テストを指定回数繰り返す |
| `--only` | `.only` のついたテストだけ実行 |
| `--todo` | `.todo` のついたテストも実行 |
| `--preload <ファイル>` | テスト前に指定ファイルを読み込む |

---

## 設定（bunfig.toml）

`bunfig.toml` はプロジェクトルートに配置する Bun の設定ファイルです。

ファイルの場所： `./bunfig.toml`（またはホームディレクトリの `~/.bunfig.toml`）

### 基本設定例

```toml title="bunfig.toml"
# デフォルトのテストタイムアウト（ミリ秒）
[test]
timeout = 10000
coverage = true
coverageReporter = ["text", "lcov"]
coverageDir = "coverage"
preload = ["./test-setup.ts"]

# パッケージインストールの設定
[install]
# デフォルトのレジストリを変更
registry = "https://registry.npmjs.org"
# キャッシュの場所を変更
cache = "~/.bun/install/cache"
# グローバルインストール先
globalDir = "~/.bun/install/global"
# 自動インストールを有効化
auto = "local"

# スコープ別レジストリの設定（プライベートレジストリなど）
[install.scoped]
"@mycompany" = { url = "https://npm.mycompany.com", token = "YOUR_TOKEN" }

# lockfile の設定
[install.lockfile]
save = true
print = "bun"

# ランタイム設定
[run]
bun-flags = ["--smol"]
```

---

### レジストリを変更する

```toml title="bunfig.toml"
# npmjs.org 以外を使う場合
[install]
registry = "https://registry.npmjs.org"

# GitHub Packages を使う場合
[install.scoped]
"@myorg" = { url = "https://npm.pkg.github.com", token = "$GITHUB_TOKEN" }
```

---

### テスト設定例

```toml title="bunfig.toml"
[test]
# タイムアウト（ミリ秒）
timeout = 5000

# カバレッジを常に取得
coverage = true

# カバレッジレポートの形式
coverageReporter = ["text", "html", "lcov"]

# カバレッジの出力先
coverageDir = "./coverage"

# テスト前に実行するセットアップファイル
preload = ["./tests/setup.ts"]

# rootDir を指定
root = "./src"
```

---

## 頻出コマンド早引き

```bash
# プロジェクト初期化
bun init -y

# 依存関係インストール
bun install

# 開発サーバー起動
bun run dev

# ビルド
bun run build

# テスト実行
bun test

# テスト（ウォッチ）
bun test --watch

# パッケージ追加
bun add <パッケージ名>

# 開発依存パッケージ追加
bun add -d <パッケージ名>

# パッケージ削除
bun remove <パッケージ名>

# パッケージ更新
bun update

# ファイルをホットリロードで実行
bun --hot index.ts

# 単一バイナリにコンパイル
bun build --compile ./index.ts --outfile myapp

# キャッシュ削除
bun pm cache rm

# Bun 自体を更新
bun upgrade
```

---

!!! info "公式ドキュメント"
    詳細は [Bun 公式ドキュメント](https://bun.sh/docs) を参照してください。
