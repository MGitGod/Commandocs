# Claude Code CLI

Claude Code は Anthropic が開発したターミナル上で動作するエージェント型コーディングアシスタントです。
コードの読み取り・編集・Git 操作・コマンド実行を自然言語で指示できます。

> **情報の鮮度について**
> Claude Code は活発に開発が続いています。最新情報は [公式ドキュメント](https://docs.claude.com/en/docs/claude-code/overview) を参照してください。
> 本ページの情報は **2026年3月時点** のものです（v2.1.77 以降）。

---

## インストール・アップデート・アンインストール

### インストール

Node.js（v18以上）と npm が必要です。

```bash
npm install -g @anthropic-ai/claude-code
```

WSL（Ubuntu）環境の場合も同じコマンドを使用します。

```bash
# バージョン確認
claude --version
claude -v
```

### アップデート

```bash
claude update
```

セッション内でも更新可能です。

```bash
# セッション内で更新を確認・適用
/release-notes
```

### アンインストール

```bash
npm uninstall -g @anthropic-ai/claude-code
```

---

## 基本コマンド

Claude Code の起動と終了に関する最も基本的なコマンドです。

### 起動

```bash
# インタラクティブモードで起動
claude

# 初期プロンプトを指定して起動
claude "auth モジュールのバグを修正して"

# 直前のセッションを続きから再開
claude -c

# セッションに名前を付けて起動
claude -n "feature-payments"
```

**使い方の例：**

```bash
# プロジェクトルートに移動してから起動する（重要）
cd ~/projects/my-app
claude
```

### 終了

```bash
# セッション内で終了
/exit
/quit

# EOF シグナルで終了
# Ctrl+D
```

### 認証

```bash
# Anthropic アカウントでログイン
claude auth login

# SSO でログイン（企業ユーザー向け）
claude auth login --sso

# 認証状態を確認
claude auth status
claude auth status --text

# ログアウト
claude auth logout

# セッション内でのログイン
/login
/logout
```

**API キーを使う場合：**

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
claude
```

---

## よく使うスラッシュコマンド

セッション内で `/` から始めるコマンドです。
`/` と入力すると一覧が表示され、続けて文字を打つと絞り込めます。

### コンテキスト管理

```text
/clear
```

会話履歴をリセットして新しいセッションを開始します（エイリアス: `/reset`, `/new`）。

```text
/compact
```

会話を圧縮してトークン使用量を削減します。フォーカスする内容を引数で指定可能。

```text
/compact 認証周りのエラーハンドリングは残して
```

**使い方の目安：** コンテキスト使用量が 80% を超えたら `/compact`、タスクを切り替えるなら `/clear`。

```text
/context
```

コンテキスト使用量をカラーグリッドで可視化し、最適化のヒントを表示します。

### セッション操作

```text
/resume
```

過去のセッションを ID・名前・ピッカーで再開します（エイリアス: `/continue`）。

```text
/resume auth-refactor
```

```text
/fork
```

現在の会話をこのポイントでフォーク（分岐）します。

```text
/fork experiment-v2
```

```text
/rename
```

セッションに名前を付けます。引数なしで自動生成。

```text
/rename feature-payments
```

```text
/rewind
```

会話やコードを過去のポイントに巻き戻します（エイリアス: `/checkpoint`）。

```bash
# Esc を2回押して巻き戻しメニューを開く
# → "Rewind code only"（コードのみ戻す）
# → "Rewind conversation"（会話ごと戻す）
```

### コスト・使用量確認

```text
/cost
```

トークン使用量とコストの詳細統計を表示します。

```text
/usage
```

プランの使用量上限とレートリミットの状況を確認します。

```text
/status
```

バージョン・モデル・アカウント・接続状況を確認します（`/config` からも開けます）。

```text
/stats
```

日次の使用量・セッション履歴・連続利用日数・モデル利用傾向を可視化します。

### モデル切り替え

```text
/model
```

AIモデルを切り替えます。左右矢印キーで努力レベルも調整できます。

```text
/model sonnet
/model opus
/model claude-sonnet-4-6
```

**使い方の例：** 複雑な設計判断には Opus、通常作業には Sonnet を使うと費用対効果が良いです。

```text
/effort
```

努力レベルを設定します（low / medium / high / max / auto）。

```text
/effort high
/effort medium
```

### 情報確認・ヘルプ

```text
/help
```

利用可能なすべてのスラッシュコマンドと説明を表示します。

```text
/doctor
```

Claude Code のインストール状態を診断します。

```text
/diff
```

未コミットの変更と各ターンの差分をインタラクティブに確認します（矢印キーで操作）。

---

## よく使うプロンプト入力パターン

### ファイル参照

```text
@src/auth.py のバグを調査して
```

`@` でファイルパスの補完が起動します。ファイル名が分からなくても Claude が grep で探してくれます。

### シェルコマンドの直接実行

```text
! git log --oneline -10
```

`!` で始めると Claude の会話モードを経由せずシェルコマンドを直接実行します。トークンを節約できます。

### バックグラウンド実行

```text
& npm run dev
```

コマンドをバックグラウンドで実行しながら作業を続けられます。

```text
/tasks
/bashes
```

バックグラウンドタスクの一覧を確認します。

```text
/kill <task-id>
```

バックグラウンドタスクを終了します。

### サイドクエスチョン（処理中でも質問可能）

```text
/btw このエラーって何が原因？
```

Claude が処理中でも割り込みで質問できます。会話履歴には残りません。

---

## 上級者向けコマンド

### プランモード

```text
/plan
```

コードを書く前にまず計画を立てるモードに入ります。複雑なタスクや大規模変更に有効です。

```text
/plan 認証システムを JWT から OAuth2 に移行する
```

**使い方の例：** 実装の前に必ずプランモードで設計を固めることが公式チームも推奨しています。

### マルチエージェント・バッチ処理

```text
/batch <指示>
```

並列エージェントで大規模変更を一括処理します。5〜30 ユニットに分解して各エージェントが独立した git worktree で作業し、PR を自動作成します。

```text
/batch "src/ の全ファイルを CommonJS から ESModule に移行して"
```

```text
/simplify
```

変更済みファイルを3エージェント並列でレビュー。コードの重複・品質・効率の問題を検出して修正します。PR 前のコード品質チェックに最適。

```text
/simplify 型安全性を重点的にチェック
```

### ループ実行

```text
/loop [間隔] <プロンプト>
```

指定した間隔でプロンプトを繰り返し実行します。デプロイ監視や PR 確認に活用できます。

```text
/loop 5m "デプロイが完了したか確認して"
```

### ワークツリー（隔離環境）

```bash
# 独立した git worktree でセッションを開始
claude -w feature-auth
claude --worktree feature-auth
```

コードの変更が互いに干渉しないよう、複数タスクを並列処理するときに使います。変更がなければ自動でクリーンアップされます。

### セキュリティレビュー

```text
/security-review
```

保留中の変更をスキャンしてセキュリティの脆弱性（インジェクション・認証問題・データ露出）を分析します。

### インサイト分析

```text
/insights
```

過去1ヶ月のセッション履歴を分析し、プロジェクト傾向・操作パターン・摩擦ポイントを HTML レポートとして生成します。

### リモートコントロール

```text
/remote-control
/rc
```

セッションを claude.ai やモバイルアプリから操作可能にします。

```bash
# CLI から起動する場合
claude remote-control --name "My Project"
claude --rc "My Project"
```

### デバッグスキル

```text
/debug
/debug セッションが重くなった
```

デバッグログを読み取り、問題を分析します。

---

## 知ってると便利なコマンド

### ファスト（高速）モード

```text
/fast
/fast on
/fast off
```

Opus 4.6 の高速設定を切り替えます。通常より 2.5 倍高速ですがトークンコストが増加します。インタラクティブな素早い反復作業や Live デバッグに最適。

### Vim モード

```text
/vim
```

Vim スタイルの入力モードと通常モードを切り替えます。

### ボイス入力

```text
/voice
```

プッシュトゥトーク音声モードを起動します。スペースキーを押しながら話してください。20以上の言語に対応。

### コピー

```text
/copy
```

最後のレスポンスをクリップボードにコピーします。コードブロックが複数ある場合はインタラクティブなピッカーが起動します。

### エクスポート

```text
/export
/export output.txt
```

会話をプレーンテキストとして保存します。ファイル名を指定すると直接書き込みます。

### ターミナル設定

```text
/terminal-setup
```

Shift+Enter のキーバインドを設定します（VS Code, Alacritty, Warp 対応）。

### カラー設定

```text
/color red
/color blue
/color default
```

プロンプトバーの色を変更します（red / blue / green / yellow / purple / orange / pink / cyan）。

### コンテキスト追加

```text
/add-dir <パス>
```

現在のセッションに追加の作業ディレクトリを追加します。

```text
/add-dir ../shared-lib
```

```bash
# CLI 起動時に指定する場合
claude --add-dir ../apps ../lib
```

### スキル一覧

```text
/skills
```

利用可能なすべてのスキルを一覧表示します。

### CLAUDE.md 初期化

```text
/init
```

プロジェクトに CLAUDE.md ガイドファイルを作成します。Claude のコンテキストとして自動的に読み込まれます。

### メモリ管理

```text
/memory
```

CLAUDE.md メモリファイルの編集・自動メモリの有効化/無効化・自動メモリエントリの確認ができます。

### MCP 管理

```text
/mcp
```

MCP サーバーの接続管理と OAuth 認証を行います。

```bash
# CLI から MCP サーバーを追加
claude mcp add --transport http github https://mcp.github.com

# デバッグモードで MCP 確認
claude --debug "mcp"

# Claude Code 自体を MCP サーバーとして公開
claude mcp serve
```

### IDE 連携

```text
/ide
```

VS Code・JetBrains との連携状態を管理・表示します。

### PR コメント確認

```text
/pr-comments
/pr-comments 123
/pr-comments https://github.com/org/repo/pull/123
```

GitHub の PR コメントを表示します。現在のブランチから PR を自動検出します。

### パーミッション設定

```text
/permissions
```

ツールの使用許可を確認・更新します（エイリアス: `/allowed-tools`）。

### フック確認

```text
/hooks
```

ツールイベントに設定されているフック（自動実行スクリプト）を確認します。

---

## キーボードショートカット

### 全般操作

| ショートカット | 説明 |
| --- | --- |
| `Ctrl+C` | 現在の入力または生成をキャンセル |
| `Ctrl+D` | セッションを終了（EOF） |
| `Ctrl+G` | 外部エディタでプロンプトを開く |
| `Ctrl+L` | ターミナル画面をクリア（会話履歴は保持） |
| `Ctrl+O` | 詳細ツール出力のオン/オフ切り替え |
| `Ctrl+R` | コマンド履歴をインタラクティブに検索 |
| `Ctrl+T` | タスクリストの表示/非表示を切り替え |
| `Ctrl+V` / `Alt+V` | クリップボードから画像をペースト（Mac/Linux か Windows） |
| `Ctrl+B` | 現在のタスクをバックグラウンドに送る |
| `Ctrl+F` （2回） | すべてのバックグラウンドエージェントを終了（3秒以内に2回） |
| `↑` / `↓` | コマンド履歴を前後に移動 |
| `←` / `→` | ダイアログのタブを切り替え |
| `Esc` + `Esc` | 巻き戻しメニューを開く |
| `Option+T` / `Alt+T` | 拡張思考（Extended Thinking）をトグル |
| `Option+P` / `Alt+P` | モデルピッカーを開く（プロンプトはそのまま保持） |
| `Shift+Tab` / `Alt+M` | パーミッションモードを切り替え（Auto-Accept / Plan / Normal） |

### テキスト編集

| ショートカット | 説明 |
| --- | --- |
| `Ctrl+K` | カーソル位置から行末まで削除（削除テキストを保存） |
| `Ctrl+U` | 行全体を削除（削除テキストを保存） |
| `Ctrl+Y` | 直前に削除したテキストをペースト |
| `Alt+Y` （Ctrl+Y の後） | ペースト履歴を循環 |
| `Alt+B` | 1ワード分カーソルを左に移動 |
| `Alt+F` | 1ワード分カーソルを右に移動 |

### 複数行入力

| 方法 | 操作 |
| --- | --- |
| バックスラッシュ + Enter | `\` （スペースなし）+ Enter — すべてのターミナルで動作 |
| macOS 標準 | Option+Enter |
| Shift+Enter | iTerm2, WezTerm, Ghostty, Kitty では標準動作。他は `/terminal-setup` を実行 |
| Ctrl+J | ラインフィード（複数行入力） |

---

## オプション（CLI フラグ）

`claude` コマンドの起動時に指定するフラグです。

### セッション管理

| フラグ | 説明 | 例 |
| --- | --- | --- |
| `-c`, `--continue` | 直前のセッションを続ける | `claude -c` |
| `-r`, `--resume` | セッションを ID または名前で再開 | `claude -r "auth-refactor" "PR を仕上げて"` |
| `-n`, `--name` | セッションの表示名を設定 | `claude -n "feature-work"` |
| `-w`, `--worktree` | 独立した git worktree で起動 | `claude -w feature-auth` |
| `--session-id` | 特定のセッション ID（UUID）を使用 | `claude --session-id "550e8400-..."` |
| `--fork-session` | 再開時に新しいセッション ID を作成 | `claude --resume abc123 --fork-session` |
| `--no-session-persistence` | セッションを保存しない（プリントモードのみ） | `claude -p --no-session-persistence "query"` |

### プリントモード（非インタラクティブ）

```bash
# 結果を出力して終了（スクリプトや CI/CD 向け）
claude -p "このコードを説明して"
claude --print "このコードを説明して"

# パイプで渡す
cat error.log | claude -p "エラーの原因を説明して"

# JSON 形式で出力
claude -p "query" --output-format json

# JSON スキーマに沿った出力（バリデーション付き）
claude -p --json-schema '{"type": "object", "properties": {...}}' "query"

# ストリーミング JSON
claude -p "query" --output-format stream-json

# エージェントのターン数を制限
claude -p --max-turns 3 "query"

# 予算を USD で制限
claude -p --max-budget-usd 5.00 "query"

# 過負荷時のフォールバックモデルを指定
claude -p --fallback-model sonnet "query"
```

### モデル・思考レベル

| フラグ | 説明 | 例 |
| --- | --- | --- |
| `--model` | モデルをエイリアスまたはフルネームで指定 | `claude --model claude-sonnet-4-6` |
| `--effort` | 努力レベル指定（low / medium / high / max） | `claude --effort high` |

```bash
# よく使うモデル指定
claude --model sonnet     # Sonnet 4.6（バランス型・省コスト）
claude --model opus       # Opus 4.6（高性能・複雑タスク向け）
claude --model haiku      # Haiku（軽量・高速・CI/CD 向け）
```

### システムプロンプトのカスタマイズ

| フラグ | 説明 | 例 |
| --- | --- | --- |
| `--system-prompt` | システムプロンプト全体を置き換える | `claude --system-prompt "あなたは Python の専門家です"` |
| `--system-prompt-file` | ファイルからシステムプロンプトを読み込む | `claude --system-prompt-file ./prompt.txt` |
| `--append-system-prompt` | デフォルトのシステムプロンプトに追記 | `claude --append-system-prompt "常に TypeScript を使って"` |
| `--append-system-prompt-file` | ファイルの内容をシステムプロンプトに追記 | `claude --append-system-prompt-file ./rules.txt` |

> **ヒント：** ほとんどの場合は `--append-*` フラグを使います。Claude Code の組み込み機能を保持したまま独自の指示を追加できます。

### パーミッション・セキュリティ

| フラグ | 説明 | 例 |
| --- | --- | --- |
| `--allowedTools` | 許可プロンプトなしにツールを許可 | `claude --allowedTools "Bash(git log *)" "Read"` |
| `--disallowedTools` | ツールを完全に無効化 | `claude --disallowedTools "Edit"` |
| `--tools` | 利用可能ツールを制限（`""` で全なし、`"default"` で全部） | `claude --tools "Bash,Edit,Read"` |
| `--permission-mode` | 起動時のパーミッションモードを指定 | `claude --permission-mode plan` |
| `--dangerously-skip-permissions` | すべてのパーミッションプロンプトをスキップ（CI/CD 用・要注意） | `claude --dangerously-skip-permissions` |

### MCP・プラグイン・外部連携

| フラグ | 説明 | 例 |
| --- | --- | --- |
| `--mcp-config` | JSON ファイルから MCP サーバーを読み込む | `claude --mcp-config ./mcp.json` |
| `--strict-mcp-config` | `--mcp-config` のみを使用し他の MCP ソースを無視 | `claude --strict-mcp-config --mcp-config ./mcp.json` |
| `--plugin-dir` | ディレクトリからプラグインを読み込む | `claude --plugin-dir ./my-plugins` |
| `--chrome` / `--no-chrome` | Chrome ブラウザ統合を有効/無効 | `claude --chrome` |
| `--ide` | 起動時に IDE に自動接続 | `claude --ide` |

### 出力・フォーマット

| フラグ | 説明 | 例 |
| --- | --- | --- |
| `--output-format` | 出力形式（text / json / stream-json） | `claude -p "query" --output-format json` |
| `--input-format` | 入力形式（text / stream-json） | `claude -p --input-format stream-json` |
| `--verbose` | 詳細ログを有効化 | `claude --verbose` |
| `--debug` | デバッグモード（カテゴリ指定可能） | `claude --debug "api,mcp"` |

### エージェント・サブエージェント

| フラグ | 説明 | 例 |
| --- | --- | --- |
| `--agent` | セッション用のエージェントを指定 | `claude --agent my-custom-agent` |
| `--agents` | JSON でカスタムサブエージェントを定義 | `claude --agents '{"reviewer":{...}}'` |
| `--teammate-mode` | エージェントチームの表示モード（auto / in-process / tmux） | `claude --teammate-mode tmux` |

### リモート・その他

| フラグ | 説明 | 例 |
| --- | --- | --- |
| `--remote` | claude.ai 上でウェブセッションを作成 | `claude --remote "バグを全部直して"` |
| `--rc`, `--remote-control` | リモートコントロールを有効にして起動 | `claude --rc "My Project"` |
| `--teleport` | リモートウェブセッションをローカルに引き継ぐ | `claude --teleport` |
| `--add-dir` | 追加の作業ディレクトリを指定 | `claude --add-dir ../apps ../lib` |
| `--from-pr` | GitHub PR に紐づいたセッションを再開 | `claude --from-pr 123` |
| `--init` / `--init-only` | セッションを初期化、または初期化フックのみ実行して終了 | `claude --init-only` |
| `--settings` | JSON ファイルまたは文字列から設定を読み込む | `claude --settings ./settings.json` |
| `--disable-slash-commands` | このセッションでスキルとコマンドを無効化 | `claude --disable-slash-commands` |

---

## 設定

### 設定ファイルの構成

Claude Code の設定はいくつかの場所に保存されます。

| ファイル | 対象 | 説明 |
| --- | --- | --- |
| `~/.claude/settings.json` | グローバル（ユーザー） | すべてのプロジェクトに適用される個人設定 |
| `.claude/settings.json` | プロジェクト | チーム共有の設定（Git 管理推奨） |
| `.claude/settings.local.json` | プロジェクトローカル | 個人用のプロジェクト設定（Git 除外推奨） |
| `~/.claude/CLAUDE.md` | グローバル | すべてのプロジェクトに適用されるメモリ |
| `.claude/CLAUDE.md` | プロジェクト | プロジェクト固有のメモリ・指示 |
| `.claude/skills/` | プロジェクト | プロジェクト共有のカスタムスキル |
| `~/.claude/skills/` | グローバル | 個人用のカスタムスキル |
| `.claude/agents/` | プロジェクト | カスタムサブエージェント定義 |

### settings.json の設定例

```json
{
  "model": "claude-sonnet-4-6",
  "fastMode": false,
  "theme": "dark",
  "env": {
    "CLAUDE_CODE_EFFORT_LEVEL": "medium",
    "BASH_DEFAULT_TIMEOUT_MS": "30000"
  }
}
```

### カスタムスキルの作成

スキルはスラッシュコマンドとして呼び出せる再利用可能なプロンプトです。

プロジェクト用スキルの場所：`.claude/skills/<スキル名>/SKILL.md`

個人用スキルの場所：`~/.claude/skills/<スキル名>/SKILL.md`

```bash
# スキルファイルを作成
mkdir -p .claude/skills/test-all
vim .claude/skills/test-all/SKILL.md
```

スキルのサンプル（`.claude/skills/test-all/SKILL.md`）：

```text
---
name: test-all
description: すべてのテストとリンターを実行する
argument-hint: "[テストスイート] [フラグ]"
allowed-tools: Bash(npm test:*), Bash(npm run:*)
model: sonnet
context: fork
---

以下のテストをすべて実行してください：

1. npm test $ARGUMENTS
2. npm run lint
3. npm run typecheck

結果のサマリーを表示してください。
```

作成後は `/test-all` で呼び出せます。

#### スキルのフロントマター一覧

| フィールド | 説明 |
| --- | --- |
| `name` | スキルの名前（スラッシュコマンド名） |
| `description` | スキルの説明（Claude による自動呼び出しの判断に使用） |
| `argument-hint` | 引数のヒントテキスト |
| `allowed-tools` | 許可するツールの一覧 |
| `model` | 使用するモデル |
| `disable-model-invocation` | モデル呼び出しを無効化（シェル処理のみ） |
| `user-invocable` | ユーザーが手動で呼び出せるか |
| `context` | `fork` を指定するとサブエージェントとして実行 |
| `agent` | 使用するエージェント（Explore / Plan など） |

スキル内で使える変数：`$ARGUMENTS`, `$0`, `$1`, `${CLAUDE_SKILL_DIR}`, `${CLAUDE_SESSION_ID}`

### カスタムサブエージェントの作成

特定のタスクに特化したエージェントを定義できます。

```bash
mkdir -p .claude/agents/code-reviewer
vim .claude/agents/code-reviewer/AGENT.md
```

```text
---
name: code-reviewer
description: コードレビューの専門家。コード変更後に積極的に使用してください。
tools: Read, Grep, Glob, Bash
model: sonnet
---

あなたはシニアコードレビュアーです。レビュー時は以下を確認してください：

- セキュリティの脆弱性（SQL インジェクション、XSS など）
- パフォーマンスの問題
- コードの可読性と保守性
- テストカバレッジ
```

---

## 環境変数

環境変数でさらに細かく動作をコントロールできます。
シェルで `export` するか、`settings.json` の `env` キーに設定すると永続化できます。

### モデル・パフォーマンス

| 変数 | 説明 | デフォルト |
| --- | --- | --- |
| `ANTHROPIC_MODEL` | デフォルトモデルを上書き | なし |
| `CLAUDE_CODE_EFFORT_LEVEL` | 努力レベル（low / medium / high） | high |
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS` | 1レスポンスの最大出力トークン数 | 32768 |
| `MAX_THINKING_TOKENS` | 拡張思考のトークン予算 | 10000 |
| `CLAUDE_CODE_SUBAGENT_MODEL` | サブエージェントが使用するモデル | なし |

### Bash・タイムアウト

| 変数 | 説明 | デフォルト |
| --- | --- | --- |
| `BASH_MAX_TIMEOUT_MS` | Bash コマンドの最大タイムアウト（ミリ秒） | 約60000 |
| `BASH_DEFAULT_TIMEOUT_MS` | Bash コマンドのデフォルトタイムアウト（ミリ秒） | 約60000 |
| `BASH_MAX_OUTPUT_LENGTH` | Bash 出力の最大文字数 | 30000 |
| `MAX_MCP_OUTPUT_TOKENS` | MCP ツール出力のトークン上限 | なし |

### 機能のオン・オフ

| 変数 | 説明 | 値 |
| --- | --- | --- |
| `CLAUDE_CODE_DISABLE_AUTO_MEMORY` | 自動メモリ作成を無効化 | `1` で無効 |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | バックグラウンドタスクを無効化 | `1` で無効 |
| `CLAUDE_CODE_DISABLE_CRON` | セッション中のスケジュールされた cron を停止 | `1` で無効 |
| `CLAUDE_CODE_DISABLE_1M_CONTEXT` | 1M トークンコンテキストウィンドウを無効化 | `1` で無効 |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` | システムプロンプトから git 指示を削除 | `1` で無効 |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | プロンプトサジェストを制御 | `false` で無効 |
| `ENABLE_TOOL_SEARCH` | MCP のツール検索（遅延ロード）を有効化 | `1` で有効 |
| `DISABLE_AUTOUPDATER` | 自動更新を無効化 | `1` で無効 |
| `DISABLE_TELEMETRY` | テレメトリを無効化 | `1` で無効 |
| `DISABLE_PROMPT_CACHING` | プロンプトキャッシュを無効化 | `1` で無効 |

### 認証・プロバイダー

| 変数 | 説明 | 備考 |
| --- | --- | --- |
| `ANTHROPIC_API_KEY` | Anthropic API キー | サブスクリプションがない場合に必要 |
| `CLAUDE_CODE_USE_BEDROCK` | AWS Bedrock をプロバイダーとして使用 | `1` で有効 |
| `CLAUDE_CODE_USE_VERTEX` | Google Vertex AI をプロバイダーとして使用 | `1` で有効 |
| `HTTP_PROXY` / `HTTPS_PROXY` | プロキシ設定 | 企業ネットワーク向け |

**settings.json に環境変数を永続化する例：**

```json
{
  "env": {
    "CLAUDE_CODE_EFFORT_LEVEL": "medium",
    "DISABLE_TELEMETRY": "1"
  }
}
```

---

## トラブルシューティング

### よくある問題と解決策

| 問題 | 原因 | 解決策 |
| --- | --- | --- |
| Claude がプロジェクトを理解しない | 起動フォルダが間違っている | プロジェクトルートで起動し、`/init` で CLAUDE.md を作成 |
| トークン上限に達した | コンテキストが多すぎる | `/compact` で圧縮、または `/clear` でリセット |
| コマンドが動かない | バージョンが古い | `claude update` で更新 |
| MCP サーバーが繋がらない | 設定ミス | `claude --debug "mcp"` で診断 |
| Shift+Enter が効かない | ターミナル設定が未実施 | `/terminal-setup` を実行 |

### デバッグコマンドまとめ

```bash
# インストール状態の確認
/doctor

# コンテキスト使用量の確認
/context

# トークンコストの確認
/cost

# 認証状態の確認
claude auth status --text

# 詳細ログを有効化して起動
claude --verbose

# デバッグモードで起動（カテゴリ指定）
claude --debug "api,mcp"

# セッションのトラブルシューティング
/debug "問題の説明"

# 設定画面を開く
/config
```

---

## 参考リンク

- [Claude Code 公式ドキュメント](https://docs.claude.com/en/docs/claude-code/overview)
- [CLI リファレンス](https://code.claude.com/docs/en/cli-reference)
- [npm パッケージ](https://www.npmjs.com/package/@anthropic-ai/claude-code)
- [Awesome Claude Code](https://github.com/hesreallyhim/awesome-claude-code)
- [Claude Code Guide（コミュニティ）](https://github.com/Cranot/claude-code-guide)
