# WSL (Ubuntu)

WSL（Windows Subsystem for Linux）上の Ubuntu で使用するコマンドをまとめたリファレンスです。

---

## インストール・アップデート・アンインストール

### WSL をインストールする

=== "PowerShell（Windows 側）"

    ```powershell
    # デフォルト（Ubuntu）をインストール
    wsl --install

    # ディストリビューションを指定してインストール
    wsl --install -d Ubuntu-22.04

    # インストール可能な一覧を表示
    wsl --list --online
    ```

=== "APT（Ubuntu 内）"

    ```bash
    # パッケージリストを更新してからインストール
    sudo apt update
    sudo apt install パッケージ名
    ```

!!! note "説明"
    PowerShell から `wsl --install` を実行すると WSL 本体と Ubuntu が一括でインストールされます。
    Ubuntu 内のパッケージ管理には `apt` コマンドを使います。

#### 使用例

=== "PowerShell"

    ```powershell
    # Ubuntu 24.04 を指定してインストール
    wsl --install -d Ubuntu-24.04
    ```

=== "APT"

    ```bash
    # git と curl をまとめてインストール
    sudo apt update
    sudo apt install git curl -y
    ```

---

### WSL をアップデートする

```powershell
# WSL 本体をアップデート
wsl --update

# バージョンを確認
wsl --version

# インストール済みディストリビューション一覧を確認
wsl --list --verbose
```

!!! note "説明"
    `wsl --update` は PowerShell から実行します。Ubuntu 内のパッケージアップデートは `sudo apt upgrade` を使います。

#### 使用例

```powershell
# バージョン確認後にアップデート
wsl --version
wsl --update
```

---

### APT でパッケージをアップデートする

```bash
# パッケージリストを更新
sudo apt update

# インストール済みパッケージをアップグレード
sudo apt upgrade

# 更新と適用を一括実行
sudo apt update && sudo apt upgrade -y

# 不要な依存パッケージを削除
sudo apt autoremove

# ダウンロードキャッシュを削除
sudo apt clean
```

!!! note "説明"
    `apt update` はパッケージリストの更新のみで、実際のアップグレードは行いません。
    `apt upgrade` と組み合わせて使います。

#### 使用例

```bash
# 定期メンテナンスの一括コマンド
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
```

---

### APT でパッケージをアンインストールする

```bash
# パッケージを削除（設定ファイルは残す）
sudo apt remove パッケージ名

# パッケージと設定ファイルを完全削除
sudo apt purge パッケージ名

# 不要な依存パッケージも削除
sudo apt purge パッケージ名 && sudo apt autoremove
```

!!! note "説明"
    `remove` は設定ファイルを残します。完全に削除したい場合は `purge` を使います。

#### 使用例

```bash
# vim を設定ファイルごと完全削除
sudo apt purge vim
sudo apt autoremove
```

---

### WSL をアンインストールする

```powershell
# 削除前にバックアップ（tar ファイルとして書き出す）
wsl --export Ubuntu D:\backup\ubuntu.tar

# ディストリビューションを登録解除（削除）
wsl --unregister Ubuntu

# バックアップから復元
wsl --import Ubuntu C:\WSL\Ubuntu D:\backup\ubuntu.tar
```

!!! note "説明"
    `--unregister` を実行するとディストリビューション内のデータがすべて削除されます。
    事前に `--export` でバックアップを取っておくことを推奨します。

#### 使用例

```powershell
# バックアップを取ってから Ubuntu を削除
wsl --export Ubuntu D:\backup\ubuntu_$(Get-Date -Format yyyyMMdd).tar
wsl --unregister Ubuntu
```

---

## ファイル・ディレクトリ操作

### カレントディレクトリを表示する

```bash
pwd
```

!!! note "説明"
    Print Working Directory の略。現在いるディレクトリの絶対パスを表示します。

#### 使用例

```bash
# 移動後に現在地を確認
cd ~/projects
pwd
```

---

### ディレクトリを移動する

```bash
# 指定ディレクトリへ移動
cd ディレクトリ名

# ホームディレクトリへ移動
cd ~
cd

# 1つ上のディレクトリへ移動
cd ..

# 直前のディレクトリへ戻る
cd -
```

!!! note "説明"
    `cd -` は直前にいたディレクトリへ戻るショートカットです。2つのディレクトリを行き来するときに便利です。

#### 使用例

```bash
# プロジェクトと設定ファイルを行き来する
cd ~/projects/myapp
cd ~/.config
cd -   # ~/projects/myapp に戻る
```

---

### ファイル・ディレクトリを一覧表示する

```bash
# 一覧表示
ls

# 詳細表示（権限・サイズ・日時）
ls -l

# 隠しファイルを含む全ファイルを詳細表示
ls -la

# サイズを人間が読みやすい形式で表示
ls -lh

# 更新日時順（新しい順）に並べ替え
ls -lt

# 再帰的に表示
ls -lR

# ツリー形式で表示（要 tree）
tree

# 深さを指定してツリー表示
tree -L 2
```

!!! note "説明"
    `ls -la` は最もよく使われる組み合わせです。`tree` は別途 `sudo apt install tree` でインストールが必要です。

#### 使用例

```bash
# ホームディレクトリの全ファイルをサイズ付きで表示
ls -lah ~

# 2階層のツリーで構成を確認
tree -L 2 ~/projects
```

---

### ディレクトリを作成する

```bash
# ディレクトリを作成
mkdir ディレクトリ名

# 深い階層を一括作成
mkdir -p 親/子/孫

# 作成と同時に権限を指定
mkdir -m 755 ディレクトリ名
```

!!! note "説明"
    `-p` オプションを使うと中間ディレクトリが存在しない場合でも一括して作成します。
    すでに存在しても エラーになりません。

#### 使用例

```bash
# プロジェクトのディレクトリ構成を一括作成
mkdir -p ~/projects/myapp/{src,tests,docs}
ls ~/projects/myapp/
```

---

### ファイルを作成する

```bash
# 空ファイルを作成（すでに存在する場合はタイムスタンプを更新）
touch ファイル名

# ファイルに文字列を書き込んで作成（上書き）
echo "内容" > ファイル名

# ファイルに追記
echo "追記内容" >> ファイル名

# 複数行をファイルに書き込む（ヒアドキュメント）
cat << EOF > ファイル名
1行目
2行目
EOF
```

!!! note "説明"
    `>` は上書き、`>>` は追記です。間違えると既存ファイルの内容が消えるため注意してください。

#### 使用例

```bash
# README.md を作成して内容を書き込む
echo "# My Project" > README.md
echo "" >> README.md
echo "プロジェクトの説明" >> README.md
```

---

### ファイル・ディレクトリをコピーする

```bash
# ファイルをコピー
cp コピー元 コピー先

# ディレクトリを中身ごとコピー
cp -r コピー元ディレクトリ コピー先

# タイムスタンプ・権限を保持してコピー
cp -p コピー元 コピー先

# 更新されたファイルのみコピー
cp -u コピー元 コピー先

# コピー内容を表示しながらコピー
cp -v コピー元 コピー先
```

!!! note "説明"
    ディレクトリをコピーする場合は必ず `-r`（再帰的）オプションが必要です。
    `-a` オプションは `-r -p` の機能を兼ねており、シンボリックリンクも保持します。

#### 使用例

```bash
# 設定ファイルをバックアップしてから編集
cp ~/.bashrc ~/.bashrc.bak
vim ~/.bashrc

# ディレクトリを丸ごとバックアップ
cp -r ~/projects/myapp ~/projects/myapp_backup
```

---

### ファイル・ディレクトリを移動・リネームする

```bash
# ファイルを移動
mv 移動元 移動先ディレクトリ/

# ファイルをリネーム
mv 古いファイル名 新しいファイル名

# ディレクトリを移動・リネーム
mv 移動元ディレクトリ 移動先

# 上書き前に確認
mv -i 移動元 移動先
```

!!! note "説明"
    `mv` は移動とリネームどちらにも使います。移動先に同名ファイルが存在する場合、デフォルトで上書きされます。

#### 使用例

```bash
# ファイルをリネーム
mv old_name.py new_name.py

# 複数ファイルをまとめてディレクトリへ移動
mv *.log ~/logs/
```

---

### ファイル・ディレクトリを削除する

```bash
# ファイルを削除
rm ファイル名

# 複数ファイルを削除
rm ファイル1 ファイル2

# ディレクトリを中身ごと削除
rm -r ディレクトリ名

# 確認なしで強制削除（注意して使う）
rm -rf ディレクトリ名

# 空のディレクトリのみ削除
rmdir ディレクトリ名
```

!!! note "説明"
    `rm -rf` は取り消しができないため、特にパスの指定ミスに注意してください。
    削除前に `ls` でパスを確認する習慣をつけることを推奨します。

#### 使用例

```bash
# キャッシュディレクトリを削除
ls -la ~/.cache/myapp   # 削除前に確認
rm -rf ~/.cache/myapp

# .pyc ファイルをすべて削除
find . -name "*.pyc" -exec rm {} \;
```

---

### ファイルの内容を表示する

```bash
# ファイル全体を表示
cat ファイル名

# 行番号付きで表示
cat -n ファイル名

# 先頭 N 行を表示（デフォルト10行）
head ファイル名
head -n 20 ファイル名

# 末尾 N 行を表示（デフォルト10行）
tail ファイル名
tail -n 20 ファイル名

# リアルタイムで末尾を追跡表示
tail -f ファイル名

# ページごとに閲覧（q で終了）
less ファイル名

# 行数・単語数・バイト数を表示
wc ファイル名
wc -l ファイル名   # 行数のみ
```

!!! note "説明"
    `tail -f` はログファイルのリアルタイム監視によく使います。
    大きなファイルを全体表示する場合は `less` を使うとスクロールして読めます。

#### 使用例

```bash
# アプリログをリアルタイム監視
tail -f /var/log/syslog

# 設定ファイルの行数を確認
wc -l /etc/nginx/nginx.conf
```

---

### パーミッションを変更する

```bash
# 数値でパーミッションを設定
chmod 755 ファイル名

# 記号でパーミッションを設定
chmod u+x ファイル名   # 所有者に実行権限を付与
chmod g-w ファイル名   # グループの書き込み権限を削除
chmod o+r ファイル名   # その他ユーザーに読み取り権限を付与
chmod a+x ファイル名   # 全ユーザーに実行権限を付与

# ディレクトリ以下すべてを変更
chmod -R 755 ディレクトリ名

# 所有者を変更
chown ユーザー名 ファイル名

# 所有者とグループを変更
chown ユーザー名:グループ名 ファイル名

# 再帰的に変更
chown -R ユーザー名 ディレクトリ名
```

!!! note "説明"
    シェルスクリプトを作成したら `chmod +x スクリプト名` で実行権限を付与します。
    SSH 秘密鍵は `chmod 600` に設定しないと接続が拒否されます。

#### 使用例

```bash
# シェルスクリプトに実行権限を付与
chmod +x start.sh
./start.sh

# SSH 秘密鍵のパーミッションを修正
chmod 600 ~/.ssh/id_ed25519

# Web 公開ディレクトリの所有者を変更
sudo chown -R www-data:www-data /var/www/html
```

---

## テキスト検索・処理

### ファイル内を検索する（grep）

```bash
# ファイル内でパターンを検索
grep "検索文字列" ファイル名

# 大文字小文字を区別しない
grep -i "検索文字列" ファイル名

# 行番号を表示
grep -n "検索文字列" ファイル名

# マッチしない行を表示
grep -v "除外文字列" ファイル名

# ディレクトリを再帰的に検索
grep -rn "検索文字列" ディレクトリ名

# 正規表現で検索
grep -E "パターン1|パターン2" ファイル名

# マッチ行の前後 N 行も表示
grep -C 3 "検索文字列" ファイル名

# マッチしたファイル名のみ表示
grep -rl "検索文字列" .

# マッチした件数を表示
grep -c "検索文字列" ファイル名
```

!!! note "説明"
    `-rn` の組み合わせはディレクトリ全体を行番号付きで検索する最頻出パターンです。
    `-E` オプションで `|`（OR）や `+`（1回以上）などの拡張正規表現が使えます。

#### 使用例

```bash
# Python ファイルの関数定義を行番号付きで検索
grep -n "def " app.py

# src/ 以下の "TODO" コメントを全ファイルから抽出
grep -rn "TODO" ./src/

# エラーログからエラー行のみ抽出して件数を確認
grep -c "ERROR" app.log
grep -n "ERROR" app.log | tail -20
```

---

### ファイル・ディレクトリを検索する（find）

```bash
# ファイル名で検索
find 検索パス -name "ファイル名"

# 拡張子で検索
find . -name "*.py"

# ファイルのみ検索
find . -type f -name "*.txt"

# ディレクトリのみ検索
find . -type d -name "config"

# N 日以内に更新されたファイルを検索
find . -mtime -7

# N MB 以上のファイルを検索
find . -size +10M

# 空ファイルを検索
find . -empty

# 検索結果にコマンドを実行
find . -name "*.log" -exec rm {} \;

# 検索結果を xargs に渡す
find . -name "*.py" | xargs grep "import"

# 最大検索深度を指定
find . -maxdepth 2 -name "*.json"
```

!!! note "説明"
    `-exec` はマッチしたファイルごとにコマンドを実行します。末尾の `\;` は必須です。
    大量のファイルを処理する場合は `xargs` の方が高速です。

#### 使用例

```bash
# .pyc ファイルをすべて検索して削除
find . -name "*.pyc" -exec rm {} \;

# 7日以内に更新されたログファイルを一覧表示
find /var/log -name "*.log" -mtime -7 -ls

# 10MB 以上のファイルをサイズ順に表示
find ~ -size +10M -type f | xargs ls -lh | sort -k5 -rh
```

---

### テキストを置換する（sed）

```bash
# 文字列を置換（最初にマッチした箇所のみ）
sed 's/置換前/置換後/' ファイル名

# 全箇所を置換
sed 's/置換前/置換後/g' ファイル名

# ファイルを直接書き換え
sed -i 's/置換前/置換後/g' ファイル名

# 特定行を削除
sed '3d' ファイル名

# 範囲行を削除
sed '2,5d' ファイル名

# 特定行のみ表示
sed -n '5,10p' ファイル名

# 空行を削除
sed '/^$/d' ファイル名

# 行頭に文字を追加
sed 's/^/  /' ファイル名
```

!!! note "説明"
    `-i` オプションはファイルを直接書き換えます。バックアップを作成するには `-i.bak` とすると
    `.bak` 拡張子で元ファイルが保存されます。

#### 使用例

```bash
# 設定ファイルのホスト名を一括置換（バックアップ付き）
sed -i.bak 's/localhost/192.168.1.10/g' config.yaml

# 空行とコメント行を除いて表示
sed '/^$/d; /^#/d' /etc/nginx/nginx.conf
```

---

### フィールドを抽出・集計する（awk）

```bash
# 空白区切りで特定フィールドを抽出
awk '{print $1}' ファイル名

# 区切り文字を指定してフィールドを抽出
awk -F':' '{print $1}' /etc/passwd

# 条件付き出力
awk '$3 > 1000 {print $1, $3}' ファイル名

# 行番号を付けて出力
awk '{print NR, $0}' ファイル名

# 合計を計算
awk '{sum += $1} END {print sum}' ファイル名

# 列数を表示
awk '{print NF}' ファイル名 | sort -u

# 特定フィールドを結合して出力
awk -F':' '{print $1 "@" $3}' /etc/passwd
```

!!! note "説明"
    `$1` `$2` はフィールド番号（1始まり）、`$0` は行全体、`NR` は行番号、`NF` はフィールド数を表します。
    `-F` で区切り文字を指定します（デフォルトは空白）。

#### 使用例

```bash
# /etc/passwd からユーザー名一覧を取得
awk -F':' '{print $1}' /etc/passwd

# アクセスログの IP アドレス別アクセス数ランキング上位10件
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10
```

---

### 並べ替え・重複処理をする（sort / uniq）

```bash
# アルファベット順に並べ替え
sort ファイル名

# 数値として並べ替え
sort -n ファイル名

# 逆順に並べ替え
sort -r ファイル名

# 人間が読みやすい数値で並べ替え（K, M, G 対応）
sort -h ファイル名

# 特定フィールドを基準に並べ替え
sort -k2 ファイル名

# 重複行を削除（sort 後に使う）
sort ファイル名 | uniq

# 重複回数を表示
sort ファイル名 | uniq -c

# 重複行のみ表示
sort ファイル名 | uniq -d
```

!!! note "説明"
    `uniq` は連続した重複のみ除去するため、必ず `sort` と組み合わせて使います。
    `uniq -c` でカウントした結果に再度 `sort -rn` をかけるとランキングが作れます。

#### 使用例

```bash
# ログから重複を除いたユニーク IP を取得してカウント
cut -d' ' -f1 access.log | sort | uniq -c | sort -rn | head -10

# ファイルの重複行を削除して上書き保存
sort ファイル名 | uniq > 一時ファイル && mv 一時ファイル ファイル名
```

---

## プロセス管理

### 実行中のプロセスを確認する

```bash
# 現在のシェルのプロセスを表示
ps

# すべてのプロセスを表示
ps aux

# プロセス名で絞り込む
ps aux | grep プロセス名

# リアルタイム監視
top

# より見やすいリアルタイム監視（要インストール）
htop

# プロセス名から PID を取得
pgrep プロセス名

# ツリー形式でプロセスを表示
pstree
```

!!! note "説明"
    `top` は `q` で終了します。`htop` は `sudo apt install htop` でインストールできます。
    `ps aux | grep プロセス名` でプロセスの PID を確認してから `kill` に渡す使い方が一般的です。

#### 使用例

```bash
# Python プロセスの PID を確認して終了する
ps aux | grep python3
pgrep python3

# htop をインストールしてリアルタイム監視
sudo apt install htop -y
htop
```

---

### プロセスを終了する

```bash
# PID を指定してシグナルを送る（デフォルト: SIGTERM 正常終了）
kill PID

# 強制終了（SIGKILL）
kill -9 PID

# プロセス名で終了
pkill プロセス名

# プロセス名で強制終了
pkill -9 プロセス名

# シグナル一覧を確認
kill -l
```

!!! note "説明"
    `kill -9` は強制終了のため、プロセスがデータを保存する機会がありません。
    まず通常の `kill` を試してから、応答がない場合に `-9` を使うのが安全です。

#### 使用例

```bash
# 応答しなくなった Python スクリプトを終了
pgrep -la python3          # PID を確認
kill $(pgrep python3)      # 正常終了を試みる
kill -9 $(pgrep python3)   # 応答がなければ強制終了
```

---

### バックグラウンドでコマンドを実行する

```bash
# バックグラウンドで実行
コマンド &

# ジョブ一覧を表示
jobs

# バックグラウンドジョブをフォアグラウンドに戻す
fg %ジョブ番号

# フォアグラウンドのジョブをバックグラウンドに送る（先に Ctrl+z で一時停止）
bg %ジョブ番号

# ターミナルを閉じてもプロセスを継続する
nohup コマンド &

# screen でセッションを維持する（要インストール）
screen
screen -S セッション名
screen -ls              # セッション一覧
screen -r セッション名  # セッションに再接続
```

!!! note "説明"
    `nohup` はターミナルを閉じてもプロセスを継続させます。出力は `nohup.out` に保存されます。
    長時間のスクリプトには `screen` や `tmux` の使用を推奨します。

#### 使用例

```bash
# 重いスクリプトをバックグラウンドで実行してログに保存
nohup python3 heavy_script.py > output.log 2>&1 &
echo "PID: $!"
tail -f output.log
```

---

## ネットワーク

### 疎通確認をする（ping）

```bash
# 疎通確認（Ctrl+c で停止）
ping ホスト名またはIP

# 送信回数を指定
ping -c 4 ホスト名

# 送信間隔を指定（秒）
ping -i 0.5 ホスト名

# ルートを確認
traceroute ホスト名

# ポートが開いているか確認
nc -zv ホスト名 ポート番号

# 開いているポートを確認
ss -tuln
```

!!! note "説明"
    `ping -c 4` は4回だけ送信して終了します。ネットワーク接続確認の基本コマンドです。
    `ss -tuln` でリッスン中のポートを確認できます（古いシステムでは `netstat -tuln`）。

#### 使用例

```bash
# Google のサーバーに4回 ping を送る
ping -c 4 google.com

# ローカルの 8080 ポートが開いているか確認
nc -zv localhost 8080
```

---

### ファイルをダウンロードする（wget）

```bash
# ファイルをダウンロード
wget URL

# ファイル名を指定してダウンロード
wget -O 保存ファイル名 URL

# バックグラウンドでダウンロード
wget -b URL

# 再試行回数を指定
wget --tries=3 URL

# 再開可能なダウンロード
wget -c URL

# ディレクトリごとダウンロード
wget -r -np URL
```

!!! note "説明"
    `-c` オプションで途中で中断したダウンロードを再開できます。
    大きなファイルのダウンロードに便利です。

#### 使用例

```bash
# ファイルをダウンロードして保存
wget https://example.com/archive.tar.gz

# 途中で中断したダウンロードを再開
wget -c https://example.com/large-file.iso
```

---

### HTTP リクエストを送る（curl）

```bash
# GET リクエスト
curl URL

# レスポンスヘッダーも表示
curl -i URL

# ヘッダーのみ表示
curl -I URL

# ファイルに保存
curl -o ファイル名 URL

# POST リクエスト（フォームデータ）
curl -X POST -d "key=value" URL

# POST リクエスト（JSON）
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' URL

# 認証ヘッダーを付けてリクエスト
curl -H "Authorization: Bearer トークン" URL

# リダイレクトを追う
curl -L URL

# 詳細な通信ログを表示
curl -v URL

# レスポンスを JSON として整形表示
curl -s URL | python3 -m json.tool
```

!!! note "説明"
    API のテストやデバッグに広く使われます。`-s` は進捗表示を抑制するサイレントモードです。
    `-H` でカスタムヘッダーを追加できます。

#### 使用例

```bash
# REST API に GET リクエストを送って JSON を整形表示
curl -s https://api.github.com/users/octocat | python3 -m json.tool

# Bearer 認証付きの POST リクエスト
curl -X POST \
  -H "Authorization: Bearer mytoken" \
  -H "Content-Type: application/json" \
  -d '{"title":"テスト"}' \
  https://api.example.com/items
```

---

### SSH 接続する

```bash
# SSH 接続
ssh ユーザー名@ホスト名またはIP

# ポートを指定して接続
ssh -p ポート番号 ユーザー名@ホスト名

# 鍵ファイルを指定して接続
ssh -i ~/.ssh/秘密鍵ファイル ユーザー名@ホスト名

# リモートでコマンドを実行して終了
ssh ユーザー名@ホスト名 "実行するコマンド"

# ローカルポートフォワーディング
ssh -L ローカルポート:転送先ホスト:転送先ポート ユーザー名@踏み台ホスト

# ファイルをリモートへ転送（scp）
scp ローカルファイル ユーザー名@ホスト名:リモートパス

# リモートからファイルを取得
scp ユーザー名@ホスト名:リモートファイル ローカルパス

# ディレクトリを転送
scp -r ローカルディレクトリ ユーザー名@ホスト名:リモートパス
```

!!! note "説明"
    初回接続時はホストの指紋確認が求められます。`yes` と入力すると `~/.ssh/known_hosts` に登録されます。
    鍵認証を使うとパスワード認証より安全で利便性も高まります。

#### 使用例

```bash
# SSH 鍵を生成してリモートサーバーに登録
ssh-keygen -t ed25519 -C "mypc@work"
ssh-copy-id user@192.168.1.10
ssh user@192.168.1.10   # パスワードなしで接続できることを確認

# ポートフォワーディングでリモートの DB に接続
ssh -L 5432:localhost:5432 user@remote-server
```

---

## アーカイブ・圧縮

### tar.gz を作成・展開する

```bash
# tar.gz を作成
tar -czf アーカイブ名.tar.gz 対象ディレクトリ

# tar.gz を展開
tar -xzf アーカイブ名.tar.gz

# 指定ディレクトリに展開
tar -xzf アーカイブ名.tar.gz -C 展開先ディレクトリ

# アーカイブの内容を確認（展開しない）
tar -tzf アーカイブ名.tar.gz

# bzip2 で圧縮（高圧縮率）
tar -cjf アーカイブ名.tar.bz2 対象ディレクトリ

# xz で圧縮（最高圧縮率）
tar -cJf アーカイブ名.tar.xz 対象ディレクトリ
```

!!! note "説明"
    `-c` で作成、`-x` で展開、`-t` で内容確認です。`-z` が gzip、`-j` が bzip2、`-J` が xz 圧縮に対応します。
    `-v` を追加すると処理中のファイル名が表示されます。

#### 使用例

```bash
# 日付付きバックアップを作成
tar -czf project_$(date +%Y%m%d).tar.gz ./project/

# 展開前に内容を確認してから展開
tar -tzf archive.tar.gz
tar -xzf archive.tar.gz -C ~/tmp/
```

---

### zip を作成・展開する

```bash
# zip を作成
zip -r アーカイブ名.zip 対象ディレクトリ

# zip を展開
unzip アーカイブ名.zip

# 指定ディレクトリに展開
unzip アーカイブ名.zip -d 展開先

# zip の内容を確認（展開しない）
unzip -l アーカイブ名.zip

# パスワード付き zip を作成
zip -r -e アーカイブ名.zip 対象ディレクトリ
```

!!! note "説明"
    Windows との互換性が必要な場合は `zip` を使います。Linux 間でのやり取りは `tar.gz` の方が一般的です。

#### 使用例

```bash
# Windows に渡すファイルを zip で圧縮
zip -r report_$(date +%Y%m%d).zip ./report/

# 展開先ディレクトリを指定して解凍
unzip report.zip -d ~/downloads/report/
```

---

## システム管理

### サービスを操作する（systemctl）

```bash
# サービスを起動
sudo systemctl start サービス名

# サービスを停止
sudo systemctl stop サービス名

# サービスを再起動
sudo systemctl restart サービス名

# 設定を再読み込み（プロセスは再起動しない）
sudo systemctl reload サービス名

# サービスの状態を確認
sudo systemctl status サービス名

# 自動起動を有効化
sudo systemctl enable サービス名

# 自動起動を無効化
sudo systemctl disable サービス名

# デーモン設定を再読み込み（Unit ファイル変更後）
sudo systemctl daemon-reload

# 全サービス一覧を表示
systemctl list-units --type=service

# 失敗したサービスを確認
systemctl --failed
```

!!! note "説明"
    `enable` を付けないとシステム再起動後にサービスが起動しません。
    新しいサービスを追加した場合は `daemon-reload` で設定を反映させてから `enable` します。

#### 使用例

```bash
# nginx を起動して自動起動を有効化し、状態を確認
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

---

### ジャーナルログを確認する（journalctl）

```bash
# 全ログを表示
journalctl

# 特定サービスのログを表示
journalctl -u サービス名

# リアルタイムでログを追跡
journalctl -u サービス名 -f

# 最新 N 行を表示
journalctl -u サービス名 -n 100

# 今日のログのみ表示
journalctl -u サービス名 --since today

# 指定日時以降のログを表示
journalctl --since "2025-01-01 00:00:00"

# エラーレベル以上のログのみ表示
journalctl -p err

# ログのディスク使用量を確認
journalctl --disk-usage
```

!!! note "説明"
    `-f` でリアルタイム追跡、`-u` でサービス名を指定します。`-p err` はエラー以上のログのみ表示するフィルタです。

#### 使用例

```bash
# nginx のエラーログをリアルタイム監視
journalctl -u nginx -f

# 直近1時間のエラーを確認
journalctl -p err --since "1 hour ago"
```

---

### ディスク使用量を確認する

```bash
# マウント済みファイルシステムの使用量を確認
df -h

# ファイルシステムの種類も表示
df -hT

# ディレクトリのサイズを確認
du -sh ディレクトリ名

# サブディレクトリのサイズを一覧表示
du -sh ./*

# サイズ順に表示
du -sh ~/* | sort -rh

# 1階層の使用量を表示
du -h --max-depth=1 /

# ブロックデバイス一覧
lsblk

# マウント状況を確認
mount | grep -E "^/dev"
```

!!! note "説明"
    `df` はファイルシステム全体の空き容量、`du` は特定のディレクトリのサイズを調べます。
    `du -sh ~/* | sort -rh` でホームディレクトリの容量を食っているものを特定できます。

#### 使用例

```bash
# ホームディレクトリの容量上位を確認
du -sh ~/* | sort -rh | head -10

# ディスク全体の空き容量を確認
df -h /
```

---

### ユーザーを管理する

```bash
# 現在のユーザー名を表示
whoami

# ユーザー ID とグループ情報を表示
id

# ユーザーを追加
sudo adduser ユーザー名

# ユーザーを削除（ホームディレクトリも削除）
sudo deluser --remove-home ユーザー名

# ユーザーをグループに追加
sudo usermod -aG グループ名 ユーザー名

# 別ユーザーに切り替え
su - ユーザー名

# スーパーユーザーに切り替え
sudo su

# パスワードを変更
passwd

# ログイン中のユーザー一覧
who
```

!!! note "説明"
    グループへの追加（`usermod -aG`）は再ログイン後に有効になります。
    `docker` グループへの追加は `sudo` なしで Docker を使うためによく行います。

#### 使用例

```bash
# 現在のユーザーを docker グループに追加して確認
sudo usermod -aG docker $USER
# 再ログインしてから確認
id
```

---

## 環境設定

### 環境変数を設定する

```bash
# 環境変数の一覧を表示
env
printenv

# 特定の環境変数を表示
echo $HOME
echo $PATH
printenv PATH

# 環境変数を設定（現在のシェルのみ）
export 変数名=値

# PATH にパスを追加
export PATH="$PATH:/新しいパス"

# 環境変数を削除
unset 変数名

# シェル設定ファイルを再読み込み
source ~/.bashrc
```

!!! note "説明"
    `export` で設定した変数は現在のセッションのみ有効です。永続化するには `~/.bashrc` に記述して `source ~/.bashrc` で反映させます。

#### 使用例

```bash
# API キーを環境変数に設定して使用（~/.bashrc に追記する例）
echo 'export OPENAI_API_KEY="your-api-key-here"' >> ~/.bashrc
source ~/.bashrc
echo $OPENAI_API_KEY
```

---

### alias を定義する

```bash
# alias の一覧を確認
alias

# alias を定義
alias ll='ls -la'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'

# alias を削除
unalias ll

# alias を永続化（~/.bashrc に追記）
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc
```

!!! note "説明"
    alias はシェルを再起動すると消えます。永続化するには `~/.bashrc` に記述します。
    よく使うコマンドを短縮したり、`rm` に `-i` を付けて安全にするのに使います。

#### 使用例

```bash
# よく使う alias をまとめて ~/.bashrc に追記
cat << 'EOF' >> ~/.bashrc
alias ll='ls -la'
alias la='ls -a'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
EOF
source ~/.bashrc
```

---

### シェルスクリプトを作成する

```bash
#!/bin/bash
# スクリプトの基本構文

# 変数の定義と使用
NAME="World"
echo "Hello, $NAME"

# コマンドの出力を変数に代入
DATE=$(date +%Y-%m-%d)
echo "今日の日付: $DATE"

# if 文
if [ "$1" = "hello" ]; then
  echo "こんにちは"
elif [ "$1" = "bye" ]; then
  echo "さようなら"
else
  echo "引数: $1"
fi

# for ループ（ファイル一覧）
for file in *.txt; do
  echo "処理中: $file"
done

# while ループ
COUNT=0
while [ "$COUNT" -lt 5 ]; do
  echo "カウント: $COUNT"
  COUNT=$((COUNT + 1))
done

# 関数定義
greet() {
  echo "Hello, $1"
}
greet "Alice"

# 終了コードを確認
ls /nonexistent 2>/dev/null
echo "終了コード: $?"
```

!!! note "説明"
    スクリプトファイルを作成したら `chmod +x スクリプト名.sh` で実行権限を付与します。
    `$1` `$2` はコマンドライン引数、`$0` はスクリプト自身のパスを表します。

#### 使用例

```bash
# バックアップスクリプトを作成して実行
cat << 'EOF' > backup.sh
#!/bin/bash
TARGET="$HOME/projects"
DEST="$HOME/backups"
DATE=$(date +%Y%m%d_%H%M%S)
mkdir -p "$DEST"
tar -czf "$DEST/backup_$DATE.tar.gz" "$TARGET"
echo "完了: $DEST/backup_$DATE.tar.gz"
EOF

chmod +x backup.sh
./backup.sh
```

---

## 便利なコマンド

### コマンド履歴を操作する

```bash
# コマンド履歴を表示
history

# 最新 N 件を表示
history 20

# 履歴番号を指定して実行
!番号

# 直前のコマンドを再実行
!!

# 直前のコマンドの最後の引数を使う
!$

# 文字列で始まる直近のコマンドを実行
!git

# 履歴をインクリメンタル検索
# （Ctrl を押しながら r を押す）

# 画面をクリア
clear
```

!!! note "説明"
    `Ctrl` を押しながら `r` でインクリメンタル検索ができます。部分文字列を入力すると一致するコマンドが表示され、`Enter` で実行します。

#### 使用例

```bash
# 直前の sudo コマンドを再実行
sudo apt install vim
!!   # 同じコマンドを再実行

# 直前の引数を使い回す
ls -la ~/projects/myapp
cd !$   # ~/projects/myapp に移動
```

---

### システム情報を確認する

```bash
# OS・カーネル情報を表示
uname -a

# ディストリビューション情報を確認
cat /etc/os-release
lsb_release -a

# CPU 情報を確認
lscpu

# メモリ使用状況を確認
free -h

# システム稼働時間を確認
uptime

# ホスト名を確認
hostname

# コマンドの実行時間を計測
time コマンド

# コマンドのパスを確認
which コマンド名
```

!!! note "説明"
    `uname -a` と `cat /etc/os-release` で OS の詳細情報をほぼすべて確認できます。
    `time コマンド` はスクリプトやプログラムのパフォーマンス測定に便利です。

#### 使用例

```bash
# システム情報を一括確認
echo "=== OS ===" && uname -a
echo "=== Distro ===" && cat /etc/os-release | grep PRETTY_NAME
echo "=== Memory ===" && free -h
echo "=== CPU ===" && lscpu | grep "Model name"
```

---

### ファイルの差分を確認する（diff）

```bash
# 2つのファイルの差分を表示
diff ファイル1 ファイル2

# 差分を統一フォーマットで表示（patch に使いやすい形式）
diff -u ファイル1 ファイル2

# ディレクトリ同士の差分を表示
diff -r ディレクトリ1 ディレクトリ2

# 差分を無視するオプション
diff -i ファイル1 ファイル2   # 大文字小文字を無視
diff -b ファイル1 ファイル2   # 空白の違いを無視
diff -B ファイル1 ファイル2   # 空行の違いを無視

# カラー表示（colordiff 要インストール）
colordiff ファイル1 ファイル2
```

!!! note "説明"
    `diff -u` の出力は `patch` コマンドでそのまま適用できるパッチ形式です。
    `colordiff` は `sudo apt install colordiff` でインストールできます。

#### 使用例

```bash
# 設定ファイルの変更前後を比較
diff -u ~/.bashrc.bak ~/.bashrc

# 2つのディレクトリの構成を比較
diff -r ./config_v1/ ./config_v2/
```

---

### 定期実行を設定する（cron）

```bash
# crontab を編集
crontab -e

# 現在の crontab を表示
crontab -l

# crontab を削除
crontab -r
```

!!! note "説明"
    cron の書式は `分 時 日 月 曜日 コマンド` の順です。
    よく使うパターンを以下に示します。

    | 書式 | 意味 |
    | --- | --- |
    | `0 2 * * *` | 毎日 2:00 に実行 |
    | `*/5 * * * *` | 5分ごとに実行 |
    | `0 9 * * 1-5` | 平日 9:00 に実行 |
    | `0 0 1 * *` | 毎月1日 0:00 に実行 |
    | `@reboot` | 起動時に実行 |
    | `@daily` | 毎日1回実行 |

#### 使用例

```bash
# crontab を編集して毎日2時にバックアップを実行
crontab -e
# 以下を追記:
# 0 2 * * * /home/user/backup.sh >> /home/user/backup.log 2>&1

# 設定を確認
crontab -l
```

---

### ファイルのハッシュ値を確認する

```bash
# MD5 ハッシュを計算
md5sum ファイル名

# SHA256 ハッシュを計算（推奨）
sha256sum ファイル名

# SHA512 ハッシュを計算
sha512sum ファイル名

# 文字列のハッシュを計算
echo -n "文字列" | sha256sum

# Base64 エンコード
echo "テキスト" | base64

# Base64 デコード
echo "エンコード済み文字列" | base64 -d
```

!!! note "説明"
    ダウンロードしたファイルの整合性確認に使います。公式サイトに記載されたハッシュ値と一致するか確認します。

#### 使用例

```bash
# ダウンロードファイルのハッシュ値を確認
sha256sum ubuntu-24.04.iso
# 公式サイトの値と比較して一致すれば OK
```

---

### テキストを変換・整形する

```bash
# JSON を整形表示（python3）
echo '{"key":"value"}' | python3 -m json.tool

# JSON を整形表示（jq 要インストール）
cat data.json | jq .

# 特定フィールドを抽出（jq）
cat data.json | jq '.key'

# 文字コードを確認
file -i ファイル名

# 文字コードを変換
iconv -f SHIFT_JIS -t UTF-8 入力ファイル > 出力ファイル

# Windows 改行コードを Linux 形式に変換
dos2unix ファイル名

# Linux 改行コードを Windows 形式に変換
unix2dos ファイル名

# 列を揃えて表示
column -t ファイル名
```

!!! note "説明"
    `dos2unix` は Windows からコピーしたスクリプトが正常に動かない場合の修正によく使います。
    `jq` は `sudo apt install jq` でインストールできます。

#### 使用例

```bash
# API レスポンスを整形して確認
curl -s https://api.github.com/users/octocat | python3 -m json.tool

# Windows で作成したスクリプトの改行コードを変換して実行
dos2unix start.sh
chmod +x start.sh
./start.sh
```

---

## オプション早見表

### ls オプション

| オプション | 説明 |
| --- | --- |
| `-l` | 詳細表示（権限・サイズ・日時） |
| `-a` | 隠しファイルを含む全ファイルを表示 |
| `-h` | ファイルサイズを人間が読みやすい形式で表示 |
| `-t` | 更新日時順（新しい順）に並べ替え |
| `-r` | 逆順に並べ替え |
| `-S` | ファイルサイズ順に並べ替え |
| `-R` | サブディレクトリも再帰的に表示 |
| `-1` | 1行に1ファイルずつ表示 |

---

### grep オプション

| オプション | 説明 |
| --- | --- |
| `-i` | 大文字小文字を区別しない |
| `-n` | 行番号を表示 |
| `-v` | マッチしない行を表示 |
| `-r` | ディレクトリを再帰的に検索 |
| `-l` | マッチしたファイル名のみ表示 |
| `-c` | マッチした行数を表示 |
| `-E` | 拡張正規表現を使用 |
| `-o` | マッチした部分のみ表示 |
| `-A N` | マッチ行の後 N 行も表示 |
| `-B N` | マッチ行の前 N 行も表示 |
| `-C N` | マッチ行の前後 N 行を表示 |
| `--color` | マッチ部分をカラー表示 |

---

### find オプション

| オプション | 説明 |
| --- | --- |
| `-name` | ファイル名で検索（大文字小文字区別あり） |
| `-iname` | ファイル名で検索（大文字小文字区別なし） |
| `-type f` | ファイルのみ検索 |
| `-type d` | ディレクトリのみ検索 |
| `-type l` | シンボリックリンクのみ検索 |
| `-mtime -N` | N 日以内に更新されたファイル |
| `-size +NM` | N MB 以上のファイル |
| `-empty` | 空のファイル・ディレクトリ |
| `-exec CMD {} \;` | 見つかったファイルにコマンドを実行 |
| `-maxdepth N` | 最大 N 階層まで検索 |

---

### tar オプション

| オプション | 説明 |
| --- | --- |
| `-c` | アーカイブを作成 |
| `-x` | アーカイブを展開 |
| `-t` | アーカイブの内容を一覧表示 |
| `-z` | gzip で圧縮・展開 |
| `-j` | bzip2 で圧縮・展開 |
| `-J` | xz で圧縮・展開 |
| `-f ファイル名` | アーカイブファイル名を指定 |
| `-v` | 詳細表示（処理中のファイル名を表示） |
| `-C ディレクトリ` | 展開先ディレクトリを指定 |

---

### chmod パーミッション数値早見表

| 数値 | 意味 |
| --- | --- |
| `777` | 全ユーザーが読み書き実行可 |
| `755` | 所有者は読み書き実行可、それ以外は読み・実行のみ |
| `644` | 所有者は読み書き可、それ以外は読み取りのみ |
| `600` | 所有者のみ読み書き可（SSH 秘密鍵に使用） |
| `700` | 所有者のみ読み書き実行可 |
| `400` | 所有者のみ読み取り可（読み取り専用） |

---

## 設定

### .wslconfig を設定する

`C:\Users\ユーザー名\.wslconfig` に記述します（WSL2 全体の設定）。

```ini
[wsl2]
# 割り当てメモリ（例: 8GB）
memory=8GB

# 割り当て CPU コア数
processors=4

# スワップサイズ
swap=2GB

# ローカルホストフォワーディングを有効化
localhostForwarding=true
```

!!! note "説明"
    変更後は PowerShell から `wsl --shutdown` で WSL を再起動して反映させます。

#### 使用例

```powershell
# WSL を再起動して設定を反映
wsl --shutdown
wsl
```

---

### wsl.conf を設定する

Ubuntu 内の `/etc/wsl.conf` に記述します（個別ディストリビューションの設定）。

```ini
[automount]
# Windows ドライブを自動マウント
enabled = true
root = /mnt/
options = "metadata,umask=22,fmask=11"

[network]
# ホスト名を指定
hostname = my-ubuntu
generateHosts = true
generateResolvConf = true

[interop]
# Windows コマンドを WSL から実行可能にする
enabled = true
appendWindowsPath = true

[user]
# デフォルトユーザーを指定
default = ユーザー名

[boot]
# WSL 起動時にコマンドを実行（WSL2 のみ）
command = "service cron start"
```

!!! note "説明"
    変更後は PowerShell から `wsl --shutdown` で WSL を再起動して反映させます。

#### 使用例

```bash
# /etc/wsl.conf を作成・編集
sudo vim /etc/wsl.conf
```

---

### .bashrc を設定する

`~/.bashrc` に記述します。シェル起動時に自動で読み込まれます。

```bash
# ---- alias ----
alias ll='ls -la'
alias la='ls -a'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias diff='diff --color=auto'
alias mkdir='mkdir -pv'
alias cp='cp -iv'
alias mv='mv -iv'

# ---- PATH の設定 ----
export PATH="$HOME/.local/bin:$PATH"

# ---- 環境変数 ----
export EDITOR=vim
export LANG=ja_JP.UTF-8

# ---- プロンプトのカスタマイズ ----
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# ---- 履歴の設定 ----
export HISTSIZE=10000
export HISTFILESIZE=20000
export HISTCONTROL=ignoredups:ignorespace
shopt -s histappend
```

!!! note "説明"
    変更後は `source ~/.bashrc` で現在のシェルに反映させます。再起動時は自動的に読み込まれます。

#### 使用例

```bash
# .bashrc を編集して反映
vim ~/.bashrc
source ~/.bashrc
```

---

### Git 初期設定をする

```bash
# ユーザー名とメールアドレスを設定
git config --global user.name "あなたの名前"
git config --global user.email "your@email.com"

# デフォルトエディタを設定
git config --global core.editor vim

# デフォルトブランチ名を設定
git config --global init.defaultBranch main

# 改行コードの自動変換を設定（Linux 環境推奨）
git config --global core.autocrlf input

# 設定を確認
git config --global --list

# 設定ファイルを直接編集
git config --global --edit
```

!!! note "説明"
    `git config --global` の設定は `~/.gitconfig` に保存されます。
    リポジトリごとに設定を変えたい場合は `--global` を外して実行します（`.git/config` に保存）。

#### 使用例

```bash
# 初期設定を一括実行
git config --global user.name "Taro Yamada"
git config --global user.email "taro@example.com"
git config --global core.editor vim
git config --global init.defaultBranch main
git config --global --list
```

---

### Python 環境を設定する（uv）

```bash
# uv をインストール
curl -LsSf https://astral.sh/uv/install.sh | sh

# インストール後に PATH を反映
source ~/.bashrc

# 新しいプロジェクトを作成
uv init プロジェクト名

# Python バージョンを指定して作成
uv init --python 3.12 プロジェクト名

# 仮想環境を作成
uv venv

# 仮想環境を有効化
source .venv/bin/activate

# パッケージを追加
uv add パッケージ名

# 開発用パッケージを追加
uv add --dev パッケージ名

# パッケージを削除
uv remove パッケージ名

# 依存関係をインストール
uv sync

# スクリプトを実行
uv run python スクリプト名.py
```

!!! note "説明"
    `uv` は Rust 製の高速な Python パッケージマネージャです。
    `pip` より大幅に高速で、仮想環境と依存関係を統合的に管理できます。

#### 使用例

```bash
# FastAPI プロジェクトを作成してサーバーを起動
uv init myapp
cd myapp
uv add fastapi uvicorn
uv run uvicorn main:app --reload
```

---

### .vimrc を設定する

`~/.vimrc` に記述します。

```vim
" 行番号を表示
set number

" タブをスペースに変換
set expandtab
set tabstop=4
set shiftwidth=4
set softtabstop=4

" 検索結果をハイライト
set hlsearch

" インクリメンタルサーチを有効化
set incsearch

" 大文字小文字を区別しない（大文字を含む場合は区別）
set ignorecase
set smartcase

" シンタックスハイライトを有効化
syntax on

" ファイルタイプの自動検出
filetype plugin indent on

" バックスペースの動作を設定
set backspace=indent,eol,start

" クリップボードと共有
set clipboard=unnamedplus

" ステータスラインを常に表示
set laststatus=2
```

!!! note "説明"
    `set clipboard=unnamedplus` で vim のヤンク（コピー）がシステムクリップボードと連携します。
    WSL 環境では `xclip` または `xsel` のインストールが必要な場合があります。

#### 使用例

```bash
# .vimrc を作成・編集
vim ~/.vimrc

# クリップボード連携に必要なパッケージをインストール
sudo apt install xclip -y
```

---

## 頻出コマンド早引き

```bash
# ---- ファイル・ディレクトリ操作 ----
pwd                          # 現在のディレクトリを表示
ls -la                       # 詳細一覧（隠しファイル含む）
cd ~                         # ホームへ移動
cd -                         # 直前のディレクトリへ戻る
mkdir -p 親/子/孫            # 階層ごとディレクトリを作成
cp -r 元 先                  # ディレクトリをコピー
mv 元 先                     # 移動またはリネーム
rm -rf ディレクトリ          # ディレクトリを強制削除（注意）
touch ファイル名             # 空ファイルを作成
cat -n ファイル名            # 行番号付きで内容表示
tail -f ログファイル         # ログをリアルタイム監視
less ファイル名              # ページングして閲覧

# ---- 検索 ----
grep -rn "文字列" ./         # ディレクトリ内を再帰検索
find . -name "*.py"          # ファイルを拡張子で検索
find . -mtime -7             # 7日以内に更新されたファイル

# ---- パッケージ管理（APT）----
sudo apt update && sudo apt upgrade -y   # 一括アップデート
sudo apt install パッケージ名 -y        # インストール
sudo apt purge パッケージ名             # 完全削除

# ---- プロセス管理 ----
ps aux | grep プロセス名     # プロセスを検索
kill -9 PID                  # プロセスを強制終了
コマンド &                   # バックグラウンドで実行
nohup コマンド &             # セッション終了後も継続

# ---- ネットワーク ----
ping -c 4 google.com         # 疎通確認
curl -s URL | python3 -m json.tool   # API レスポンスを整形表示
ss -tuln                     # リッスン中のポートを確認

# ---- アーカイブ ----
tar -czf archive.tar.gz ./dir/   # tar.gz を作成
tar -xzf archive.tar.gz          # tar.gz を展開
tar -tzf archive.tar.gz          # 内容を確認

# ---- システム情報 ----
df -h                        # ディスク空き容量
free -h                      # メモリ使用状況
du -sh ~/* | sort -rh        # ホームの容量ランキング
uname -a                     # OS・カーネル情報

# ---- サービス管理 ----
sudo systemctl start サービス名    # 起動
sudo systemctl enable サービス名   # 自動起動を有効化
sudo systemctl status サービス名   # 状態確認
journalctl -u サービス名 -f        # ログをリアルタイム監視

# ---- テキスト処理 ----
sed -i 's/旧/新/g' ファイル名              # 一括置換
awk -F':' '{print $1}' /etc/passwd        # フィールド抽出
sort ファイル名 | uniq -c | sort -rn       # 出現回数ランキング
diff -u ファイル1 ファイル2               # 差分確認

# ---- よく使う組み合わせ ----
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
du -sh ~/* | sort -rh | head -10
find . -name "*.pyc" -exec rm {} \;
grep -rn "TODO" ./src/
```

!!! info "公式ドキュメント"
    - [WSL ドキュメント（Microsoft）](https://learn.microsoft.com/ja-jp/windows/wsl/)
    - [Ubuntu マニュアルページ](https://manpages.ubuntu.com/)
    - [GNU Coreutils マニュアル](https://www.gnu.org/software/coreutils/manual/)
    - [Bash リファレンスマニュアル](https://www.gnu.org/software/bash/manual/)
    - [uv ドキュメント](https://docs.astral.sh/uv/)
