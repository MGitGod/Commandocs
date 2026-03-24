# Git / GitHub

Git はファイルの変更履歴を管理する分散型バージョン管理システムです。  
GitHub は Git リポジトリをクラウドでホスティングするサービスで、チーム開発・OSS 公開に広く使われます。

---

## インストール・アップデート・アンインストール

### インストール

=== "macOS"

    ```bash
    # Homebrew を使ったインストール
    brew install git
    ```

=== "Ubuntu / Debian"

    ```bash
    # apt を使ったインストール
    sudo apt update
    sudo apt install git
    ```

=== "Windows"

    ```bash
    # winget を使ったインストール
    winget install --id Git.Git -e --source winget
    ```

!!! note "説明"
    公式インストーラーは [https://git-scm.com/downloads](https://git-scm.com/downloads) からもダウンロードできます。

#### 使用例

```bash
# インストール後にバージョン確認
git --version
# 例: git version 2.44.0
```

---

### アップデート

=== "macOS"

    ```bash
    brew upgrade git
    ```

=== "Ubuntu / Debian"

    ```bash
    sudo apt update && sudo apt upgrade git
    ```

=== "Git for Windows"

    ```bash
    # Git 2.16.1 以降は git 自身でアップデート可能
    git update-git-for-windows
    ```

!!! note "説明"
    最新バージョンを維持することでセキュリティパッチや新機能が使えます。

#### 使用例

```bash
# アップデート後にバージョン確認
git --version
```

---

### アンインストール

=== "macOS"

    ```bash
    brew uninstall git
    ```

=== "Ubuntu / Debian"

    ```bash
    sudo apt remove git
    sudo apt autoremove
    ```

!!! note "説明"
    アンインストールしてもローカルリポジトリ（`.git` フォルダ）は削除されません。

---

## 初期設定

### ユーザー情報を設定する

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

!!! note "説明"
    コミット履歴に記録される名前とメールアドレスを設定します。
    `--global` はすべてのリポジトリに適用され、`--local` は現在のリポジトリだけに適用されます。

#### 使用例

```bash
# 名前とメールアドレスを設定
git config --global user.name "Taro Yamada"
git config --global user.email "taro@example.com"

# デフォルトブランチ名を main に設定（推奨）
git config --global init.defaultBranch main

# デフォルトエディタを vim に設定
git config --global core.editor vim

# 設定内容を確認
git config --list

# 特定の設定値だけ確認
git config user.name
```

---

### SSH キーを生成・登録する

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

!!! note "説明"
    GitHub にパスワードなしで接続するための SSH キーを生成します。
    生成後、`~/.ssh/id_ed25519.pub` の内容を GitHub の Settings → SSH keys に登録してください。

#### 使用例

```bash
# SSH キー生成（Enter で保存先・パスフレーズをスキップ可能）
ssh-keygen -t ed25519 -C "you@example.com"

# 公開鍵の内容を表示（GitHub に貼り付ける）
cat ~/.ssh/id_ed25519.pub

# 接続テスト
ssh -T git@github.com
```

---

## 基本コマンド

### リポジトリを初期化する

```bash
git init
```

!!! note "説明"
    カレントディレクトリに `.git` フォルダを作成し、Git 管理を開始します。

#### 使用例

```bash
# 新しいプロジェクトを初期化
mkdir my-project && cd my-project
git init

# 特定ディレクトリを指定して初期化
git init my-project
```

---

### リポジトリをクローンする

```bash
git clone <URL>
```

!!! note "説明"
    リモートリポジトリをローカルにコピーします。HTTPS と SSH の両方に対応しています。

#### 使用例

```bash
# HTTPS でクローン
git clone https://github.com/user/repo.git

# SSH でクローン
git clone git@github.com:user/repo.git

# フォルダ名を指定してクローン
git clone https://github.com/user/repo.git my-folder

# 特定ブランチだけクローン
git clone -b develop https://github.com/user/repo.git

# 直近 1 コミット分だけクローン（浅いクローン）
git clone --depth 1 https://github.com/user/repo.git
```

---

### ファイルをステージに追加する

```bash
git add <ファイル名>
```

!!! note "説明"
    変更したファイルをコミット対象（ステージングエリア）に追加します。

#### 使用例

```bash
# 特定ファイルを追加
git add index.html

# 複数ファイルを追加
git add index.html style.css

# カレントディレクトリ以下すべてを追加
git add .

# 拡張子で絞り込んで追加
git add "*.md"

# 変更のある追跡済みファイルをすべて追加（新規ファイルは除く）
git add -u
```

---

### コミットする

```bash
git commit -m "コミットメッセージ"
```

!!! note "説明"
    ステージに追加したファイルの変更をリポジトリに記録します。メッセージは変更内容を簡潔に表す文章にしましょう。

#### 使用例

```bash
# 1 行メッセージでコミット
git commit -m "feat: ログイン機能を追加"

# add と commit を同時に実行（追跡済みファイルのみ）
git commit -am "fix: バグを修正"

# エディタを開いて詳細なメッセージを書く
git commit

# 直前のコミットメッセージを修正
git commit --amend -m "feat: ログイン機能を追加（修正）"
```

---

### 状態を確認する

```bash
git status
```

!!! note "説明"
    ステージ済み・未追跡・変更済みのファイル一覧を確認します。

#### 使用例

```bash
# 状態確認
git status

# 短縮形式で表示
git status -s
```

---

### 変更差分を確認する

```bash
git diff
```

!!! note "説明"
    ワーキングツリーとステージの差分を表示します。

#### 使用例

```bash
# ステージ前の変更を確認
git diff

# ステージ済みの変更を確認
git diff --staged

# 特定ファイルの差分を確認
git diff index.html

# 2 つのブランチ間の差分
git diff main..develop

# ファイル名だけ一覧表示
git diff --name-only main..develop
```

---

### コミット履歴を確認する

```bash
git log
```

!!! note "説明"
    コミット履歴を新しい順に表示します。

#### 使用例

```bash
# 標準ログ表示
git log

# 1 行でコンパクトに表示
git log --oneline

# グラフ付きで表示
git log --oneline --graph --all

# 件数を絞り込む
git log -5

# 特定ファイルの変更履歴
git log --oneline -- index.html

# 検索ワードを含むコミットを絞り込む
git log --grep="fix"

# 日付範囲で絞り込む
git log --after="2024-01-01" --before="2024-12-31"
```

---

## リモートリポジトリ操作

### リモートを追加・確認する

```bash
git remote add <名前> <URL>
```

!!! note "説明"
    リモートリポジトリに名前をつけて登録します。通常は `origin` という名前を使います。

#### 使用例

```bash
# リモートを追加
git remote add origin https://github.com/user/repo.git

# リモート一覧を確認
git remote -v

# リモート URL を変更
git remote set-url origin git@github.com:user/repo.git

# リモートを削除
git remote remove origin
```

---

### プッシュする

```bash
git push <リモート名> <ブランチ名>
```

!!! note "説明"
    ローカルのコミットをリモートリポジトリへ送信します。

#### 使用例

```bash
# main ブランチをプッシュ
git push origin main

# 初回プッシュで上流ブランチを設定しつつプッシュ
git push -u origin main

# 強制プッシュ（注意！）
git push --force origin main

# 安全な強制プッシュ（他のコミットを上書きしない）
git push --force-with-lease origin main

# タグをプッシュ
git push origin --tags
```

---

### プルする

```bash
git pull
```

!!! note "説明"
    リモートの変更をローカルに取得してマージします。`fetch + merge` の組み合わせです。

#### 使用例

```bash
# リモートの変更をプル
git pull

# リベースでプル（履歴がきれいになる）
git pull --rebase

# 特定リモート・ブランチをプル
git pull origin develop
```

---

### フェッチする

```bash
git fetch
```

!!! note "説明"
    リモートの変更を取得しますが、ローカルのワーキングツリーには反映しません。確認してからマージしたいときに使います。

#### 使用例

```bash
# すべてのリモートをフェッチ
git fetch --all

# 特定リモートをフェッチ
git fetch origin

# 削除されたリモートブランチをローカルにも反映
git fetch --prune
```

---

## ブランチ操作

### ブランチを作成・切り替える

```bash
git branch <ブランチ名>
git switch <ブランチ名>
```

!!! note "説明"
    `branch` でブランチを作成し、`switch` で切り替えます。`checkout` でも同様の操作ができます（旧来の方法）。

#### 使用例

```bash
# ブランチを作成
git branch feature/login

# ブランチを切り替え
git switch feature/login

# 作成と切り替えを同時に実行
git switch -c feature/login

# 旧来の方法（checkout）
git checkout -b feature/login

# ブランチ一覧を表示（ローカル）
git branch

# ブランチ一覧を表示（リモート含む）
git branch -a

# ブランチを削除（マージ済み）
git branch -d feature/login

# ブランチを強制削除
git branch -D feature/login

# ブランチ名を変更
git branch -m old-name new-name
```

---

### マージする

```bash
git merge <ブランチ名>
```

!!! note "説明"
    指定したブランチの変更を現在のブランチに統合します。

#### 使用例

```bash
# feature ブランチを main にマージ
git switch main
git merge feature/login

# マージコミットを必ず作成する（fast-forward しない）
git merge --no-ff feature/login

# コンフリクトなどでマージをやめる
git merge --abort
```

---

### リベースする

```bash
git rebase <ブランチ名>
```

!!! note "説明"
    コミット履歴を整理して、ブランチの起点を別のコミットに移動させます。チーム開発では共有ブランチへのリベースに注意が必要です。

#### 使用例

```bash
# main を起点にリベース
git switch feature/login
git rebase main

# 対話的リベース（直近 3 コミットを整理）
git rebase -i HEAD~3

# リベース中にコンフリクトを解消したあと続行
git rebase --continue

# リベースをやめる
git rebase --abort
```

---

## 変更の一時保存・取り消し

### 変更を一時退避する（スタッシュ）

```bash
git stash
```

!!! note "説明"
    コミットせずに作業中の変更を一時的に退避します。ブランチを切り替えるときなどに便利です。

#### 使用例

```bash
# 変更を退避
git stash

# コメント付きで退避
git stash push -m "ログイン画面の途中作業"

# 未追跡ファイルも含めて退避
git stash -u

# スタッシュ一覧を確認
git stash list

# 最新のスタッシュを復元（退避から削除）
git stash pop

# 最新のスタッシュを適用（退避に残す）
git stash apply

# 特定のスタッシュを適用
git stash apply stash@{2}

# スタッシュを削除
git stash drop stash@{0}

# スタッシュをすべて削除
git stash clear
```

---

### ファイルの変更を取り消す

```bash
git restore <ファイル名>
```

!!! note "説明"
    ワーキングツリーの変更を取り消してコミット済みの状態に戻します。

#### 使用例

```bash
# 特定ファイルの変更を取り消し（ステージ前）
git restore index.html

# ステージを取り消す（ワーキングツリーは維持）
git restore --staged index.html

# すべての変更を取り消し
git restore .
```

---

### コミットを取り消す

```bash
git revert <コミットハッシュ>
git reset <オプション> <コミットハッシュ>
```

!!! note "説明"
    `revert` は取り消しコミットを追加する安全な方法です。`reset` は履歴を書き換えるため、共有ブランチでは注意が必要です。

#### 使用例

```bash
# 特定コミットを打ち消すコミットを作成（安全）
git revert abc1234

# 直前のコミットを取り消し（ファイルの変更は維持）
git reset --soft HEAD~1

# 直前のコミットを取り消し（ステージも戻す）
git reset --mixed HEAD~1

# 直前のコミットを取り消し（変更もすべて破棄・危険）
git reset --hard HEAD~1
```

---

## タグ操作

### タグを作成・プッシュする

```bash
git tag <タグ名>
```

!!! note "説明"
    リリースバージョンなどの節目にタグを付けます。注釈付きタグ（annotated tag）が推奨です。

#### 使用例

```bash
# 軽量タグを作成
git tag v1.0.0

# 注釈付きタグを作成（推奨）
git tag -a v1.0.0 -m "バージョン 1.0.0 リリース"

# 特定コミットにタグを付ける
git tag -a v0.9.0 abc1234 -m "ベータ版"

# タグ一覧を表示
git tag

# タグをリモートにプッシュ
git push origin v1.0.0

# すべてのタグをプッシュ
git push origin --tags

# タグを削除（ローカル）
git tag -d v1.0.0

# タグを削除（リモート）
git push origin --delete v1.0.0
```

---

## 上級者向けコマンド

### チェリーピック

```bash
git cherry-pick <コミットハッシュ>
```

!!! note "説明"
    別ブランチの特定コミットだけを現在のブランチに取り込みます。

#### 使用例

```bash
# 特定コミットを取り込む
git cherry-pick abc1234

# 複数コミットを取り込む
git cherry-pick abc1234 def5678

# コミットせずに変更だけ取り込む
git cherry-pick -n abc1234

# コンフリクトを解消したあと続行
git cherry-pick --continue

# やめる
git cherry-pick --abort
```

---

### バイセクト（原因コミットを二分探索する）

```bash
git bisect start
```

!!! note "説明"
    バグが混入したコミットを二分探索で特定します。大量のコミット履歴から原因を素早く見つけるのに役立ちます。

#### 使用例

```bash
# 二分探索を開始
git bisect start

# 現在のコミットがバグあり
git bisect bad

# 問題のなかった古いコミットを指定
git bisect good v1.0.0

# テストして良否を回答しながら探索
git bisect good   # このコミットは正常
git bisect bad    # このコミットはバグあり

# 探索終了
git bisect reset
```

---

### サブモジュール

```bash
git submodule add <URL>
```

!!! note "説明"
    別の Git リポジトリをサブディレクトリとして埋め込みます。ライブラリやテーマの管理に使います。

#### 使用例

```bash
# サブモジュールを追加
git submodule add https://github.com/user/lib.git libs/lib

# サブモジュールを含めてクローン
git clone --recurse-submodules https://github.com/user/repo.git

# 既存リポジトリのサブモジュールを初期化・取得
git submodule update --init --recursive

# サブモジュールを最新に更新
git submodule update --remote

# サブモジュールを削除
git submodule deinit libs/lib
git rm libs/lib
```

---

### ワークツリーを追加する

```bash
git worktree add <パス> <ブランチ名>
```

!!! note "説明"
    1 つのリポジトリから複数のワーキングツリーを同時に展開します。ブランチを切り替えずに別ブランチの作業ができます。

#### 使用例

```bash
# 別ディレクトリに hotfix ブランチを展開
git worktree add ../hotfix hotfix/urgent

# ワークツリー一覧を確認
git worktree list

# ワークツリーを削除
git worktree remove ../hotfix
```

---

### スパースチェックアウト

```bash
git sparse-checkout init
```

!!! note "説明"
    巨大なリポジトリの一部ディレクトリだけをチェックアウトします。モノレポ環境で有効です。

#### 使用例

```bash
# スパースチェックアウトを有効化
git sparse-checkout init --cone

# 取得するディレクトリを指定
git sparse-checkout set packages/my-app

# 現在の設定を確認
git sparse-checkout list

# 無効化してすべて取得
git sparse-checkout disable
```

---

### フィルターブランチ・フィルターリポ

```bash
git filter-repo --path <パス>
```

!!! note "説明"
    コミット履歴からファイルを完全に削除したり、リポジトリを分割したりします。`git-filter-repo` のインストールが必要です（`pip install git-filter-repo`）。

#### 使用例

```bash
# パスワードが含まれたファイルを履歴から完全削除
git filter-repo --path secrets.txt --invert-paths

# 特定ディレクトリだけを残して履歴を書き換え
git filter-repo --path src/
```

---

## 知ってると便利なコマンド

### 特定ファイルをなかったことにする（.gitignore 追加後）

```bash
git rm --cached <ファイル名>
```

!!! note "説明"
    すでに追跡されているファイルを Git の管理から外しますが、ローカルファイルは削除しません。`.gitignore` に追加したあとに使います。

#### 使用例

```bash
# 特定ファイルをキャッシュから削除
git rm --cached .env

# ディレクトリごとキャッシュから削除
git rm --cached -r node_modules/
```

---

### 変更したファイルの行単位で犯人を探す（blame）

```bash
git blame <ファイル名>
```

!!! note "説明"
    ファイルの各行が最後に変更されたコミットと作者を表示します。

#### 使用例

```bash
# ファイル全体を確認
git blame index.html

# 特定行範囲を確認（10〜20 行目）
git blame -L 10,20 index.html

# コミットハッシュを短縮して表示
git blame --abbrev=7 index.html
```

---

### 特定の文字列をリポジトリ全体から検索する

```bash
git grep <検索ワード>
```

!!! note "説明"
    ワーキングツリー全体からパターンに一致する行を高速に検索します。

#### 使用例

```bash
# キーワードを検索
git grep "TODO"

# 行番号付きで検索
git grep -n "console.log"

# 特定ブランチを検索
git grep "api_key" develop

# 大文字小文字を区別しない
git grep -i "error"
```

---

### reflog（操作履歴を復元する）

```bash
git reflog
```

!!! note "説明"
    `reset --hard` や `rebase` で消えたように見えるコミットも reflog から復元できます。Git で「やり直し」ができる最後の砦です。

#### 使用例

```bash
# reflog を確認
git reflog

# 特定の操作前の状態に戻す
git reset --hard HEAD@{3}

# 削除したブランチを復元
git checkout -b recovered-branch abc1234
```

---

### エイリアスを設定する

```bash
git config --global alias.<エイリアス名> "<コマンド>"
```

!!! note "説明"
    長いコマンドに短い別名を付けて作業を効率化します。

#### 使用例

```bash
# よく使うエイリアスを登録
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.undo "reset --soft HEAD~1"

# 使い方
git st
git lg
```

---

### バンドルファイルを作成する

```bash
git bundle create <ファイル名> --all
```

!!! note "説明"
    リポジトリ全体を 1 つのファイルにまとめてオフライン転送できます。

#### 使用例

```bash
# バンドルを作成
git bundle create repo.bundle --all

# バンドルからクローン
git clone repo.bundle my-repo

# バンドルの内容を確認
git bundle list-heads repo.bundle
```

---

### archive（スナップショットをエクスポートする）

```bash
git archive --format=zip HEAD -o <出力ファイル名>
```

!!! note "説明"
    Git 管理外のファイルや `.git` ディレクトリを含まず、特定コミット時点のスナップショットを zip/tar で出力します。

#### 使用例

```bash
# HEAD の状態を zip でエクスポート
git archive --format=zip HEAD -o snapshot.zip

# 特定タグを tar.gz でエクスポート
git archive --format=tar.gz v1.0.0 -o release-v1.0.0.tar.gz

# 特定ディレクトリだけエクスポート
git archive HEAD src/ -o src-only.zip
```

---

## オプション一覧

### git log の主なオプション

| オプション | 説明 |
| --- | --- |
| `--oneline` | 1 行で簡潔に表示 |
| `--graph` | ブランチをグラフで表示 |
| `--all` | すべてのブランチを表示 |
| `-n <数>` | 表示件数を制限 |
| `--author=<名前>` | 特定の作者で絞り込む |
| `--grep=<文字列>` | コミットメッセージで絞り込む |
| `--since=<日付>` | 指定日以降を表示 |
| `--until=<日付>` | 指定日以前を表示 |
| `--stat` | 変更ファイルの統計を表示 |
| `-p` | 差分も一緒に表示 |
| `--follow <ファイル>` | ファイル名変更をたどる |

---

### git diff の主なオプション

| オプション | 説明 |
| --- | --- |
| `--staged` | ステージ済みの差分を表示 |
| `--name-only` | ファイル名だけを表示 |
| `--stat` | 統計サマリーを表示 |
| `--word-diff` | 単語単位で差分を表示 |
| `--ignore-space-change` | 空白の変化を無視 |

---

### git push の主なオプション

| オプション | 説明 |
| --- | --- |
| `-u` / `--set-upstream` | 上流ブランチを設定してプッシュ |
| `--force` | 強制プッシュ（危険） |
| `--force-with-lease` | 安全な強制プッシュ |
| `--tags` | すべてのタグをプッシュ |
| `--delete <ブランチ>` | リモートブランチを削除 |
| `--dry-run` | 実際には送信せず確認のみ |

---

### git reset の主なオプション

| オプション | 説明 |
| --- | --- |
| `--soft` | コミットを取り消す（ステージ・変更を維持） |
| `--mixed` | コミット・ステージを取り消す（変更を維持） |
| `--hard` | コミット・ステージ・変更をすべて破棄（危険） |

---

## 設定

### .gitignore の書き方

```gitignore
# OS 関連
.DS_Store
Thumbs.db

# エディタ関連
.vscode/
.idea/
*.swp

# 依存ファイル
node_modules/
__pycache__/
*.pyc

# 環境変数・機密情報
.env
.env.local
*.pem

# ビルド成果物
dist/
build/
*.log
```

!!! note "説明"
    `.gitignore` はプロジェクトルートに置きます。
    `#` はコメント、`*` はワイルドカード、`/` 末尾はディレクトリを示します。
    テンプレートは [https://github.com/github/gitignore](https://github.com/github/gitignore) を参照してください。

---

### .gitconfig の設定例

```ini
[user]
    name = Taro Yamada
    email = taro@example.com

[core]
    editor = vim
    autocrlf = input

[init]
    defaultBranch = main

[pull]
    rebase = false

[alias]
    st = status
    co = checkout
    br = branch
    lg = log --oneline --graph --all
    undo = reset --soft HEAD~1

[color]
    ui = auto
```

!!! note "説明"
    `~/.gitconfig` がグローバル設定ファイルです。直接編集するか `git config --global` コマンドで設定できます。

---

### グローバル .gitignore を設定する

```bash
git config --global core.excludesfile ~/.gitignore_global
```

!!! note "説明"
    すべてのリポジトリで無視したいファイルをグローバルに設定します。OS 固有のファイル（`.DS_Store` など）をここにまとめると便利です。

#### 使用例

```bash
# グローバル gitignore を設定
git config --global core.excludesfile ~/.gitignore_global

# ~/.gitignore_global を作成して内容を記入
vim ~/.gitignore_global
```

---

## 頻出コマンド早引き

```bash
# リポジトリの初期化
git init

# クローン
git clone https://github.com/user/repo.git

# ステージに追加（全ファイル）
git add .

# コミット
git commit -m "メッセージ"

# 状態確認
git status

# ログ（グラフ付き）
git log --oneline --graph --all

# プッシュ（初回）
git push -u origin main

# プル
git pull

# ブランチ作成と切り替え
git switch -c feature/my-feature

# マージ
git merge feature/my-feature

# スタッシュ（退避）
git stash

# スタッシュを復元
git stash pop

# 直前のコミットを取り消し（変更は残す）
git reset --soft HEAD~1

# タグを作成してプッシュ
git tag -a v1.0.0 -m "リリース"
git push origin --tags

# リモートブランチを削除
git push origin --delete feature/old-branch

# reflog で操作履歴を確認
git reflog
```

---

!!! info "公式ドキュメント"
    - [Git 公式ドキュメント](https://git-scm.com/doc)
    - [GitHub ドキュメント](https://docs.github.com/ja)
    - [Pro Git（日本語版）](https://git-scm.com/book/ja/v2)
    - [GitHub gitignore テンプレート集](https://github.com/github/gitignore)
