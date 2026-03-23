# Gemini CLI

Googleが提供するオープンソースのAIエージェント「Gemini CLI」のコマンドリファレンスです。
ターミナル上でGeminiの能力を直接活用できます。

- **無料枠**: 個人Googleアカウントで 60リクエスト/分、1,000リクエスト/日
- **コンテキストウィンドウ**: 100万トークン（Gemini 2.5 Pro）
- **公式リポジトリ**: [github.com/google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli)

---

## インストール・アップデート・アンインストール

### インストール

#### 方法①：npx（インストール不要で即実行）

```bash
npx @google/gemini-cli
```

#### 方法②：npm でグローバルインストール（推奨）

```bash
npm install -g @google/gemini-cli
```

#### 方法③：conda 経由（Node.js 環境を conda で管理する場合）

```bash
conda create -y -n gemini_env -c conda-forge nodejs
conda activate gemini_env
npm install -g @google/gemini-cli
```

初回起動後、Googleアカウントでの認証またはAPIキーの設定が必要です。

```bash
# APIキーを環境変数に設定する場合
export GEMINI_API_KEY="YOUR_API_KEY"
```

### アップデート

```bash
npm install -g @google/gemini-cli@latest
```

または、CLI内から：

```bash
gemini update
```

### アンインストール

```bash
npm uninstall -g @google/gemini-cli
```

設定ファイルも削除する場合：

```bash
# Linux / macOS
rm -rf ~/.gemini

# Windows（PowerShell）
Remove-Item -Recurse -Force "$env:USERPROFILE\.gemini"
```

---

## 基本コマンド

### 起動・実行

| コマンド | 説明 | 使用例 |
| --- | --- | --- |
| `gemini` | インタラクティブREPLを起動 | `gemini` |
| `gemini "クエリ"` | クエリを実行してインタラクティブモードへ | `gemini "このプロジェクトを説明して"` |
| `gemini -p "クエリ"` | 非インタラクティブにクエリを実行 | `gemini -p "README.md を要約して"` |
| `gemini -i "クエリ"` | クエリを実行後、インタラクティブモードへ継続 | `gemini -i "バグを探して"` |
| `gemini --version` | バージョン情報を表示 | `gemini --version` |
| `gemini --help` | ヘルプを表示 | `gemini --help` |

#### パイプ処理（標準入力との組み合わせ）

```bash
# ファイル内容をGeminiに渡す
cat logs.txt | gemini

# 複数ファイルをパイプ
cat error.log | gemini -p "このエラーの原因を教えて"
```

### セッション管理

| コマンド | 説明 | 使用例 |
| --- | --- | --- |
| `gemini -r "latest"` | 直近のセッションを再開 | `gemini -r "latest"` |
| `gemini -r "latest" "クエリ"` | セッション再開＋新しいプロンプト | `gemini -r "latest" "型エラーを確認して"` |
| `gemini -r "<session-id>" "クエリ"` | IDを指定してセッション再開 | `gemini -r "abc123" "このPRを完成させて"` |
| `gemini --list-sessions` | 利用可能なセッション一覧を表示して終了 | `gemini --list-sessions` |
| `gemini --delete-session <index>` | セッションをインデックス番号で削除 | `gemini --delete-session 3` |

---

## よく使うコマンド

### スラッシュコマンド（対話モード内）

インタラクティブREPL内では `/` 始まりのコマンドを使用します。

#### ヘルプ・情報系

```text
/help
```

> 利用可能なすべてのコマンドと説明を表示します。

```text
/?
```

> `/help` の短縮形。

```text
/about
```

> CLIのバージョン情報を表示します。バグ報告時に便利です。

```text
/stats
```

> 現在のセッションのトークン使用量、キャッシュ節約量、セッション時間などを表示します。

---

#### 会話・セッション制御

```text
/clear
```

> ターミナル画面をクリアします。ショートカット：`Ctrl+L`

```text
/compress
```

> チャット履歴全体をサマリーに圧縮します。トークン節約に有効です。

```text
/quit
```

> CLIを終了します。`/exit` でも同様です。

---

#### チャット保存・再開

```text
/chat save <タグ名>
```

> 現在の会話履歴をタグを付けて保存します。

```bash
# 例：作業中の会話を保存
/chat save feature-login
```

```text
/chat resume <タグ名>
```

> 保存された会話を再開します。

```bash
/chat resume feature-login
```

```text
/chat list
```

> 保存済みの会話タグ一覧を表示します。

```text
/chat delete <タグ名>
```

> 保存済みの会話を削除します。

```text
/chat share <ファイル名>
```

> 現在の会話をMarkdownまたはJSONファイルに書き出します。

```bash
# Markdownに書き出し
/chat share my-session.md

# JSONに書き出し
/chat share my-session.json
```

---

#### ファイル・コンテキスト参照（@コマンド）

`@` を使ってファイルやディレクトリの内容をプロンプトに取り込めます。

```text
@<ファイルパス>
```

```bash
# ファイルの内容を参照してプロンプトを送る
@src/main.py このコードを説明して

# ディレクトリ全体を参照
@src/ このプロジェクトの構造を要約して

# プロンプトの後にファイルを指定することも可
このREADMEについて質問があります @README.md
```

> **注意**：スペースを含むパスはバックスラッシュでエスケープしてください（例：`@My\ Documents/file.txt`）

---

#### シェルコマンドの実行（!コマンド）

```text
!<シェルコマンド>
```

```bash
# lsコマンドを実行してGemini CLIに戻る
!ls -la

# Gitの状態確認
!git status

# シェルモードのトグル（!単体で入力）
!
```

---

#### メモリ・コンテキスト管理

```text
/memory show
```

> GEMINI.md から読み込まれたメモリ（指示コンテキスト）を全表示します。

```text
/memory add <テキスト>
```

> AIのメモリにテキストを追加します。

```bash
/memory add このプロジェクトではTypeScriptを使用しています
```

```text
/memory refresh
```

> GEMINI.md ファイルからメモリを再読み込みします。

```text
/memory list
```

> 読み込まれているGEMINI.mdファイルのパス一覧を表示します。

---

#### コピー・クリップボード

```text
/copy
```

> Gemini CLIの直近の出力をクリップボードにコピーします。
> LinuxはXclipまたはxsel、macOSはpbcopy、Windowsはclipが必要です。

---

#### 設定・テーマ

```text
/settings
```

> 設定エディタを開き、CLIの動作や見た目を変更できます。

```text
/theme
```

> ビジュアルテーマを変更するダイアログを開きます。

```text
/auth
```

> 認証方法を変更するダイアログを開きます。

---

## 上級者向けコマンド

### ヘッドレスモード（スクリプト・自動化）

```bash
# 非インタラクティブにクエリを実行
gemini -p "このコードベースのアーキテクチャを説明して"

# JSON形式で出力（スクリプト処理に便利）
gemini -p "Explain the architecture" --output-format json

# ストリーミングJSON形式で出力
gemini -p "Analyze this code" --output-format stream-json

# テキスト形式（デフォルト）
gemini -p "Write a summary" --output-format text
```

### モデル指定

```bash
# 高度な推論タスクにProモデルを使用
gemini -m pro "複雑なアルゴリズムを実装して"

# 高速・バランス型モデル
gemini -m flash "このコードをリファクタリングして"

# 最速の軽量モデル
gemini -m flash-lite "短いコメントを追加して"

# 具体的なモデル名で指定
gemini -m gemini-2.5-pro "詳細な分析をして"
```

| エイリアス | 解決先 | 説明 |
| --- | --- | --- |
| `auto` | `gemini-2.5-pro`（デフォルト） | 標準的なProモデル |
| `pro` | `gemini-2.5-pro` | 複雑な推論タスク向け |
| `flash` | `gemini-2.5-flash` | 高速・バランス型 |
| `flash-lite` | `gemini-2.5-flash-lite` | 最速・軽量タスク向け |

### サンドボックスモード

```bash
# 安全な実行環境でCLIを起動
gemini --sandbox
gemini -s
```

### 承認モード設定

```bash
# デフォルト（ツール実行前に確認を求める）
gemini --approval-mode default

# ファイル編集を自動承認
gemini --approval-mode auto_edit

# すべての操作を自動承認（注意して使用）
gemini --approval-mode yolo
```

### ワークスペース・マルチディレクトリ

```bash
# 複数ディレクトリを含めてCLIを起動
gemini --include-directories ./src,./docs

# セッション中にディレクトリを追加
/directory add ./backend

# 追加済みディレクトリを確認
/directory show
```

### チェックポイント・ファイル復元

```bash
# チェックポイントを有効にして起動
gemini --checkpointing
```

```text
/restore
```

> ツール実行直前の状態にプロジェクトファイルを復元します。
> ツールコールIDなしで実行すると、利用可能なチェックポイント一覧を表示します。

```bash
# 特定のツールコールIDで復元
/restore <tool_call_id>
```

### 巻き戻し（Rewind）

```text
/rewind
```

> 会話履歴を遡り、過去のインタラクションを確認・ファイル変更の取り消しができます。
> ショートカット：`Esc` を素早く2回押す

### Vimモード

```text
/vim
```

> Vimスタイルの入力モードをオン/オフします。

| Vimモード | 主な操作 |
| --- | --- |
| NORMAL | `h/j/k/l` で移動、`w/b/e` で単語移動、`dd` で行削除 |
| INSERT | 通常のテキスト入力、`Esc` でNORMALモードへ |
| カウント | `3h`、`5w` など数字でコマンドを繰り返し |
| 繰り返し | `.` で直前の編集操作を繰り返す |

### プランモード

```text
/plan
```

> 読み取り専用のプランモードに切り替えます。実装前に計画だけを確認できます。

### MCPサーバー管理

```bash
# stdioベースのMCPサーバーを追加
gemini mcp add github npx -y @modelcontextprotocol/server-github

# HTTPベースのMCPサーバーを追加
gemini mcp add api-server http://localhost:3000 --transport http

# 環境変数付きでMCPサーバーを追加
gemini mcp add slack node server.js --env SLACK_TOKEN=xoxb-xxx

# ユーザースコープで追加
gemini mcp add db node db-server.js --scope user

# 特定のツールのみを含める
gemini mcp add github npx -y @modelcontextprotocol/server-github --include-tools list_repos,get_pr

# MCPサーバーを削除
gemini mcp remove github

# 設定済みMCPサーバー一覧を表示
gemini mcp list
```

セッション中：

```text
/mcp
```

> MCPサーバーの状態と利用可能なツールを表示します。

```text
/mcp desc
```

> MCPサーバーとツールの詳細な説明を表示します。

```text
/mcp schema
```

> ツールの設定パラメータのJSONスキーマを表示します。

### エクステンション管理

```bash
# GitリポジトリからExtensionをインストール
gemini extensions install https://github.com/user/my-extension

# 特定のブランチ/タグ/コミットを指定してインストール
gemini extensions install https://github.com/user/my-extension --ref develop

# 自動アップデートを有効にしてインストール
gemini extensions install https://github.com/user/my-extension --auto-update

# Extensionをアンインストール
gemini extensions uninstall my-extension

# インストール済みExtension一覧
gemini extensions list

# 特定のExtensionをアップデート
gemini extensions update my-extension

# 全Extensionをアップデート
gemini extensions update --all

# Extensionを有効化
gemini extensions enable my-extension

# Extensionを無効化
gemini extensions disable my-extension

# ローカルパスからリンク（開発用）
gemini extensions link /path/to/extension

# 新しいExtensionをテンプレートから作成
gemini extensions new ./my-extension

# Extensionの構造を検証
gemini extensions validate ./my-extension
```

### エージェントスキル管理

```bash
# 利用可能なスキル一覧
gemini skills list

# GitリポジトリからInstall
gemini skills install https://github.com/user/repo

# ローカルパスからリンク
gemini skills link /path/to/my-skills

# スキルをアンインストール
gemini skills uninstall my-skill

# スキルを有効化
gemini skills enable my-skill
gemini skills enable --all

# スキルを無効化
gemini skills disable my-skill
gemini skills disable --all
```

### IDE連携

```text
/ide install
```

> VS Code連携をセットアップします。

```text
/ide enable
```

> VS Codeとの接続を有効にします（カーソル位置・選択テキストなどのコンテキストが使用可能になります）。

### Gitワークツリー

```bash
# 新しいgit worktreeでGeminiを起動
gemini -w my-feature

# 名前自動生成で起動
gemini -w
```

> `settings.json` に `"experimental.worktrees": true` が必要です。

---

## 知ってると便利なコマンド

### GEMINI.md の初期化

```text
/init
```

> カレントディレクトリを解析し、プロジェクト専用の `GEMINI.md` を生成します。
> AIへの永続的な指示（コーディングスタイル、プロジェクト情報など）を記述するファイルです。

#### GEMINI.md の読み込み順序（階層型メモリ）

```text
~/.gemini/GEMINI.md                 # グローバル（全プロジェクト共通）
<プロジェクトルート>/GEMINI.md       # プロジェクト固有
<サブディレクトリ>/GEMINI.md         # コンポーネント固有
```

### カスタムコマンド（スラッシュコマンドの自作）

`~/.gemini/commands/`（グローバル）または `.gemini/commands/`（プロジェクト）にTOMLファイルを作成します。

```bash
# ファイルを作成（例：/reviewコマンド）
vim ~/.gemini/commands/review.toml
```

TOMLファイルの基本形式：

```toml
description = "コードレビューを実行します"
prompt = """
あなたは経験豊富なコードレビュアーです。
以下のコードを詳しくレビューしてください：{{args}}
"""
```

- ファイル名がコマンド名になります（`review.toml` → `/review`）
- サブディレクトリでネームスペース化できます（`git/commit.toml` → `/git:commit`）

```bash
# カスタムコマンドを使用
/review src/main.py
```

### ツール一覧の確認

```text
/tools
```

> 現在利用可能なツールの一覧を表示します。

```text
/tools desc
```

> 各ツールの詳細な説明を表示します。

### プライバシー設定

```text
/privacy
```

> プライバシーポリシーを表示し、データ収集への同意設定を変更できます。

### バグ報告

```text
/bug <バグの概要>
```

> GitHubリポジトリにIssueとして報告します。

```bash
/bug CLIが特定のファイル読み込み時にクラッシュする
```

### セッション中のリロード

```text
/memory reload
/mcp reload
/skills reload
/agents reload
/commands reload
/extensions reload
```

> 各コンポーネントをディスクから再読み込みします。セッションを再起動せずに設定変更を反映できます。

### 承認モードのトグル

キーボードショートカット `Shift+Tab`（入力中）で `default` → `auto_edit` → `plan` の順で承認モードを循環します。

キーボードショートカット `Ctrl+Y` でYOLOモード（全自動承認）のオン/オフを切り替えます。

### スクリーンリーダーモード

```bash
gemini --screen-reader
```

> アクセシビリティのためのスクリーンリーダーモードを有効にします。

---

## オプション一覧

`gemini` コマンドに使用できるフラグの全一覧です。

| オプション | 短縮形 | 型 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| `--debug` | `-d` | boolean | `false` | 詳細ログ付きのデバッグモードで実行 |
| `--version` | `-v` | - | - | CLIのバージョン番号を表示して終了 |
| `--help` | `-h` | - | - | ヘルプ情報を表示 |
| `--model` | `-m` | string | `auto` | 使用するモデルを指定（エイリアスまたはモデル名） |
| `--prompt` | `-p` | string | - | プロンプトテキスト。非インタラクティブモードを強制 |
| `--prompt-interactive` | `-i` | string | - | プロンプトを実行後、インタラクティブモードへ継続 |
| `--sandbox` | `-s` | boolean | `false` | サンドボックス環境で実行 |
| `--approval-mode` | - | string | `default` | ツール実行の承認モード：`default` / `auto_edit` / `yolo` |
| `--yolo` | `-y` | boolean | `false` | **非推奨** すべての操作を自動承認（`--approval-mode=yolo` を使用） |
| `--resume` | `-r` | string | - | 前のセッションを再開。`"latest"` または インデックス番号 |
| `--list-sessions` | - | boolean | - | 利用可能なセッション一覧を表示して終了 |
| `--delete-session` | - | string | - | インデックス番号でセッションを削除 |
| `--include-directories` | - | array | - | ワークスペースに含める追加ディレクトリ（カンマ区切り） |
| `--worktree` | `-w` | string | - | 新しいgit worktreeでGeminiを起動 |
| `--output-format` | `-o` | string | `text` | 出力形式：`text` / `json` / `stream-json` |
| `--allowed-mcp-server-names` | - | array | - | 許可するMCPサーバー名（カンマ区切り） |
| `--extensions` | `-e` | array | - | 使用するExtensionのリスト（カンマ区切り） |
| `--list-extensions` | `-l` | boolean | - | 利用可能なExtension一覧を表示して終了 |
| `--screen-reader` | - | boolean | - | スクリーンリーダーモードを有効化 |
| `--checkpointing` | - | boolean | - | チェックポイント機能を有効化 |
| `--experimental-acp` | - | boolean | - | ACPモードで起動（実験的機能） |

---

## 設定

### 設定ファイルの場所

```text
~/.gemini/settings.json               # グローバル設定（全プロジェクト共通）
<プロジェクト>/.gemini/settings.json  # プロジェクト固有の設定
```

### 設定ファイルの編集

```bash
# GUIで設定を編集（CLIの /settings コマンド）
/settings

# 直接ファイルを編集する場合
vim ~/.gemini/settings.json
```

### 主要な設定項目

```json
{
  "theme": "dark",
  "selectedAuthType": "oauth-personal",
  "model": "gemini-2.5-pro",
  "vim": false,
  "approval_mode": "default",
  "checkpointing": true,
  "context": {
    "fileFiltering": {
      "respectGitIgnore": true
    }
  },
  "advanced": {
    "bugCommand": "https://github.com/google-gemini/gemini-cli/issues/new"
  },
  "experimental": {
    "plan": true,
    "worktrees": false
  },
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"]
    },
    "myPythonServer": {
      "command": "python",
      "args": ["-m", "my_mcp_server", "--port", "8080"],
      "cwd": "./mcp_tools/python",
      "env": {
        "DATABASE_URL": "$DB_URL_FROM_ENV"
      },
      "timeout": 15000,
      "trust": false,
      "includeTools": ["safe_tool_1"],
      "excludeTools": ["dangerous_tool"]
    }
  }
}
```

### 環境変数

| 変数名 | 説明 |
| --- | --- |
| `GEMINI_API_KEY` | Gemini APIキー（Google AI Studio から取得） |
| `GOOGLE_API_KEY` | Google Cloud APIキー（Vertex AI使用時） |
| `GOOGLE_GENAI_USE_VERTEXAI` | `true` を設定するとVertex AI経由で利用 |
| `GEMINI_CLI=1` | `!` コマンド実行時にサブプロセスへ自動設定される |

### 認証方法の設定

```bash
# 個人GoogleアカウントでOAuth認証（デフォルト）
/auth

# APIキーを環境変数で設定
export GEMINI_API_KEY="YOUR_API_KEY"

# Vertex AI（Google Cloud）を使用
export GOOGLE_API_KEY="YOUR_CLOUD_KEY"
export GOOGLE_GENAI_USE_VERTEXAI=true
```

### .geminiignore ファイル

`.gitignore` と同様の書式で、Gemini CLIが参照しないファイル/ディレクトリを指定します。

```bash
# プロジェクトルートに作成
vim .geminiignore
```

```gitignore
# .geminiignore の例
node_modules/
dist/
.env
*.secret
*.key
build/
```

### テーマ設定

```text
/theme
```

> ダイアログからビジュアルテーマを選択します。設定は `~/.gemini/settings.json` に保存されます。

---

## キーボードショートカット一覧

### 基本操作

| 操作 | キー |
| --- | --- |
| 現在の選択を確定 | `Enter` |
| ダイアログを閉じる / フォーカスをキャンセル | `Esc` / `Ctrl+[` |
| リクエストのキャンセル / 終了（入力が空の場合） | `Ctrl+C` |
| 終了（入力バッファが空の場合） | `Ctrl+D` |

### カーソル移動

| 操作 | キー |
| --- | --- |
| 行頭へ移動 | `Ctrl+A` / `Home` |
| 行末へ移動 | `Ctrl+E` / `End` |
| 1単語左へ移動 | `Ctrl+Left` / `Alt+B` |
| 1単語右へ移動 | `Ctrl+Right` / `Alt+F` |

### テキスト編集

| 操作 | キー |
| --- | --- |
| カーソルから行末まで削除 | `Ctrl+K` |
| カーソルから行頭まで削除 | `Ctrl+U` |
| 前の単語を削除 | `Ctrl+W` / `Ctrl+Backspace` |
| 次の単語を削除 | `Alt+D` / `Ctrl+Delete` |
| 元に戻す | `Cmd/Win+Z` / `Alt+Z` |
| やり直し | `Ctrl+Shift+Z` |

### テキスト入力

| 操作 | キー |
| --- | --- |
| プロンプトを送信 | `Enter` |
| 改行を挿入（送信しない） | `Ctrl+Enter` / `Shift+Enter` / `Alt+Enter` |
| 外部エディタでプロンプトを編集 | `Ctrl+X` |
| クリップボードから貼り付け | `Ctrl+V` / `Cmd/Win+V` |

### 履歴・検索

| 操作 | キー |
| --- | --- |
| 前の履歴を表示 | `Ctrl+P` / `上矢印` |
| 次の履歴を表示 | `Ctrl+N` / `下矢印` |
| 履歴の逆順検索 | `Ctrl+R` |
| 前のインタラクションに巻き戻し | `Esc` 2回 |

### スクロール

| 操作 | キー |
| --- | --- |
| 上にスクロール | `Shift+Up` |
| 下にスクロール | `Shift+Down` |
| 最上部へ | `Ctrl+Home` / `Shift+Home` |
| 最下部へ | `Ctrl+End` / `Shift+End` |
| ページアップ | `Page Up` |
| ページダウン | `Page Down` |

### アプリ操作

| 操作 | キー |
| --- | --- |
| 画面のクリア＆再描画 | `Ctrl+L` |
| MCPツール説明のトグル | `Ctrl+T` |
| Markdownレンダリングのトグル | `Alt+M` |
| YOLOモードのトグル | `Ctrl+Y` |
| 承認モードの循環（default → auto_edit → plan） | `Shift+Tab`（入力中） |
| バックグラウンドシェルの表示切り替え | `Ctrl+B` |
| 詳細エラー情報のトグル | `F12` |
| IDEコンテキストの表示 | `Ctrl+G` |
| アプリの再起動 | `R` |
| CLIをバックグラウンドに移行 | `Ctrl+Z` |

---

## よく使う頻出コマンド集

### 起動・基本操作

```bash
gemini
```

```bash
gemini -p "README.md を要約して"
```

```bash
cat エラーログ.txt | gemini -p "このエラーの原因と解決策を教えて"
```

```bash
gemini -r "latest"
```

### ファイル参照

```text
@README.md このプロジェクトについて教えて
```

```text
@src/ このコードを全体的にレビューして
```

### セッション保存・復元

```text
/chat save my-work
```

```text
/chat resume my-work
```

### メモリ管理

```text
/memory show
```

```text
/memory add TypeScript 5.0以降の構文を優先すること
```

### シェル実行

```text
!git log --oneline -10
```

```text
!npm test
```

### モデル選択付き起動

```bash
gemini -m flash "素早くコードを修正して"
```

```bash
gemini -m pro "このシステムの設計を詳しく考察して"
```

---

> **参考リンク**
>
> - 公式ドキュメント：<https://geminicli.com/docs/>
> - コマンドリファレンス：<https://google-gemini.github.io/gemini-cli/docs/cli/commands.html>
> - GitHub：<https://github.com/google-gemini/gemini-cli>
> - Google AI Studio（APIキー取得）：<https://aistudio.google.com/>
