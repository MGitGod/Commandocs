# WSL (Ubuntu)

WSL（Windows Subsystem for Linux）上のUbuntuで使用する主要コマンドをまとめたリファレンスです。

---

## インストール・アップデート・アンインストール

### WSL 自体のインストール（PowerShell / Windows側）

WSLをWindowsにインストールする。デフォルトはUbuntu。

```powershell
# WSL と Ubuntu をインストール
wsl --install

# ディストリビューションを指定してインストール
wsl --install -d Ubuntu-22.04

# インストール可能なディストリビューション一覧を表示
wsl --list --online
```

**使い方の例：**

```powershell
# Ubuntu 24.04 をインストールする
wsl --install -d Ubuntu-24.04
```

---

### WSL のアップデート（PowerShell / Windows側）

WSL カーネル・コンポーネントをアップデートする。

```powershell
# WSL 本体をアップデート
wsl --update

# WSL のバージョンを確認
wsl --version

# インストール済みディストリビューションの一覧表示
wsl --list --verbose
```

**使い方の例：**

```powershell
# バージョン確認後にアップデート
wsl --version
wsl --update
```

---

### WSL のアンインストール（PowerShell / Windows側）

ディストリビューションを削除する。

```powershell
# 指定したディストリビューションを登録解除（削除）
wsl --unregister Ubuntu

# ディストリビューションのエクスポート（バックアップ）
wsl --export Ubuntu C:\backup\ubuntu.tar

# ディストリビューションのインポート（復元）
wsl --import Ubuntu C:\WSL\Ubuntu C:\backup\ubuntu.tar
```

**使い方の例：**

```powershell
# 削除前にバックアップを取る
wsl --export Ubuntu D:\backup\ubuntu_backup.tar
wsl --unregister Ubuntu
```

---

### APT パッケージ管理（Ubuntu内）

Ubuntuのパッケージマネージャ `apt` を使ったインストール・アップデート・アンインストール。

```bash
# パッケージリストを更新
sudo apt update

# インストール済みパッケージをアップグレード
sudo apt upgrade

# update と upgrade を一括実行
sudo apt update && sudo apt upgrade -y

# パッケージをインストール
sudo apt install パッケージ名

# パッケージをアンインストール（設定ファイルは残す）
sudo apt remove パッケージ名

# パッケージと設定ファイルを完全に削除
sudo apt purge パッケージ名

# 不要な依存パッケージを削除
sudo apt autoremove

# キャッシュを削除
sudo apt clean

# パッケージを検索
apt search キーワード

# パッケージの詳細情報を表示
apt show パッケージ名

# インストール済みパッケージ一覧
apt list --installed
```

**使い方の例：**

```bash
# git をインストールする
sudo apt update
sudo apt install git -y

# vim を完全に削除する
sudo apt purge vim
sudo apt autoremove
```

---

### Snap パッケージ管理

```bash
# snap パッケージのインストール
sudo snap install パッケージ名

# snap パッケージの削除
sudo snap remove パッケージ名

# インストール済み snap 一覧
snap list

# snap パッケージのアップデート
sudo snap refresh
```

**使い方の例：**

```bash
# VS Code を snap でインストール
sudo snap install code --classic
```

---

## 基本コマンド

### ディレクトリ操作

```bash
# 現在のディレクトリを表示
pwd

# ディレクトリを移動
cd ディレクトリ名

# ホームディレクトリへ移動
cd ~
cd

# 1つ上のディレクトリへ移動
cd ..

# 直前のディレクトリへ戻る
cd -

# ディレクトリを作成
mkdir ディレクトリ名

# 深い階層のディレクトリを一括作成
mkdir -p 親ディレクトリ/子ディレクトリ/孫ディレクトリ

# ディレクトリを削除（空のディレクトリのみ）
rmdir ディレクトリ名

# ディレクトリを中身ごと削除
rm -r ディレクトリ名

# 確認なしで強制削除（注意）
rm -rf ディレクトリ名
```

**使い方の例：**

```bash
# projects/myapp/src ディレクトリを一括作成
mkdir -p ~/projects/myapp/src

# work ディレクトリへ移動して現在地を確認
cd ~/work && pwd
```

---

### ファイル操作

```bash
# ファイルを作成（空ファイル）
touch ファイル名

# ファイルをコピー
cp コピー元 コピー先

# ディレクトリごとコピー
cp -r コピー元ディレクトリ コピー先

# ファイルを移動・リネーム
mv 移動元 移動先

# ファイルを削除
rm ファイル名

# 複数ファイルを削除
rm ファイル1 ファイル2

# ファイルの内容を表示
cat ファイル名

# ファイルの先頭10行を表示
head ファイル名

# ファイルの先頭N行を表示
head -n 20 ファイル名

# ファイルの末尾10行を表示
tail ファイル名

# ファイルの末尾N行を表示
tail -n 20 ファイル名

# リアルタイムでファイルの末尾を追跡表示
tail -f ファイル名

# ファイルをページごとに閲覧（q で終了）
less ファイル名

# ファイルの行数・単語数・バイト数を表示
wc ファイル名

# 行数のみ表示
wc -l ファイル名
```

**使い方の例：**

```bash
# 設定ファイルをバックアップしてから編集
cp ~/.bashrc ~/.bashrc.bak
vim ~/.bashrc

# ログファイルの末尾をリアルタイム監視
tail -f /var/log/syslog
```

---

### ファイル・ディレクトリの一覧表示

```bash
# 一覧表示
ls

# 詳細表示（権限・サイズ・日時）
ls -l

# 隠しファイルも表示
ls -a

# 詳細 + 隠しファイル
ls -la

# サイズを人間が読みやすい形式で表示
ls -lh

# 更新日時順に表示（新しい順）
ls -lt

# 再帰的に表示
ls -lR

# ツリー形式で表示（tree コマンドが必要）
tree

# 深さを指定してツリー表示
tree -L 2
```

**使い方の例：**

```bash
# ホームディレクトリの全ファイルをサイズ付きで表示
ls -lah ~

# カレントディレクトリを2階層のツリーで表示
tree -L 2
```

---

### テキスト表示・編集

```bash
# 標準出力にテキストを表示
echo "テキスト"

# ファイルへ書き込み（上書き）
echo "テキスト" > ファイル名

# ファイルへ追記
echo "テキスト" >> ファイル名

# 複数行をファイルに書き込む（ヒアドキュメント）
cat << EOF > ファイル名
1行目
2行目
EOF

# vim でファイルを編集
vim ファイル名

# nano でファイルを編集
nano ファイル名
```

**使い方の例：**

```bash
# README.md を作成して内容を書き込む
echo "# My Project" > README.md
echo "プロジェクトの説明" >> README.md
```

---

### パーミッション（権限）操作

```bash
# ファイルの権限を変更（数値指定）
chmod 755 ファイル名

# ファイルの権限を変更（記号指定）
chmod u+x ファイル名   # 所有者に実行権限を付与
chmod g-w ファイル名   # グループの書き込み権限を削除
chmod o+r ファイル名   # その他ユーザーに読み取り権限を付与
chmod a+x ファイル名   # 全員に実行権限を付与

# ディレクトリ以下すべての権限を変更
chmod -R 755 ディレクトリ名

# ファイルの所有者を変更
chown ユーザー名 ファイル名

# 所有者とグループを変更
chown ユーザー名:グループ名 ファイル名

# 再帰的に変更
chown -R ユーザー名 ディレクトリ名
```

**使い方の例：**

```bash
# シェルスクリプトに実行権限を付与
chmod +x start.sh

# /var/www を www-data の所有にする
sudo chown -R www-data:www-data /var/www
```

---

## よく使うコマンド

### テキスト検索（grep）

```bash
# ファイル内でパターンを検索
grep "検索文字列" ファイル名

# 大文字小文字を区別しない
grep -i "検索文字列" ファイル名

# 行番号を表示
grep -n "検索文字列" ファイル名

# マッチしない行を表示
grep -v "除外文字列" ファイル名

# ディレクトリ内を再帰的に検索
grep -r "検索文字列" ディレクトリ名

# 再帰検索 + 行番号 + ファイル名
grep -rn "検索文字列" ディレクトリ名

# 正規表現で検索
grep -E "パターン" ファイル名

# マッチした件数を表示
grep -c "検索文字列" ファイル名

# マッチしたファイル名のみ表示
grep -l "検索文字列" *.txt
```

**使い方の例：**

```bash
# Pythonファイル内の "def " を行番号付きで検索
grep -n "def " app.py

# src/ ディレクトリ内の "import" を再帰検索
grep -rn "import" ./src/
```

---

### ファイル検索（find）

```bash
# ファイル名で検索
find 検索パス -name "ファイル名"

# 拡張子で検索
find . -name "*.py"

# ディレクトリのみ検索
find . -type d -name "ディレクトリ名"

# ファイルのみ検索
find . -type f -name "*.txt"

# 更新日時が N 日以内のファイルを検索
find . -mtime -3

# サイズが N MB 以上のファイルを検索
find . -size +10M

# 検索結果に対してコマンドを実行
find . -name "*.log" -exec rm {} \;

# 検索結果を xargs に渡す
find . -name "*.py" | xargs grep "main"

# 空ファイルを検索
find . -empty
```

**使い方の例：**

```bash
# カレントディレクトリ以下の .pyc ファイルをすべて削除
find . -name "*.pyc" -exec rm {} \;

# 更新日時が7日以内の .log ファイルを一覧表示
find /var/log -name "*.log" -mtime -7
```

---

### プロセス管理

```bash
# 実行中のプロセスを表示
ps

# すべてのプロセスを表示
ps aux

# プロセスをリアルタイム監視
top

# より見やすいプロセスモニタ（要インストール）
htop

# プロセス名で検索
pgrep プロセス名

# プロセスを終了（PIDを指定）
kill PID

# 強制終了
kill -9 PID

# プロセス名で終了
pkill プロセス名

# バックグラウンドでコマンドを実行
コマンド &

# バックグラウンドジョブ一覧表示
jobs

# バックグラウンドジョブをフォアグラウンドに戻す
fg %ジョブ番号

# フォアグラウンドをバックグラウンドに送る
bg %ジョブ番号
```

**使い方の例：**

```bash
# Python スクリプトをバックグラウンドで実行
python3 server.py &

# "python" という名前のプロセスを全終了
pkill python
```

---

### ネットワーク

```bash
# IPアドレスを確認
ip addr
ip a

# ネットワークインターフェース情報（非推奨だが頻出）
ifconfig

# 疎通確認
ping ホスト名またはIPアドレス

# N回だけ ping を送る
ping -c 4 google.com

# ルートトレース
traceroute ホスト名

# ポートが開いているか確認
nc -zv ホスト名 ポート番号

# 開いているポートを確認
ss -tuln

# netstat（要 net-tools）
netstat -tuln

# URLからファイルをダウンロード
wget URL

# バックグラウンドでダウンロード
wget -b URL

# curl でデータ取得
curl URL

# curl でレスポンスヘッダーも表示
curl -i URL

# curl でPOSTリクエスト
curl -X POST -d "data=value" URL

# DNS 名前解決を確認
nslookup ドメイン名
dig ドメイン名
```

**使い方の例：**

```bash
# google.com に4回 ping を送る
ping -c 4 google.com

# ファイルをダウンロードして保存
wget https://example.com/file.tar.gz

# REST API にGETリクエストを送る
curl -s https://api.example.com/data | python3 -m json.tool
```

---

### アーカイブ・圧縮

```bash
# tar.gz を作成
tar -czf アーカイブ名.tar.gz 対象ディレクトリ

# tar.gz を展開
tar -xzf アーカイブ名.tar.gz

# tar.gz を指定ディレクトリに展開
tar -xzf アーカイブ名.tar.gz -C 展開先ディレクトリ

# アーカイブの内容を確認（展開しない）
tar -tzf アーカイブ名.tar.gz

# zip を作成
zip -r アーカイブ名.zip 対象ディレクトリ

# zip を展開
unzip アーカイブ名.zip

# zip を指定ディレクトリに展開
unzip アーカイブ名.zip -d 展開先

# gzip で圧縮（元ファイルは削除される）
gzip ファイル名

# gzip を展開
gunzip ファイル名.gz
```

**使い方の例：**

```bash
# project/ を圧縮してバックアップ
tar -czf project_backup_$(date +%Y%m%d).tar.gz ./project/

# 展開して内容を確認
tar -tzf project_backup_20250101.tar.gz
```

---

### ユーザー・グループ管理

```bash
# 現在のユーザー名を表示
whoami

# ユーザーIDとグループ情報を表示
id

# ログイン中のユーザー一覧
who

# スーパーユーザーに切り替え
sudo su

# 別ユーザーに切り替え
su - ユーザー名

# ユーザーを追加
sudo adduser ユーザー名

# ユーザーを削除（ホームディレクトリも削除）
sudo deluser --remove-home ユーザー名

# ユーザーをグループに追加
sudo usermod -aG グループ名 ユーザー名

# グループ一覧
cat /etc/group

# パスワード変更
passwd

# 他ユーザーのパスワード変更（root権限）
sudo passwd ユーザー名
```

**使い方の例：**

```bash
# 現在のユーザーを docker グループに追加
sudo usermod -aG docker $USER

# 変更を反映させるため再ログイン後に確認
id
```

---

## 上級者向けコマンド

### テキスト処理（awk / sed / cut / sort / uniq）

```bash
# awk: 区切り文字でフィールドを抽出
awk '{print $1}' ファイル名

# 区切り文字を指定
awk -F':' '{print $1}' /etc/passwd

# 条件付き出力
awk '$3 > 1000 {print $1, $3}' ファイル名

# sed: 文字列の置換
sed 's/置換前/置換後/' ファイル名

# sed: 全箇所を置換（g オプション）
sed 's/置換前/置換後/g' ファイル名

# sed: ファイルを直接書き換え
sed -i 's/置換前/置換後/g' ファイル名

# sed: 特定行を削除
sed '3d' ファイル名

# sed: 範囲行を削除
sed '2,5d' ファイル名

# cut: 区切り文字で特定列を抽出
cut -d':' -f1 /etc/passwd

# sort: 並べ替え
sort ファイル名

# 数値として並べ替え
sort -n ファイル名

# 逆順に並べ替え
sort -r ファイル名

# 重複行を削除（sort後に使う）
sort ファイル名 | uniq

# 重複行のみ表示
sort ファイル名 | uniq -d

# 重複回数を表示
sort ファイル名 | uniq -c
```

**使い方の例：**

```bash
# /etc/passwd からユーザー名一覧を取得
awk -F':' '{print $1}' /etc/passwd

# access.log の中の "ERROR" を "WARNING" に一括置換
sed -i 's/ERROR/WARNING/g' access.log

# ログのIPアドレス列を抽出してアクセス数上位10件を表示
cut -d' ' -f1 access.log | sort | uniq -c | sort -rn | head -10
```

---

### パイプとリダイレクト

```bash
# パイプ: コマンドの出力を次のコマンドへ渡す
コマンド1 | コマンド2

# 標準出力をファイルへ書き込み（上書き）
コマンド > ファイル名

# 標準出力をファイルへ追記
コマンド >> ファイル名

# 標準エラー出力をファイルへ書き込み
コマンド 2> エラーログ.txt

# 標準出力と標準エラー出力を同じファイルへ
コマンド > ファイル名 2>&1

# 出力を捨てる（表示しない）
コマンド > /dev/null

# エラーも捨てる
コマンド > /dev/null 2>&1

# tee: 出力をファイルと標準出力の両方へ
コマンド | tee ファイル名

# tee で追記
コマンド | tee -a ファイル名

# xargs: 前のコマンドの出力をコマンドの引数に渡す
echo "hello world" | xargs echo
find . -name "*.txt" | xargs wc -l
```

**使い方の例：**

```bash
# pip list の出力をファイルに保存しながら表示
pip list | tee requirements_list.txt

# エラーログを別ファイルに保存
python3 app.py > output.log 2> error.log
```

---

### シェルスクリプト

```bash
# スクリプトファイルの先頭（シバン行）
#!/bin/bash

# 変数の定義と使用
NAME="World"
echo "Hello, $NAME"

# コマンドの出力を変数に代入
DATE=$(date +%Y-%m-%d)
echo "今日の日付: $DATE"

# if 文
if [ 条件 ]; then
  echo "真"
elif [ 別の条件 ]; then
  echo "別の真"
else
  echo "偽"
fi

# for ループ
for i in 1 2 3 4 5; do
  echo "番号: $i"
done

# ファイル一覧をループ
for file in *.txt; do
  echo "処理中: $file"
done

# while ループ
COUNT=0
while [ $COUNT -lt 5 ]; do
  echo "カウント: $COUNT"
  COUNT=$((COUNT + 1))
done

# 関数定義
greet() {
  echo "Hello, $1"
}
greet "Alice"

# 終了コードを確認
コマンド
echo "終了コード: $?"
```

**使い方の例：**

```bash
# バックアップスクリプトの例
#!/bin/bash
TARGET="$HOME/projects"
DEST="$HOME/backups"
DATE=$(date +%Y%m%d_%H%M%S)
mkdir -p "$DEST"
tar -czf "$DEST/backup_$DATE.tar.gz" "$TARGET"
echo "バックアップ完了: backup_$DATE.tar.gz"
```

---

### SSH

```bash
# SSH 接続
ssh ユーザー名@ホスト名またはIPアドレス

# 鍵ファイルを指定して接続
ssh -i 秘密鍵ファイル ユーザー名@ホスト名

# ポートを指定して接続
ssh -p ポート番号 ユーザー名@ホスト名

# SSH 鍵ペアを生成（Ed25519推奨）
ssh-keygen -t ed25519 -C "コメント"

# 公開鍵をリモートホストに登録
ssh-copy-id ユーザー名@ホスト名

# SSH エージェントに鍵を追加
eval $(ssh-agent)
ssh-add ~/.ssh/id_ed25519

# リモートホストでコマンドを実行
ssh ユーザー名@ホスト名 "実行するコマンド"

# ローカルポートフォワーディング
ssh -L ローカルポート:転送先ホスト:転送先ポート ユーザー名@踏み台ホスト

# ファイルをリモートへ転送（scp）
scp ローカルファイル ユーザー名@ホスト名:リモートパス

# リモートからファイルを取得
scp ユーザー名@ホスト名:リモートファイル ローカルパス
```

**使い方の例：**

```bash
# Ed25519 の SSH 鍵を生成
ssh-keygen -t ed25519 -C "mypc@work"

# GitHub に公開鍵を登録する前に内容を確認
cat ~/.ssh/id_ed25519.pub
```

---

### systemd サービス管理

```bash
# サービスの起動
sudo systemctl start サービス名

# サービスの停止
sudo systemctl stop サービス名

# サービスの再起動
sudo systemctl restart サービス名

# サービスの状態確認
sudo systemctl status サービス名

# 自動起動を有効化
sudo systemctl enable サービス名

# 自動起動を無効化
sudo systemctl disable サービス名

# 設定ファイルを再読み込み（再起動なし）
sudo systemctl reload サービス名

# デーモンの設定を再読み込み
sudo systemctl daemon-reload

# 全サービスの一覧
systemctl list-units --type=service

# 失敗したサービスを確認
systemctl --failed

# ジャーナルログを確認
journalctl -u サービス名

# リアルタイムでログを追跡
journalctl -u サービス名 -f

# 最新N行を表示
journalctl -u サービス名 -n 100
```

**使い方の例：**

```bash
# nginx を起動して自動起動を有効化
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

---

### ディスク・ストレージ管理

```bash
# ディスク使用量を確認（人間が読みやすい形式）
df -h

# ファイルシステムごとに表示
df -hT

# ディレクトリのサイズを確認
du -sh ディレクトリ名

# サブディレクトリのサイズを一覧表示
du -sh ./*/

# サイズ順に表示
du -sh ./* | sort -h

# ディスク全体の使用量（深さ1）
du -h --max-depth=1 /

# ブロックデバイスの一覧
lsblk

# マウント状況を確認
mount | grep -E "^/dev"

# ファイルシステム情報の確認
sudo fdisk -l
```

**使い方の例：**

```bash
# ホームディレクトリの容量上位を確認
du -sh ~/* | sort -rh | head -10
```

---

### 環境変数とシェル設定

```bash
# 環境変数を一覧表示
env
printenv

# 特定の環境変数を表示
echo $HOME
echo $PATH
printenv PATH

# 環境変数を設定（現在のシェルのみ）
export 変数名=値

# PATH に追加
export PATH="$PATH:/新しいパス"

# 環境変数を削除
unset 変数名

# alias を確認
alias

# alias を定義
alias ll='ls -la'
alias ..='cd ..'

# alias を削除
unalias ll

# シェルの設定ファイルを再読み込み
source ~/.bashrc
. ~/.bashrc
```

**使い方の例：**

```bash
# よく使うディレクトリへの alias を定義して即反映
echo "alias proj='cd ~/projects'" >> ~/.bashrc
source ~/.bashrc
proj
```

---

## 知ってると便利なコマンド

### 履歴・ショートカット

```bash
# コマンド履歴を表示
history

# 履歴の最新N件を表示
history 20

# 履歴番号を指定して実行
!番号

# 直前のコマンドを再実行
!!

# 直前のコマンドの引数を再利用（最後の引数）
!$

# 文字列で始まる直近のコマンドを実行
!git

# 履歴をインクリメンタル検索（Ctrl+r）
# （キーボードで Ctrl を押しながら r を押す）

# 画面をクリア
clear

# コマンドの実行場所を確認
which コマンド名
whereis コマンド名

# コマンドの種類を確認
type コマンド名

# コマンドのマニュアルを表示
man コマンド名

# コマンドの簡単な説明を表示
whatis コマンド名

# キーワードでマニュアルを検索
man -k キーワード
```

**使い方の例：**

```bash
# git コマンドのパスを確認
which git

# ls コマンドのマニュアルを確認
man ls
```

---

### 日時・時間

```bash
# 現在日時を表示
date

# フォーマット指定で表示
date +"%Y-%m-%d %H:%M:%S"

# 日付のみ
date +%Y%m%d

# Unix タイムスタンプを表示
date +%s

# コマンドの実行時間を計測
time コマンド

# システム稼働時間を表示
uptime

# カレンダーを表示
cal

# 指定月のカレンダー
cal 3 2026
```

**使い方の例：**

```bash
# スクリプトのバックアップファイルに日付を付ける
cp app.py app_$(date +%Y%m%d).py

# スクリプトの実行時間を計測
time python3 heavy_script.py
```

---

### システム情報

```bash
# OS情報を表示
uname -a

# ディストリビューション情報を確認
cat /etc/os-release
lsb_release -a

# CPU情報を確認
cat /proc/cpuinfo
lscpu

# メモリ使用状況を確認
free -h

# カーネルバージョンを確認
uname -r

# ホスト名を確認
hostname

# ホスト名を変更
sudo hostnamectl set-hostname 新しいホスト名

# 起動メッセージを確認
dmesg | tail -20

# ハードウェア情報
sudo lshw -short
```

**使い方の例：**

```bash
# OS・カーネル・アーキテクチャを一括確認
uname -a
cat /etc/os-release
free -h
```

---

### ジョブ・スケジュール（cron）

```bash
# crontab を編集
crontab -e

# 現在の crontab を表示
crontab -l

# crontab を削除
crontab -r

# cron の書式
# 分 時 日 月 曜日 コマンド
# 0 2 * * * /path/to/script.sh     # 毎日2時0分に実行
# */5 * * * * コマンド              # 5分ごとに実行
# 0 9 * * 1-5 コマンド             # 平日9時0分に実行
# @reboot コマンド                  # 起動時に実行
# @daily コマンド                   # 毎日1回実行

# at コマンドで一回限りのスケジュール
echo "コマンド" | at 10:00
echo "コマンド" | at now + 1 hour

# at のジョブ一覧
atq

# at のジョブを削除
atrm ジョブ番号
```

**使い方の例：**

```bash
# 毎日午前2時にバックアップを実行する cron を登録
crontab -e
# エディタが開いたら以下を追記:
# 0 2 * * * /home/user/backup.sh >> /home/user/backup.log 2>&1
```

---

### 便利なコマンド集

```bash
# コマンドのエイリアスを確認（全体）
alias

# コマンドの出力を行数付きで表示
cat -n ファイル名
nl ファイル名

# 2つのファイルの差分を表示
diff ファイル1 ファイル2

# 差分をカラー表示（colordiff が必要）
colordiff ファイル1 ファイル2

# ファイルの種類を確認
file ファイル名

# バイナリファイルを16進数で確認
xxd ファイル名 | head

# シンボリックリンクを作成
ln -s リンク先 リンク名

# シンボリックリンクの実体を確認
readlink -f シンボリックリンク名

# コマンドを一定間隔で繰り返し実行
watch -n 2 コマンド   # 2秒ごとに実行

# テキストを整形して表示
column -t ファイル名

# 乱数を生成
echo $RANDOM

# UUID を生成
uuidgen

# QRコードを表示（qrencode が必要）
echo "テキスト" | qrencode -t UTF8

# スリープ（待機）
sleep 秒数

# コマンドの説明を確認（tldr が必要）
tldr コマンド名
```

**使い方の例：**

```bash
# 2つの設定ファイルの差分を確認
diff ~/.bashrc ~/.bashrc.bak

# ディスク使用量を2秒ごとに監視
watch -n 2 df -h

# シンボリックリンクを作成
ln -s ~/projects/myapp/src ~/src
```

---

### テキスト変換・エンコーディング

```bash
# Base64 エンコード
echo "テキスト" | base64

# Base64 デコード
echo "エンコード済み文字列" | base64 -d

# URL エンコード（Python を利用）
python3 -c "import urllib.parse; print(urllib.parse.quote('エンコードしたいテキスト'))"

# JSON を整形表示（python3）
echo '{"key":"value"}' | python3 -m json.tool

# JSON を整形表示（jq が必要）
cat data.json | jq .

# 文字コードを確認
file -i ファイル名

# 文字コードを変換（iconv）
iconv -f SHIFT_JIS -t UTF-8 入力ファイル > 出力ファイル

# 改行コードを変換（dos2unix / unix2dos）
dos2unix ファイル名
unix2dos ファイル名

# 文字列のハッシュ値を計算
echo -n "文字列" | md5sum
echo -n "文字列" | sha256sum

# ファイルのハッシュ値を確認
md5sum ファイル名
sha256sum ファイル名
```

**使い方の例：**

```bash
# Windows からコピーしたファイルの改行コードを変換
dos2unix script.sh

# APIレスポンスのJSONを整形して表示
curl -s https://api.example.com | python3 -m json.tool
```

---

## オプション早見表

### ls オプション

| オプション | 説明 |
| --- | --- |
| `-l` | 詳細表示（権限・サイズ・日時） |
| `-a` | 隠しファイルを含む全ファイル表示 |
| `-h` | ファイルサイズを人間が読みやすい形式で表示 |
| `-t` | 更新日時順に並べ替え |
| `-r` | 逆順に並べ替え |
| `-S` | ファイルサイズ順に並べ替え |
| `-R` | サブディレクトリも再帰的に表示 |
| `-1` | 1行に1ファイルずつ表示 |
| `-d` | ディレクトリ自体の情報を表示 |

---

### cp オプション

| オプション | 説明 |
| --- | --- |
| `-r` | ディレクトリを再帰的にコピー |
| `-p` | タイムスタンプ・権限を保持 |
| `-i` | 上書き前に確認 |
| `-v` | コピー内容を表示 |
| `-u` | 更新されたファイルのみコピー |
| `-a` | `-r -p` + シンボリックリンクを保持（アーカイブモード） |

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
| `--color` | マッチ部分を色付きで表示 |

---

### find オプション

| オプション | 説明 |
| --- | --- |
| `-name` | ファイル名で検索（大文字小文字区別） |
| `-iname` | ファイル名で検索（大文字小文字区別なし） |
| `-type f` | ファイルのみ検索 |
| `-type d` | ディレクトリのみ検索 |
| `-type l` | シンボリックリンクのみ検索 |
| `-mtime N` | N 日前に更新されたファイル |
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
| `-v` | 詳細表示 |
| `-C ディレクトリ` | 展開先ディレクトリを指定 |

---

### chmod パーミッション数値早見表

| 数値 | 意味 |
| --- | --- |
| `777` | 所有者・グループ・その他 すべて読み書き実行可 |
| `755` | 所有者は読み書き実行可、グループ・その他は読み・実行可 |
| `644` | 所有者は読み書き可、グループ・その他は読み取りのみ |
| `600` | 所有者のみ読み書き可（SSH秘密鍵等に使用） |
| `700` | 所有者のみ読み書き実行可 |
| `400` | 所有者のみ読み取り可（読み取り専用） |

---

## 設定

### WSL の設定ファイル（Windows側）

`C:\Users\ユーザー名\.wslconfig` に記述する（WSL2全体の設定）。

```ini
# C:\Users\ユーザー名\.wslconfig

[wsl2]
# 割り当てメモリ（例: 8GB）
memory=8GB

# 割り当てCPUコア数
processors=4

# スワップサイズ
swap=2GB

# ローカルホストフォワーディングを有効化
localhostForwarding=true

# カーネルのカスタムパスを指定（省略可）
# kernel=C:\\path\\to\\custom\\kernel
```

変更後、WSLを再起動して反映させる。

```powershell
# PowerShell で WSL を終了・再起動
wsl --shutdown
wsl
```

---

### WSL ディストリビューションの設定ファイル

Ubuntu内の `/etc/wsl.conf` に記述する（個別ディストリビューションの設定）。

```ini
# /etc/wsl.conf

[automount]
# Windows のドライブを自動マウント
enabled = true
root = /mnt/
options = "metadata,umask=22,fmask=11"
mountFsTab = true

[network]
# ホスト名を指定
hostname = my-ubuntu

# /etc/hosts を自動生成
generateHosts = true

# /etc/resolv.conf を自動生成
generateResolvConf = true

[interop]
# Windows のコマンドを WSL から実行可能にする
enabled = true
appendWindowsPath = true

[user]
# デフォルトユーザーを指定
default = ユーザー名

[boot]
# WSL 起動時にコマンドを実行（WSL2のみ）
command = "service cron start"
```

設定後、WSLを再起動して反映させる。

```bash
# Ubuntu内から再起動（WSL2）
wsl.exe --shutdown
```

---

### .bashrc の設定例

`~/.bashrc` に記述する（ログインシェルの設定）。

```bash
# ~/.bashrc の設定例

# ---- エイリアス ----
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
# ユーザー名@ホスト名:カレントディレクトリ $ の形式
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# ---- 履歴の設定 ----
export HISTSIZE=10000
export HISTFILESIZE=20000
export HISTCONTROL=ignoredups:ignorespace
shopt -s histappend

# ---- 補完を有効化 ----
if [ -f /etc/bash_completion ]; then
  . /etc/bash_completion
fi
```

変更後に反映させる。

```bash
source ~/.bashrc
```

---

### Git の初期設定

```bash
# ユーザー名とメールアドレスを設定
git config --global user.name "あなたの名前"
git config --global user.email "your@email.com"

# デフォルトエディタを設定
git config --global core.editor vim

# デフォルトブランチ名を設定
git config --global init.defaultBranch main

# 改行コードの自動変換を無効化（Linux環境推奨）
git config --global core.autocrlf input

# 設定内容を確認
git config --global --list

# 設定ファイルをエディタで開く
git config --global --edit
```

---

### Python 環境の設定（uv 使用）

```bash
# uv をインストール
curl -LsSf https://astral.sh/uv/install.sh | sh

# uv を PATH に追加（インストール後に実行）
source ~/.bashrc

# 新しい Python プロジェクトを作成
uv init プロジェクト名

# Python のバージョンを指定して作成
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

# Python スクリプトを実行
uv run python スクリプト名.py

# インストール済みパッケージを確認
uv pip list
```

**使い方の例：**

```bash
# FastAPI プロジェクトを作成してサーバーを起動
uv init myapp
cd myapp
uv add fastapi uvicorn
uv run uvicorn main:app --reload
```

---

### vim の基本設定

`~/.vimrc` に記述する。

```vim
" ~/.vimrc

" 行番号を表示
set number

" 相対行番号を表示
set relativenumber

" タブをスペースに変換
set expandtab

" タブのスペース数
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

" カラースキームを設定
colorscheme desert

" バックスペースの動作を設定
set backspace=indent,eol,start

" クリップボードと共有
set clipboard=unnamedplus

" ステータスラインを常に表示
set laststatus=2

" コマンドモードの補完を有効化
set wildmenu
```

---

*このドキュメントは WSL (Ubuntu) のコマンドリファレンスとしてまとめたものです。各コマンドの詳細は `man コマンド名` で確認できます。*
