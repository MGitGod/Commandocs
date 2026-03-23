# tmux

tmux はターミナルマルチプレクサで、1つのターミナルウィンドウ内で複数のセッション・ウィンドウ・ペインを管理できます。
プレフィックスキーはデフォルトで `Ctrl+b` です（以下 `<prefix>` と表記）。

---

## 基本コマンド

### セッション操作

| コマンド                            | 説明                                          |
| ----------------------------------- | --------------------------------------------- |
| `tmux`                              | 新しいセッションを開始する                    |
| `tmux new -s セッション名`          | 名前付きセッションを開始する                  |
| `tmux ls`                           | セッション一覧を表示する                      |
| `tmux attach`                       | 直近のセッションにアタッチする                |
| `tmux attach -t セッション名`       | 指定したセッションにアタッチする              |
| `tmux kill-session -t セッション名` | 指定したセッションを終了する                  |
| `tmux kill-server`                  | tmux サーバーごとすべてのセッションを終了する |

**使い方の例：**

```bash
# "work" という名前でセッションを新規作成
tmux new -s work

# セッション一覧を確認
tmux ls
# 出力例: work: 1 windows (created Thu Mar 19 10:00:00 2026)

# "work" セッションにアタッチ
tmux attach -t work
```

### ペイン内での基本操作

| キー操作     | 説明                                               |
| ------------ | -------------------------------------------------- |
| `<prefix> d` | セッションをデタッチする（バックグラウンドで継続） |
| `<prefix> ?` | キーバインド一覧を表示する                         |
| `<prefix> :` | コマンドプロンプトを開く                           |

**使い方の例：**

```text
# セッションをデタッチ（バックグラウンドで処理を継続したまま離れる）
Ctrl+b → d

# キーバインドの確認（q で閉じる）
Ctrl+b → ?
```

---

## よく使うコマンド

### ウィンドウ操作

| キー操作        | 説明                                               |
| --------------- | -------------------------------------------------- |
| `<prefix> c`    | 新しいウィンドウを作成する                         |
| `<prefix> ,`    | 現在のウィンドウ名を変更する                       |
| `<prefix> w`    | ウィンドウ一覧を表示・選択する                     |
| `<prefix> n`    | 次のウィンドウへ移動する                           |
| `<prefix> p`    | 前のウィンドウへ移動する                           |
| `<prefix> 数字` | 指定番号のウィンドウへ移動する（例: `<prefix> 0`） |
| `<prefix> &`    | 現在のウィンドウを閉じる（確認あり）               |
| `<prefix> l`    | 直前に表示していたウィンドウへ切り替える           |

**使い方の例：**

```text
# ウィンドウを2つ作り、それぞれに名前をつける
Ctrl+b → c       # 新ウィンドウ作成
Ctrl+b → ,       # "editor" と入力してリネーム
Ctrl+b → c
Ctrl+b → ,       # "server" と入力してリネーム

# ウィンドウ番号で切り替え
Ctrl+b → 0       # 1番目のウィンドウへ
Ctrl+b → 1       # 2番目のウィンドウへ
```

### ペイン操作

| キー操作            | 説明                                     |
| ------------------- | ---------------------------------------- |
| `<prefix> %`        | 縦方向に分割（左右2ペイン）              |
| `<prefix> "`        | 横方向に分割（上下2ペイン）              |
| `<prefix> 矢印キー` | 隣のペインへ移動する                     |
| `<prefix> o`        | 次のペインへ順番に移動する               |
| `<prefix> x`        | 現在のペインを閉じる（確認あり）         |
| `<prefix> z`        | 現在のペインを最大化・元に戻す（ズーム） |
| `<prefix> q`        | ペイン番号を一時的に表示する             |
| `<prefix> q 数字`   | 指定番号のペインへ移動する               |
| `<prefix> {`        | ペインを左（前）に移動する               |
| `<prefix> }`        | ペインを右（後）に移動する               |

**使い方の例：**

```text
# 画面を左右に分割してエディタとターミナルを並べる
Ctrl+b → %       # 左右に分割
Ctrl+b → →       # 右のペインへ移動

# さらに右ペインを上下に分割
Ctrl+b → "       # 上下に分割

# 作業中のペインを一時的に全画面表示
Ctrl+b → z       # ズーム（もう一度押すと元に戻る）

# ペイン番号を表示して素早く移動
Ctrl+b → q → 2   # ペイン番号2へジャンプ
```

### ペインのリサイズ

| キー操作             | 説明                            |
| -------------------- | ------------------------------- |
| `<prefix> Ctrl+矢印` | ペインを矢印方向に1単位リサイズ |
| `<prefix> Alt+矢印`  | ペインを矢印方向に5単位リサイズ |

**使い方の例：**

```text
# 現在のペインを右方向に広げる
Ctrl+b → Ctrl+→  # 1単位ずつ拡大
Ctrl+b → Alt+→   # 5単位ずつ拡大（素早いリサイズ）
```

### コピーモード（スクロール・テキスト選択）

| キー操作                  | 説明                                 |
| ------------------------- | ------------------------------------ |
| `<prefix> [`              | コピーモードに入る（vi風操作が可能） |
| `q` または `Esc`          | コピーモードを抜ける                 |
| `Space`（コピーモード内） | 選択開始（vi モード）                |
| `Enter`（コピーモード内） | 選択範囲をコピーしてモードを抜ける   |
| `<prefix> ]`              | コピーしたテキストをペーストする     |
| `↑ / ↓` または `j / k`    | スクロールする                       |
| `Ctrl+u / Ctrl+d`         | 半ページ上下スクロール               |
| `/`                       | 前方検索（コピーモード内）           |
| `?`                       | 後方検索（コピーモード内）           |

**使い方の例：**

```text
# ログを遡って確認する
Ctrl+b → [       # コピーモードへ
Ctrl+u           # 上にスクロール
/エラー           # "エラー" という文字列を検索
n                # 次の一致へ
q                # コピーモードを抜ける
```

---

## 上級者向けコマンド

### セッション管理の応用

| コマンド                           | 説明                                       |
| ---------------------------------- | ------------------------------------------ |
| `tmux new -s 名前 -d`              | セッションをバックグラウンドで作成する     |
| `tmux rename-session -t 旧名 新名` | セッション名を変更する                     |
| `tmux switch -t セッション名`      | 別のセッションに切り替える（デタッチせず） |
| `<prefix> $`                       | 現在のセッション名を変更する               |
| `<prefix> s`                       | セッション一覧から選択して切り替える       |
| `<prefix> (`                       | 前のセッションへ切り替える                 |
| `<prefix> )`                       | 次のセッションへ切り替える                 |

**使い方の例：**

```bash
# バックグラウンドで "build" セッションを起動してコマンドを実行
tmux new -s build -d
tmux send-keys -t build "make all" Enter

# セッション名を変更
tmux rename-session -t build deploy
```

### ペインの高度な操作

| キー操作 / コマンド                  | 説明                                         |
| ------------------------------------ | -------------------------------------------- |
| `<prefix> !`                         | 現在のペインを新しいウィンドウとして切り出す |
| `<prefix> Space`                     | プリセットレイアウトを順番に切り替える       |
| `tmux select-layout even-horizontal` | 左右均等レイアウトにする                     |
| `tmux select-layout even-vertical`   | 上下均等レイアウトにする                     |
| `tmux select-layout tiled`           | タイル状レイアウトにする                     |
| `tmux select-layout main-horizontal` | メイン+下部ペインレイアウトにする            |
| `tmux select-layout main-vertical`   | メイン+右側ペインレイアウトにする            |

**使い方の例：**

```text
# ペインが増えて見づらくなったときにレイアウトを整える
Ctrl+b → Space   # プリセットレイアウトを順番に切り替えて確認

# コマンドプロンプトからレイアウトを均等に整える
Ctrl+b → :
select-layout even-horizontal
```

### スクリプトからの操作（自動化）

| コマンド                                             | 説明                                       |
| ---------------------------------------------------- | ------------------------------------------ |
| `tmux send-keys -t セッション名 "コマンド" Enter`    | 指定セッションにキー入力を送る             |
| `tmux split-window -h -t セッション名`               | 指定セッションで縦分割する                 |
| `tmux split-window -v -t セッション名`               | 指定セッションで横分割する                 |
| `tmux select-pane -t セッション名:ウィンドウ.ペイン` | 指定ペインを選択する                       |
| `tmux new-window -t セッション名 -n ウィンドウ名`    | 指定セッションに新しいウィンドウを追加する |
| `tmux display-message -p '#S'`                       | 現在のセッション名を取得する               |
| `tmux display-message -p '#W'`                       | 現在のウィンドウ名を取得する               |

**使い方の例：**

```bash
#!/bin/bash
# 開発環境を一発で立ち上げるスクリプト例

SESSION="dev"

# セッションを作成（すでに存在する場合はアタッチ）
tmux new-session -d -s "$SESSION" -n "editor"

# 左ペインでエディタを起動
tmux send-keys -t "$SESSION:editor" "vim ." Enter

# 右ペインでサーバーを起動
tmux split-window -h -t "$SESSION:editor"
tmux send-keys -t "$SESSION:editor.1" "python -m http.server 8000" Enter

# 新しいウィンドウでログを監視
tmux new-window -t "$SESSION" -n "logs"
tmux send-keys -t "$SESSION:logs" "tail -f /var/log/syslog" Enter

# 作成したセッションにアタッチ
tmux attach -t "$SESSION"
```

### 設定ファイルの操作

| コマンド                             | 説明                                     |
| ------------------------------------ | ---------------------------------------- |
| `tmux source-file ~/.tmux.conf`      | 設定ファイルをリロードする               |
| `<prefix> :source-file ~/.tmux.conf` | コマンドプロンプトから設定をリロードする |
| `tmux show-options -g`               | グローバルオプションを一覧表示する       |
| `tmux show-options -s`               | サーバーオプションを一覧表示する         |
| `tmux set-option -g オプション名 値` | グローバルオプションを変更する           |

**使い方の例：**

```bash
# ~/.tmux.conf の設定を即座に反映する
tmux source-file ~/.tmux.conf

# マウス操作を有効にする（コマンドプロンプトから入力）
set-option -g mouse on
```

---

## 知ってると便利なコマンド

### 情報確認・デバッグ

| コマンド                   | 説明                                  |
| -------------------------- | ------------------------------------- |
| `tmux info`                | tmux サーバーのすべての情報を表示する |
| `tmux list-keys`           | すべてのキーバインドを表示する        |
| `tmux list-commands`       | すべての tmux コマンドを表示する      |
| `tmux show-environment`    | 環境変数を表示する                    |
| `tmux show-environment -g` | グローバル環境変数を表示する          |
| `<prefix> t`               | 現在時刻を大きく表示する              |

**使い方の例：**

```bash
# 利用可能なコマンドを調べる
tmux list-commands | grep session

# キーバインドを確認する
tmux list-keys | grep split
```

### ペインの同期入力

| キー操作 / コマンド                            | 説明                                     |
| ---------------------------------------------- | ---------------------------------------- |
| `tmux set-window-option synchronize-panes on`  | すべてのペインに同じ入力を送る（同期ON） |
| `tmux set-window-option synchronize-panes off` | ペイン同期をOFFにする                    |
| `<prefix> : setw synchronize-panes`            | コマンドプロンプトからトグルで切り替え   |

**使い方の例：**

```text
# 複数サーバーへの同時コマンド実行
# 1. 各ペインで別々のサーバーに SSH 接続しておく
# 2. コマンドプロンプトで同期を有効化
Ctrl+b → :
setw synchronize-panes on

# 全ペインに同じコマンドが送られる
uptime

# 終わったら同期を解除
Ctrl+b → :
setw synchronize-panes off
```

### ログ・記録

| コマンド                                     | 説明                                 |
| -------------------------------------------- | ------------------------------------ |
| `<prefix> :pipe-pane -o "cat >> ~/tmux.log"` | 現在のペイン出力をファイルに記録する |
| `<prefix> :pipe-pane`                        | ログ記録を停止する                   |
| `tmux capture-pane -p`                       | 現在のペイン内容を標準出力に出力する |
| `tmux capture-pane -p -t セッション名`       | 指定ペインの内容を取得する           |
| `tmux save-buffer ファイル名`                | コピーバッファをファイルに保存する   |

**使い方の例：**

```bash
# 実行中のペイン内容をキャプチャしてファイルに保存
tmux capture-pane -p -t work > ~/session_output.txt

# コピーバッファをファイルに保存
tmux save-buffer ~/clipboard.txt
```

### ~/.tmux.conf の便利な設定例

```bash
# プレフィックスキーを Ctrl+a に変更（GNU Screen 風）
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

# マウス操作を有効にする
set-option -g mouse on

# ウィンドウ番号を1始まりにする
set -g base-index 1
setw -g pane-base-index 1

# 設定リロードを <prefix> r で行う
bind r source-file ~/.tmux.conf \; display "設定をリロードしました"

# ペイン分割のキーバインドを直感的に変更
bind | split-window -h   # | で縦分割
bind - split-window -v   # - で横分割

# ペイン移動を vim 風に
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# ステータスバーの更新間隔（秒）
set -g status-interval 5

# コピーモードを vi 風にする
setw -g mode-keys vi

# vi 風コピーモードのキーバインド
bind -T copy-mode-vi v send-keys -X begin-selection
bind -T copy-mode-vi y send-keys -X copy-selection-and-cancel
```

### ショートカット早見表

```text
セッション操作
  tmux new -s <名前>        新規セッション作成
  tmux attach -t <名前>     セッションにアタッチ
  tmux ls                   セッション一覧
  <prefix> d                デタッチ
  <prefix> s                セッション一覧＆切り替え

ウィンドウ操作
  <prefix> c                新規ウィンドウ
  <prefix> ,                ウィンドウ名変更
  <prefix> n / p            次/前のウィンドウ
  <prefix> 数字              番号指定でウィンドウ移動
  <prefix> &                ウィンドウを閉じる

ペイン操作
  <prefix> %                左右に分割
  <prefix> "                上下に分割
  <prefix> 矢印              ペイン移動
  <prefix> z                ズーム切り替え
  <prefix> x                ペインを閉じる
  <prefix> Space            レイアウト切り替え

コピー操作
  <prefix> [                コピーモード開始
  Space → Enter             選択してコピー（vi モード）
  <prefix> ]                ペースト
```
