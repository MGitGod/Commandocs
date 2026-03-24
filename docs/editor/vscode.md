# VSCode

Visual Studio Code（VSCode）の `code` CLI コマンド・キーボードショートカット・設定のまとめです。
ターミナルから使える `code` コマンドを中心に、エディタ操作全般を網羅しています。

---

## インストール・アップデート・アンインストール

### インストールする

=== "macOS (Homebrew)"

    ```bash
    brew install --cask visual-studio-code
    ```

=== "Ubuntu / Debian"

    ```bash
    # Microsoft の GPG キーとリポジトリを追加してインストール
    wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
    sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
    echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] \
      https://packages.microsoft.com/repos/code stable main" | \
      sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null
    sudo apt update
    sudo apt install code
    ```

=== "Windows (winget)"

    ```powershell
    winget install Microsoft.VisualStudioCode
    ```

!!! note "説明"
    macOS では Homebrew Cask でインストールするのが最も簡単です。
    Linux ではリポジトリを追加することで、以後 `apt upgrade` でアップデートも自動化できます。

#### 使用例

```bash
# macOS：インストール後に PATH を通す（ターミナルで code コマンドを使えるようにする）
# VSCode のコマンドパレット（Cmd+Shift+P）から
# "Shell Command: Install 'code' command in PATH" を実行する

# PATH が通っているか確認する
which code
# → /usr/local/bin/code などが表示されれば OK
```

---

### アップデートする

=== "macOS (Homebrew)"

    ```bash
    brew upgrade --cask visual-studio-code
    ```

=== "Ubuntu / Debian"

    ```bash
    sudo apt update && sudo apt upgrade code
    ```

=== "Windows (winget)"

    ```powershell
    winget upgrade Microsoft.VisualStudioCode
    ```

!!! note "説明"
    GUI からは **ヘルプ → 更新の確認** でもアップデートできます。
    自動更新が有効な環境では、再起動時に自動で最新版が適用されます。

#### 使用例

```bash
# 現在インストールされているバージョンを確認する
code --version
# → 1.90.0
#    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
#    x64
```

---

### アンインストールする

=== "macOS (Homebrew)"

    ```bash
    brew uninstall --cask visual-studio-code
    ```

=== "Ubuntu / Debian"

    ```bash
    sudo apt remove code
    ```

=== "Windows (winget)"

    ```powershell
    winget uninstall Microsoft.VisualStudioCode
    ```

!!! note "説明"
    設定・拡張機能のデータは `~/.vscode` および `~/Library/Application Support/Code`（macOS）に残ります。
    完全に削除したい場合はこれらのディレクトリも手動で削除してください。

#### 使用例

```bash
# macOS で設定ファイルも含めて完全削除する
rm -rf ~/.vscode
rm -rf ~/Library/Application\ Support/Code
```

---

## 基本コマンド

### ファイル・フォルダを開く

```bash
code <ファイルまたはディレクトリのパス>
```

!!! note "説明"
    最もよく使う基本コマンドです。
    パスにはファイル・ディレクトリどちらも指定できます。
    引数なしで `code` を実行すると VSCode が起動するだけです。

#### 使用例

```bash
# カレントディレクトリをプロジェクトとして開く
code .

# 特定のファイルを開く
code README.md

# 複数のファイルを同時に開く
code index.html style.css app.js

# ホームディレクトリのプロジェクトフォルダを開く
code ~/projects/my-app
```

---

### バージョンを確認する

```bash
code --version
```

!!! note "説明"
    インストールされている VSCode のバージョン・コミットハッシュ・アーキテクチャを表示します。
    バグ報告や環境確認のときに使います。

#### 使用例

```bash
code --version
# → 1.90.0
#    abcdef1234567890abcdef1234567890abcdef12
#    x64
```

---

### ヘルプを表示する

```bash
code --help
```

!!! note "説明"
    使用できるすべての CLI オプションとその説明を表示します。
    使い方を忘れたときに役立ちます。

#### 使用例

```bash
code --help
# → Usage: code [options][paths...]
#    オプション一覧が出力される
```

---

## よく使うコマンド

### 新しいウィンドウで開く

```bash
code --new-window <パス>
# 短縮形
code -n <パス>
```

!!! note "説明"
    既存のウィンドウには影響せず、新しいウィンドウを起動してファイル・フォルダを開きます。
    複数プロジェクトを並行して作業するときに便利です。

#### 使用例

```bash
# 新しいウィンドウでカレントディレクトリを開く
code -n .

# 新しいウィンドウで別プロジェクトを開く
code -n ~/projects/another-app
```

---

### 既存ウィンドウで開く

```bash
code --reuse-window <パス>
# 短縮形
code -r <パス>
```

!!! note "説明"
    最後にフォーカスしていた VSCode ウィンドウでファイルを開きます。
    新しいウィンドウを増やしたくないときに使います。

#### 使用例

```bash
# 既存ウィンドウで設定ファイルを開く
code -r ~/.zshrc
```

---

### ファイルの差分を表示する

```bash
code --diff <ファイル1> <ファイル2>
```

!!! note "説明"
    2 つのファイルの差分を VSCode の diff ビューアで表示します。
    `git diff` の代わりにビジュアルで確認したいときに便利です。

#### 使用例

```bash
# 2 ファイルの差分を確認する
code --diff old_config.json new_config.json

# git のマージコンフリクト解消に使う
code --diff HEAD~1:README.md README.md
```

---

### 指定行・列にジャンプして開く

```bash
code --goto <ファイル:行[:列]>
# 短縮形
code -g <ファイル:行[:列]>
```

!!! note "説明"
    ファイルを開いたとき、指定した行・列にカーソルを移動します。
    エラーログのパスをそのままコピーして開くときに役立ちます。

#### 使用例

```bash
# app.js の 42 行目を開く
code -g app.js:42

# app.js の 42 行目・10 列目を開く
code -g app.js:42:10
```

---

### 拡張機能をインストールする

```bash
code --install-extension <拡張機能ID>
```

!!! note "説明"
    拡張機能の ID（`発行者.拡張機能名` の形式）を指定してインストールします。
    複数指定する場合はフラグを繰り返します。
    拡張機能 ID は Marketplace のページ URL や `--list-extensions` で確認できます。

#### 使用例

```bash
# Python 拡張機能をインストールする
code --install-extension ms-python.python

# GitLens をインストールする
code --install-extension eamodio.gitlens

# 複数まとめてインストールする
code --install-extension ms-python.python \
     --install-extension dbaeumer.vscode-eslint \
     --install-extension esbenp.prettier-vscode
```

---

### 拡張機能をアンインストールする

```bash
code --uninstall-extension <拡張機能ID>
```

!!! note "説明"
    指定した拡張機能をアンインストールします。
    ID は `--list-extensions` で確認できます。

#### 使用例

```bash
# ESLint 拡張機能をアンインストールする
code --uninstall-extension dbaeumer.vscode-eslint
```

---

### インストール済み拡張機能を一覧表示する

```bash
code --list-extensions
```

!!! note "説明"
    現在インストールされているすべての拡張機能の ID を一覧表示します。
    環境のバックアップや新環境への移行に活用できます。

#### 使用例

```bash
# 一覧をファイルに保存する（環境バックアップ）
code --list-extensions > extensions.txt

# バージョン番号も含めて一覧表示する
code --list-extensions --show-versions

# 保存した一覧から一括インストールする
cat extensions.txt | xargs -L1 code --install-extension
```

---

### コマンドの完了を待機する

```bash
code --wait <ファイル>
# 短縮形
code -w <ファイル>
```

!!! note "説明"
    VSCode でファイルを開き、そのウィンドウが閉じられるまでターミナルをブロックします。
    git のコミットメッセージエディタや、スクリプトから VSCode を呼び出すときに使います。

#### 使用例

```bash
# git のエディタとして VSCode を設定する
git config --global core.editor "code --wait"

# git commit のメッセージを VSCode で編集する
git commit
# → VSCode が開き、閉じると git commit が続行される
```

---

## 上級者向けコマンド

### ステータス情報を表示する

```bash
code --status
```

!!! note "説明"
    実行中のプロセス・ログパス・拡張機能ホストの状態など、診断に役立つ情報を出力します。
    VSCode が重い・動作がおかしいときのデバッグに使います。

#### 使用例

```bash
code --status
# → Version: 1.90.0
#    OS Version: Darwin x64 ...
#    CPUs: ...
#    Memory: ...
#    ログパスや実行中プロセスが表示される
```

---

### ログレベルを指定して起動する

```bash
code --log <level>
```

!!! note "説明"
    起動時のログの詳細レベルを指定します。
    `critical` / `error` / `warn` / `info` / `debug` / `trace` / `off` が指定できます。
    問題の調査をするときは `debug` や `trace` が役立ちます。

#### 使用例

```bash
# デバッグログを有効にして起動する
code --log debug

# ログを無効にして起動する（パフォーマンス優先）
code --log off
```

---

### 起動プロファイルを計測する

```bash
code --prof-startup
```

!!! note "説明"
    起動時のパフォーマンスプロファイルを記録します。
    VSCode の起動が遅い原因を調査するときに使います。
    プロファイルデータは一時ディレクトリに保存されます。

#### 使用例

```bash
# 起動プロファイルを取得する
code --prof-startup .

# プロファイルファイルが保存されたパスを確認する
code --status | grep "Logs Path"
```

---

### すべての拡張機能を無効にして起動する

```bash
code --disable-extensions
```

!!! note "説明"
    インストール済みの拡張機能をすべて無効にして VSCode を起動します。
    「拡張機能が原因で動作がおかしい」と疑われる場合の切り分けに使います。

#### 使用例

```bash
# 拡張機能なしで起動して問題を切り分ける
code --disable-extensions .

# 特定の拡張機能だけ無効にして起動する
code --disable-extension ms-python.python .
```

---

### GPU アクセラレーションを無効にして起動する

```bash
code --disable-gpu
```

!!! note "説明"
    GPU レンダリングを無効にして起動します。
    画面のちらつき・クラッシュ・描画崩れが発生するときの回避策として有効です。

#### 使用例

```bash
# GPU 無効で起動する（描画崩れが起きるときに試す）
code --disable-gpu .
```

---

### ユーザーデータディレクトリを指定して起動する

```bash
code --user-data-dir <ディレクトリパス>
```

!!! note "説明"
    設定・キーバインド・ワークスペース情報を保存するディレクトリを変更して起動します。
    設定を分離した「クリーンな VSCode 環境」で動作確認したいときに使います。

#### 使用例

```bash
# 一時ディレクトリを使って設定ゼロの状態で起動する
code --user-data-dir /tmp/vscode-clean .

# プロジェクト専用の設定で起動する
code --user-data-dir ~/vscode-profiles/work .
```

---

### 拡張機能ディレクトリを指定して起動する

```bash
code --extensions-dir <ディレクトリパス>
```

!!! note "説明"
    拡張機能のインストール先ディレクトリを変更して起動します。
    `--user-data-dir` と組み合わせることで、完全に独立した VSCode 環境を作れます。

#### 使用例

```bash
# 拡張機能ディレクトリと設定ディレクトリを両方カスタムにする
code --user-data-dir /tmp/vscode-test \
     --extensions-dir /tmp/vscode-test-ext \
     .
```

---

### 詳細ログを出力する

```bash
code --verbose
```

!!! note "説明"
    すべての操作の詳細ログをターミナルに出力します。
    問題の診断やデバッグ時に `--log debug` と合わせて使うと効果的です。

#### 使用例

```bash
# 詳細ログを有効にして起動する
code --verbose .
```

---

## 知ってると便利なコマンド

### 標準入力からファイルを開く

```bash
<コマンド> | code -
```

!!! note "説明"
    パイプでつなぐことで、標準入力の内容を新しいファイルとして VSCode で開きます。
    コマンドの出力結果をすぐに確認・編集したいときに便利です。

#### 使用例

```bash
# ls の結果を VSCode で開く
ls -la | code -

# curl の結果（JSON など）を VSCode で開く
curl https://api.example.com/data | code -

# git log を VSCode で見る
git log --oneline | code -
```

---

### ワークスペースファイルを開く

```bash
code <ファイル名>.code-workspace
```

!!! note "説明"
    `.code-workspace` ファイルを開くことで、複数フォルダを含むマルチルートワークスペースを起動できます。
    モノレポや関連プロジェクトを同時に管理するときに便利です。

#### 使用例

```bash
# ワークスペースファイルを開く
code myproject.code-workspace
```

---

### コマンドラインから設定を変更する（settings CLI）

```bash
# settings.json のパスを直接開いて編集する
code ~/.config/Code/User/settings.json
```

!!! note "説明"
    `settings.json` を直接開いて編集することで、GUI を使わずに設定変更できます。
    パスは OS によって異なります（後述の「設定」セクションも参照）。

#### 使用例

```bash
# macOS の settings.json を開く
code ~/Library/Application\ Support/Code/User/settings.json

# Linux の settings.json を開く
code ~/.config/Code/User/settings.json

# Windows の settings.json を開く（PowerShell）
code $env:APPDATA\Code\User\settings.json
```

---

### トンネル（リモートアクセス）を作成する

```bash
code tunnel
```

!!! note "説明"
    VSCode Server のトンネルを作成し、ブラウザや別の VSCode から遠隔マシンに接続できるようにします。
    SSH が使えない環境でのリモート開発に役立ちます（要 GitHub / Microsoft アカウント）。

#### 使用例

```bash
# トンネルを起動する（初回はアカウント認証が必要）
code tunnel

# トンネルをサービスとして登録する（ログアウト後も動作）
code tunnel service install

# トンネルの状態を確認する
code tunnel status

# トンネルサービスを停止・削除する
code tunnel service uninstall
```

---

### 拡張機能の VSIX ファイルをインストールする

```bash
code --install-extension <ファイル名>.vsix
```

!!! note "説明"
    Marketplace を経由せず、ローカルの `.vsix` ファイルから拡張機能をインストールします。
    オフライン環境・社内配布・ベータ版テストに使います。

#### 使用例

```bash
# ローカルの VSIX ファイルをインストールする
code --install-extension my-extension-1.0.0.vsix

# VSIX ファイルをエクスポートする（拡張機能開発者向け）
# vsce コマンドが必要（npm install -g @vscode/vsce）
vsce package
# → my-extension-1.0.0.vsix が生成される
```

---

### キーボードショートカット（よく使うもの）

!!! note "説明"
    CLI コマンドではありませんが、日常作業で特に重要なショートカットをまとめています。
    `Ctrl` は macOS では `Cmd` に読み替えてください。

#### 使用例

=== "ファイル操作"

    ```text
    Ctrl+P              クイックオープン（ファイル名検索）
    Ctrl+Shift+P        コマンドパレットを開く
    Ctrl+N              新規ファイルを作成する
    Ctrl+S              上書き保存する
    Ctrl+Shift+S        名前を付けて保存する
    Ctrl+W              タブを閉じる
    Ctrl+K Ctrl+W       すべてのタブを閉じる
    Ctrl+Z              元に戻す
    Ctrl+Shift+Z        やり直す
    ```

=== "編集・カーソル操作"

    ```text
    Ctrl+/              行コメントをトグルする
    Ctrl+Shift+/        ブロックコメントをトグルする
    Alt+Up / Alt+Down   行を上下に移動する
    Shift+Alt+Up/Down   行を上下にコピーする
    Ctrl+D              同じ単語を連続選択する（マルチカーソル）
    Ctrl+Shift+L        同じ単語をすべて選択する
    Ctrl+G              指定行にジャンプする
    Home / End          行頭 / 行末に移動する
    Ctrl+Home / End     ファイルの先頭 / 末尾に移動する
    ```

=== "検索・置換"

    ```text
    Ctrl+F              現在のファイル内を検索する
    Ctrl+H              現在のファイル内で置換する
    Ctrl+Shift+F        ワークスペース全体を検索する
    Ctrl+Shift+H        ワークスペース全体で置換する
    F3 / Shift+F3       次 / 前の検索結果に移動する
    ```

=== "表示・レイアウト"

    ```text
    Ctrl+B              サイドバーを開閉する
    Ctrl+`              統合ターミナルを開閉する
    Ctrl+Shift+`        新しいターミナルを開く
    Ctrl+\ （バックスラッシュ）  エディタを分割する
    Ctrl+1 / 2 / 3      分割したエディタを切り替える
    Ctrl+= / -          フォントサイズを拡大 / 縮小する
    Ctrl+Shift+E        エクスプローラーを開く
    Ctrl+Shift+G        ソース管理パネルを開く
    Ctrl+Shift+X        拡張機能パネルを開く
    Ctrl+Shift+D        デバッグパネルを開く
    ```

=== "コード補完・IntelliSense"

    ```text
    Ctrl+Space          補完候補を表示する
    Ctrl+Shift+Space    シグネチャヘルプを表示する
    F12                 定義に移動する
    Alt+F12             定義をピーク表示する
    Shift+F12           参照をすべて表示する
    F2                  シンボルをリネームする
    Shift+Alt+F         ドキュメントをフォーマットする
    ```

---

## オプション

### オプション一覧

| オプション | 短縮形 | 説明 |
| --- | --- | --- |
| `--new-window` | `-n` | 新しいウィンドウで開く |
| `--reuse-window` | `-r` | 既存のウィンドウで開く |
| `--diff <file1> <file2>` | - | 2 ファイルの差分を表示する |
| `--merge <base> <theirs> <ours> <result>` | - | マージエディタを開く |
| `--goto <file:line[:col]>` | `-g` | 指定行・列を開く |
| `--wait` | `-w` | ウィンドウが閉じられるまで待機する |
| `--install-extension <id>` | - | 拡張機能をインストールする |
| `--uninstall-extension <id>` | - | 拡張機能をアンインストールする |
| `--list-extensions` | - | インストール済み拡張機能を一覧表示する |
| `--show-versions` | - | `--list-extensions` と組み合わせてバージョンも表示する |
| `--disable-extensions` | - | すべての拡張機能を無効にして起動する |
| `--disable-extension <id>` | - | 指定の拡張機能だけ無効にして起動する |
| `--disable-gpu` | - | GPU アクセラレーションを無効にする |
| `--user-data-dir <dir>` | - | ユーザーデータディレクトリを変更する |
| `--extensions-dir <dir>` | - | 拡張機能ディレクトリを変更する |
| `--log <level>` | - | ログレベルを指定する |
| `--verbose` | - | 詳細ログを出力する |
| `--prof-startup` | - | 起動プロファイルを記録する |
| `--status` | - | ステータス情報を表示する |
| `--version` | `-v` | バージョンを表示する |
| `--help` | `-h` | ヘルプを表示する |

---

## 設定

### settings.json のパスを確認する

```bash
# macOS
~/Library/Application Support/Code/User/settings.json

# Linux
~/.config/Code/User/settings.json

# Windows
%APPDATA%\Code\User\settings.json
```

!!! note "説明"
    VSCode のすべての設定は `settings.json` に JSON 形式で保存されます。
    GUI の設定画面（`Ctrl+,`）で変更した内容はこのファイルに反映されます。

#### 使用例

```bash
# settings.json を VSCode で開く（macOS）
code ~/Library/Application\ Support/Code/User/settings.json

# settings.json を VSCode で開く（Linux）
code ~/.config/Code/User/settings.json
```

---

### よく使う settings.json の設定例

```json
{
  // フォントサイズを変更する
  "editor.fontSize": 14,

  // フォントファミリーを指定する
  "editor.fontFamily": "'JetBrains Mono', 'Fira Code', monospace",

  // リガチャ（合字）を有効にする
  "editor.fontLigatures": true,

  // タブをスペースに変換する
  "editor.insertSpaces": true,

  // タブ幅を 2 スペースにする
  "editor.tabSize": 2,

  // 保存時に自動フォーマットする
  "editor.formatOnSave": true,

  // 保存時に末尾の空白を削除する
  "files.trimTrailingWhitespace": true,

  // 保存時に末尾に改行を追加する
  "files.insertFinalNewline": true,

  // 自動保存を設定する（off / afterDelay / onFocusChange / onWindowChange）
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,

  // ミニマップを非表示にする
  "editor.minimap.enabled": false,

  // 行末の表示形式を統一する（LF に固定）
  "files.eol": "\n",

  // ターミナルのフォントサイズを変更する
  "terminal.integrated.fontSize": 13,

  // デフォルトのフォーマッタを指定する（Prettier など）
  "editor.defaultFormatter": "esbenp.prettier-vscode",

  // ブラケットのカラーペアリングを有効にする
  "editor.bracketPairColorization.enabled": true,

  // 空白文字を表示する
  "editor.renderWhitespace": "boundary",

  // ワードラップを有効にする
  "editor.wordWrap": "on"
}
```

---

### keybindings.json のパスを確認する

```bash
# macOS
~/Library/Application Support/Code/User/keybindings.json

# Linux
~/.config/Code/User/keybindings.json
```

!!! note "説明"
    キーバインドのカスタマイズは `keybindings.json` で行います。
    `Ctrl+K Ctrl+S` または コマンドパレットから「キーボードショートカットを開く (JSON)」で開けます。

#### 使用例

```json
// keybindings.json の記述例
[
  {
    // Ctrl+Shift+T で新しいターミナルを開く（上書き設定の例）
    "key": "ctrl+shift+t",
    "command": "workbench.action.terminal.new"
  },
  {
    // Ctrl+Alt+F でファイル全体をフォーマットする
    "key": "ctrl+alt+f",
    "command": "editor.action.formatDocument"
  }
]
```

---

### tasks.json でタスクを自動化する

```json
// .vscode/tasks.json
{
  "version": "2.0.0",
  "tasks": [
    {
      // タスク名
      "label": "MkDocs サーバーを起動する",
      "type": "shell",
      "command": "mkdocs serve",
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "presentation": {
        "reveal": "always",
        "panel": "new"
      }
    }
  ]
}
```

!!! note "説明"
    `Ctrl+Shift+B` でデフォルトビルドタスクを実行できます。
    MkDocs・npm スクリプト・テスト実行など、よく使うコマンドを登録しておくと便利です。

---

### launch.json でデバッグ設定をする

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: 現在のファイル",
      "type": "debugpy",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal"
    }
  ]
}
```

!!! note "説明"
    `F5` でデバッグを開始できます。
    `${file}` は現在開いているファイルのパスに展開されます。

---

## 頻出コマンド早引き

```bash
# カレントディレクトリを開く（最もよく使う）
code .

# 新しいウィンドウで開く
code -n .

# ファイルの差分を確認する
code --diff <ファイル1> <ファイル2>

# 指定行を開く
code -g <ファイル>:<行>

# 拡張機能をインストールする
code --install-extension <拡張機能ID>

# インストール済み拡張機能を一覧表示してファイルに保存する
code --list-extensions > extensions.txt

# 保存済み一覧から一括インストールする
cat extensions.txt | xargs -L1 code --install-extension

# 拡張機能なしで起動する（トラブルシューティング）
code --disable-extensions .

# git コミットメッセージエディタを VSCode にする
git config --global core.editor "code --wait"

# 標準入力の内容を VSCode で開く
<コマンド> | code -

# バージョン確認
code --version

# ステータス確認（重い・おかしいとき）
code --status
```

---

!!! info "公式ドキュメント"
    - [Visual Studio Code ドキュメント（英語）](https://code.visualstudio.com/docs)
    - [VS Code CLI リファレンス](https://code.visualstudio.com/docs/editor/command-line)
    - [キーボードショートカット（macOS）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
    - [キーボードショートカット（Windows）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
    - [キーボードショートカット（Linux）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
    - [拡張機能 Marketplace](https://marketplace.visualstudio.com/vscode)
