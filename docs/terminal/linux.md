# Linux

Linux の主要コマンドを用途別にまとめたリファレンスです。
ファイル操作・プロセス管理・ネットワーク・シェルスクリプトなど、日常的な作業から上級者向けの操作まで網羅しています。

---

## インストール・アップデート・アンインストール

=== "Debian / Ubuntu (apt)"

    ### パッケージをインストールする

    ```bash
    sudo apt install パッケージ名
    ```

    !!! note "説明"
        `apt` は Debian 系ディストリビューションの標準パッケージマネージャーです。
        インストール前に `apt update` でパッケージリストを更新することを推奨します。

    ```bash
    # パッケージリストを更新する
    sudo apt update

    # パッケージをインストールする
    sudo apt install vim

    # 複数パッケージを一括インストールする
    sudo apt install vim curl git wget

    # 確認プロンプトをスキップしてインストールする
    sudo apt install -y vim
    ```

    ---

    ### パッケージをアップデートする

    ```bash
    sudo apt upgrade
    ```

    !!! note "説明"
        インストール済みパッケージをすべて最新版に更新します。
        `dist-upgrade` はカーネルなど依存関係の変更を伴うアップデートも行います。

    ```bash
    # パッケージリスト更新 → 全パッケージアップグレード（一括実行）
    sudo apt update && sudo apt upgrade -y

    # 依存関係の変更を含む完全アップグレード
    sudo apt dist-upgrade

    # 特定パッケージだけアップグレードする
    sudo apt install --only-upgrade vim
    ```

    ---

    ### パッケージをアンインストールする

    ```bash
    sudo apt remove パッケージ名
    ```

    !!! note "説明"
        `remove` はパッケージ本体のみ削除します。設定ファイルも含めて完全に削除するには `purge` を使います。
        `autoremove` で不要な依存パッケージも一緒に削除できます。

    ```bash
    # パッケージを削除する（設定ファイルは残る）
    sudo apt remove vim

    # 設定ファイルも含めて完全削除する
    sudo apt purge vim

    # 不要な依存パッケージをまとめて削除する
    sudo apt autoremove

    # purge + autoremove を一括実行する
    sudo apt purge vim && sudo apt autoremove
    ```

=== "Red Hat / CentOS (dnf / yum)"

    ### パッケージをインストールする

    ```bash
    sudo dnf install パッケージ名
    ```

    !!! note "説明"
        `dnf` は Red Hat 系ディストリビューション（RHEL 8+、CentOS 8+、Fedora）の標準パッケージマネージャーです。
        旧バージョンでは `yum` を使います。

    ```bash
    # パッケージをインストールする
    sudo dnf install vim

    # 確認プロンプトをスキップしてインストールする
    sudo dnf install -y vim

    # 全パッケージをアップデートする
    sudo dnf update -y

    # パッケージをアンインストールする
    sudo dnf remove vim
    ```

=== "Arch Linux (pacman)"

    ### パッケージをインストールする

    ```bash
    sudo pacman -S パッケージ名
    ```

    !!! note "説明"
        `pacman` は Arch Linux のパッケージマネージャーです。
        `-S` でインストール、`-R` で削除、`-Syu` でシステム全体の更新を行います。

    ```bash
    # パッケージリストを更新してインストールする
    sudo pacman -Sy vim

    # システム全体をアップデートする
    sudo pacman -Syu

    # パッケージを削除する
    sudo pacman -R vim

    # 依存パッケージも含めて削除する
    sudo pacman -Rs vim
    ```

---

## 基本コマンド

### ファイル・ディレクトリを操作する

#### ディレクトリを表示する（ls）

```bash
ls [オプション] [パス]
```

!!! note "説明"
    カレントディレクトリまたは指定パスのファイル一覧を表示します。
    `-l` で詳細表示、`-a` で隠しファイルも表示します。

```bash
# カレントディレクトリのファイルを表示する
ls

# 詳細情報（パーミッション・サイズ・日時）を表示する
ls -l

# 隠しファイルも含めて表示する
ls -la

# ファイルサイズを人間が読みやすい形式で表示する
ls -lh

# サイズ順（降順）で表示する
ls -lhS

# 更新日時順（新しい順）で表示する
ls -lt
```

---

#### ディレクトリを移動する（cd）

```bash
cd [パス]
```

!!! note "説明"
    カレントディレクトリを変更します。
    `~` はホームディレクトリ、`..` は一つ上のディレクトリを表します。

```bash
# ホームディレクトリに移動する
cd ~
cd        # 引数なしも同じ

# 一つ上のディレクトリに移動する
cd ..

# 二つ上のディレクトリに移動する
cd ../..

# 直前にいたディレクトリに戻る
cd -

# 絶対パスで移動する
cd /var/log
```

---

#### ディレクトリを作成する（mkdir）

```bash
mkdir [オプション] ディレクトリ名
```

!!! note "説明"
    新しいディレクトリを作成します。
    `-p` で中間ディレクトリも含めて一括作成できます。

```bash
# ディレクトリを作成する
mkdir mydir

# ネストしたディレクトリを一括作成する
mkdir -p parent/child/grandchild

# 複数ディレクトリを一括作成する
mkdir dir1 dir2 dir3

# パーミッションを指定して作成する
mkdir -m 755 mydir
```

---

#### ファイル・ディレクトリをコピーする（cp）

```bash
cp [オプション] コピー元 コピー先
```

!!! note "説明"
    ファイルやディレクトリをコピーします。
    ディレクトリをコピーする際は `-r`（再帰的）オプションが必要です。

```bash
# ファイルをコピーする
cp file.txt copy.txt

# 別ディレクトリにコピーする
cp file.txt /tmp/

# ディレクトリごとコピーする（再帰的）
cp -r mydir/ /tmp/mydir/

# 上書き前に確認プロンプトを出す
cp -i file.txt /tmp/

# タイムスタンプ・パーミッションを保持してコピーする
cp -p file.txt /tmp/

# バックアップを作りながらコピーする
cp --backup=numbered file.txt /tmp/
```

---

#### ファイル・ディレクトリを移動・名前変更する（mv）

```bash
mv [オプション] 移動元 移動先
```

!!! note "説明"
    ファイルやディレクトリを移動します。同じディレクトリ内で使うと名前変更になります。

```bash
# ファイル名を変更する
mv old.txt new.txt

# ファイルを別ディレクトリに移動する
mv file.txt /tmp/

# ディレクトリを移動する
mv mydir/ /var/

# 上書き前に確認プロンプトを出す
mv -i file.txt /tmp/

# 移動内容を表示する（verboseモード）
mv -v file.txt /tmp/
```

---

#### ファイル・ディレクトリを削除する（rm）

```bash
rm [オプション] ファイル名
```

!!! note "説明"
    ファイルやディレクトリを削除します。削除したファイルはゴミ箱に入らず、復元できません。
    ディレクトリを削除するには `-r` オプションが必要です。

!!! warning "注意"
    `rm -rf /` や `rm -rf *` は全ファイルを削除するため、実行前に必ずパスを確認してください。

```bash
# ファイルを削除する
rm file.txt

# 削除前に確認プロンプトを出す
rm -i file.txt

# ディレクトリを再帰的に削除する
rm -r mydir/

# 確認なしで強制削除する
rm -rf mydir/

# ワイルドカードで一括削除する
rm *.log
```

---

#### カレントディレクトリを表示する（pwd）

```bash
pwd
```

!!! note "説明"
    現在いるディレクトリの絶対パスを表示します。

```bash
# 現在のパスを表示する
pwd

# シンボリックリンクを解決して実パスを表示する
pwd -P
```

---

### ファイル内容を表示する

#### ファイルの内容を表示する（cat）

```bash
cat [オプション] ファイル名
```

!!! note "説明"
    ファイルの内容を標準出力に表示します。複数ファイルを連結して表示することもできます。

```bash
# ファイルの内容を表示する
cat file.txt

# 行番号付きで表示する
cat -n file.txt

# 複数ファイルを連結して表示する
cat file1.txt file2.txt

# ファイルを作成しながら内容を入力する（Ctrl+D で終了）
cat > newfile.txt

# ファイルに追記する
cat >> file.txt
```

---

#### ファイルをページ単位で表示する（less）

```bash
less [オプション] ファイル名
```

!!! note "説明"
    長いファイルをページ単位でスクロールしながら閲覧します。
    `q` で終了、`/` でキーワード検索、`G` で末尾に移動します。

```bash
# ファイルをページ表示する
less /var/log/syslog

# 行番号付きで表示する
less -N file.txt

# 末尾から表示する（ログ確認に便利）
less +G /var/log/syslog

# 検索ハイライトを有効にして開く
less -i file.txt
```

---

#### ファイルの先頭・末尾を表示する（head / tail）

```bash
head [オプション] ファイル名
tail [オプション] ファイル名
```

!!! note "説明"
    `head` はファイルの先頭行、`tail` は末尾行を表示します。
    `tail -f` でリアルタイムにログを監視できます。

```bash
# 先頭10行を表示する（デフォルト）
head file.txt

# 先頭20行を表示する
head -n 20 file.txt

# 末尾10行を表示する（デフォルト）
tail file.txt

# 末尾30行を表示する
tail -n 30 file.txt

# ログをリアルタイムで監視する
tail -f /var/log/syslog

# 複数ファイルをリアルタイム監視する
tail -f /var/log/syslog /var/log/auth.log
```

---

## よく使うコマンド

### テキストを検索・処理する

#### テキストを検索する（grep）

```bash
grep [オプション] パターン [ファイル名]
```

!!! note "説明"
    ファイルや標準入力からパターンに一致する行を検索します。
    正規表現に対応しており、パイプと組み合わせて強力なフィルタリングが可能です。

```bash
# ファイルからキーワードを検索する
grep "error" /var/log/syslog

# 大文字小文字を区別しない検索
grep -i "error" /var/log/syslog

# 一致しない行を表示する（否定）
grep -v "debug" /var/log/syslog

# 行番号も表示する
grep -n "error" file.txt

# ディレクトリを再帰的に検索する
grep -r "TODO" ./src/

# 一致した件数を表示する
grep -c "error" /var/log/syslog

# 前後の行も含めて表示する（前後3行）
grep -C 3 "error" /var/log/syslog

# 正規表現で検索する
grep -E "^[0-9]{4}-[0-9]{2}" file.txt

# マッチした部分だけ表示する
grep -o "error[^:]*" file.txt

# パイプと組み合わせる
cat /var/log/syslog | grep "error" | tail -20
```

---

#### テキストを加工する（sed）

```bash
sed [オプション] 'コマンド' ファイル名
```

!!! note "説明"
    ストリームエディタとして、テキストの置換・削除・挿入を行います。
    `-i` でファイルを直接編集できます。

```bash
# 最初の一致を置換する
sed 's/old/new/' file.txt

# 全ての一致を置換する（グローバル）
sed 's/old/new/g' file.txt

# 大文字小文字を区別しない置換
sed 's/old/new/gi' file.txt

# ファイルを直接書き換える
sed -i 's/old/new/g' file.txt

# バックアップを作りながらファイルを書き換える
sed -i.bak 's/old/new/g' file.txt

# 特定の行を削除する（5行目を削除）
sed '5d' file.txt

# 空行を削除する
sed '/^$/d' file.txt

# 特定行の後に行を追加する（3行目の後）
sed '3a\追加する行' file.txt

# 行頭にテキストを追加する
sed 's/^/PREFIX: /' file.txt
```

---

#### テキストを集計・処理する（awk）

```bash
awk [オプション] 'プログラム' ファイル名
```

!!! note "説明"
    列単位のテキスト処理に特化した強力なツールです。
    CSV・ログ解析・集計処理などに広く使われます。

```bash
# 2列目を表示する
awk '{print $2}' file.txt

# カンマ区切りのCSVから3列目を表示する
awk -F',' '{print $3}' data.csv

# 条件に一致した行の特定列を表示する
awk '$3 > 100 {print $1, $3}' data.txt

# 合計を計算する
awk '{sum += $1} END {print sum}' numbers.txt

# 行数をカウントする
awk 'END {print NR}' file.txt

# ヘッダー行を除いて処理する
awk 'NR > 1 {print $1}' data.csv

# 特定パターンを含む行を表示する
awk '/error/ {print NR, $0}' /var/log/syslog
```

---

### ファイルを検索する

#### ファイルを検索する（find）

```bash
find [検索パス] [条件] [アクション]
```

!!! note "説明"
    ディレクトリツリーを再帰的に検索します。
    ファイル名・種類・更新日時・パーミッションなど多様な条件で絞り込めます。

```bash
# カレントディレクトリ以下で名前で検索する
find . -name "*.txt"

# 大文字小文字を区別しない名前検索
find . -iname "readme*"

# ファイルのみ検索する
find . -type f -name "*.log"

# ディレクトリのみ検索する
find . -type d -name "node_modules"

# 7日以内に更新されたファイルを検索する
find . -mtime -7

# 100MB以上のファイルを検索する
find / -size +100M

# 特定のパーミッションを持つファイルを検索する
find . -perm 755

# 検索結果に対してコマンドを実行する
find . -name "*.log" -exec rm {} \;

# 検索結果を確認しながら削除する（安全）
find . -name "*.log" -ok rm {} \;

# 特定ディレクトリを除外して検索する
find . -not -path "*/node_modules/*" -name "*.js"
```

---

### 圧縮・解凍する

#### tarアーカイブを作成・展開する（tar）

```bash
tar [オプション] アーカイブ名 [ファイル名]
```

!!! note "説明"
    ファイルのアーカイブ（圧縮・展開）を行います。
    `.tar.gz` は gzip 圧縮、`.tar.bz2` は bzip2 圧縮です。

```bash
# gzip圧縮アーカイブを作成する
tar -czf archive.tar.gz mydir/

# bzip2圧縮アーカイブを作成する
tar -cjf archive.tar.bz2 mydir/

# xz圧縮アーカイブを作成する（最高圧縮率）
tar -cJf archive.tar.xz mydir/

# アーカイブを展開する（形式を自動判別）
tar -xf archive.tar.gz

# 指定ディレクトリに展開する
tar -xf archive.tar.gz -C /tmp/

# アーカイブの内容一覧を表示する
tar -tf archive.tar.gz

# 進捗を表示しながら展開する（verbose）
tar -xvf archive.tar.gz

# 特定ファイルだけ展開する
tar -xf archive.tar.gz mydir/file.txt
```

---

### ディスク・容量を確認する

#### ディスク使用量を確認する（df）

```bash
df [オプション] [パス]
```

!!! note "説明"
    ファイルシステムのディスク使用状況を表示します。
    `-h` で GB/MB などの人間が読みやすい単位で表示します。

```bash
# 全マウントポイントのディスク使用量を表示する
df -h

# 特定パスのディスク使用量を表示する
df -h /home

# iノード使用量を表示する
df -i

# ファイルシステムタイプも表示する
df -hT
```

---

#### ディレクトリのサイズを確認する（du）

```bash
du [オプション] [パス]
```

!!! note "説明"
    ディレクトリやファイルのディスク使用量を表示します。
    `df` がファイルシステム全体を見るのに対し、`du` は個別のディレクトリサイズを調べるのに使います。

```bash
# カレントディレクトリのサイズを表示する
du -sh .

# サブディレクトリのサイズ一覧を表示する
du -sh */

# 深さ1レベルまでのサイズを表示する
du -h --max-depth=1

# サイズ順にソートして表示する
du -h /var | sort -rh | head -20

# 特定ディレクトリの合計サイズを表示する
du -sh /home/user/
```

---

## 上級者向けコマンド

### プロセスを管理する

#### プロセスを表示する（ps）

```bash
ps [オプション]
```

!!! note "説明"
    実行中のプロセス一覧を表示します。
    `aux` オプションで全ユーザーのプロセスを詳細表示します。

```bash
# 全プロセスを詳細表示する（よく使う形式）
ps aux

# プロセスをツリー形式で表示する
ps auxf

# 特定プロセスを検索する
ps aux | grep nginx

# 特定ユーザーのプロセスを表示する
ps -u username

# PIDと名前だけ表示する
ps -eo pid,comm

# CPUとメモリ使用率も表示する
ps -eo pid,comm,%cpu,%mem --sort=-%cpu
```

---

#### プロセスをリアルタイム監視する（top / htop）

```bash
top
htop
```

!!! note "説明"
    CPU・メモリ使用率などをリアルタイムで表示します。
    `htop` は `top` の改良版で、カラー表示・マウス操作に対応しています（別途インストールが必要）。

```bash
# リアルタイムでプロセスを監視する
top

# 更新間隔を2秒にして起動する
top -d 2

# 特定ユーザーのプロセスのみ表示する
top -u username

# htop を起動する（要インストール: sudo apt install htop）
htop

# 特定プロセスのみ表示する
htop -p $(pgrep nginx)
```

---

#### プロセスにシグナルを送る（kill / killall）

```bash
kill [オプション] PID
killall プロセス名
```

!!! note "説明"
    プロセスを終了させます。`kill -9` は強制終了です。
    `pgrep` でプロセス名からPIDを調べられます。

```bash
# PIDを指定してプロセスを終了する
kill 1234

# 強制終了する（応答しない場合）
kill -9 1234

# プロセス名を指定して終了する
killall nginx

# プロセス名を指定して強制終了する
killall -9 firefox

# PIDをまとめて取得して強制終了する
kill -9 $(pgrep nginx)

# シグナル一覧を表示する
kill -l
```

---

### システム情報を確認する

#### システム情報を表示する（uname / hostnamectl）

```bash
uname [オプション]
hostnamectl
```

!!! note "説明"
    カーネル・OS・アーキテクチャなどのシステム情報を表示します。

```bash
# カーネルバージョンを表示する
uname -r

# 全システム情報を表示する
uname -a

# OSのホスト名・バージョンを詳しく表示する
hostnamectl

# OS種別を表示する
uname -s

# CPUアーキテクチャを表示する
uname -m
```

---

#### ハードウェア情報を表示する（lscpu / lsmem / lspci）

```bash
lscpu
lsmem
lspci
```

!!! note "説明"
    CPU・メモリ・PCIデバイスなどのハードウェア情報を表示します。

```bash
# CPU情報を表示する
lscpu

# コア数だけ表示する
lscpu | grep "CPU(s):"

# メモリ情報を表示する
lsmem

# メモリサイズを確認する
free -h

# PCIデバイス一覧を表示する
lspci

# USBデバイス一覧を表示する
lsusb

# ブロックデバイス一覧を表示する
lsblk
```

---

### ネットワークを操作する

#### ネットワーク設定を確認する（ip / ifconfig）

```bash
ip [オブジェクト] [コマンド]
ifconfig [インターフェース]
```

!!! note "説明"
    ネットワークインターフェースの設定・確認を行います。
    現在は `ifconfig` より `ip` コマンドが標準です。

```bash
# ネットワークインターフェースとIPアドレスを表示する
ip addr show
ip a    # 短縮形

# ルーティングテーブルを表示する
ip route show
ip r    # 短縮形

# 特定インターフェースを有効にする
sudo ip link set eth0 up

# 一時的にIPアドレスを設定する
sudo ip addr add 192.168.1.100/24 dev eth0

# 古い方法（ifconfigが使える環境）
ifconfig

# 特定インターフェースのみ表示する
ifconfig eth0
```

---

#### 通信を確認する（ping / traceroute / curl）

```bash
ping [オプション] ホスト名またはIPアドレス
traceroute ホスト名またはIPアドレス
curl [オプション] URL
```

!!! note "説明"
    ネットワークの疎通確認・ルート追跡・HTTP通信のテストを行います。

```bash
# 疎通確認をする
ping google.com

# 4回だけpingを送る
ping -c 4 google.com

# パケットサイズを指定してpingを送る
ping -s 1024 google.com

# ルートを追跡する
traceroute google.com

# HTTPリクエストを送信する
curl https://example.com

# レスポンスをファイルに保存する
curl -o output.html https://example.com

# リダイレクトを追いかける
curl -L https://example.com

# HTTPヘッダーを表示する
curl -I https://example.com

# POSTリクエストを送信する
curl -X POST -d '{"key":"value"}' -H "Content-Type: application/json" https://api.example.com

# 進捗を表示してファイルをダウンロードする
curl -# -O https://example.com/file.zip
```

---

#### ポートを確認する（ss / netstat）

```bash
ss [オプション]
netstat [オプション]
```

!!! note "説明"
    開いているポートやソケットの状態を確認します。
    現在は `netstat` より `ss` コマンドが標準です。

```bash
# 全ポートの使用状況を表示する
ss -tuln

# 待ち受けポートを表示する
ss -tlnp

# 特定ポートを確認する
ss -tlnp | grep 80

# TCPの接続状況を表示する
ss -tn

# 古い方法（netstatが使える環境）
netstat -tuln

# 使用中のポートとプロセスを表示する
netstat -tlnp
```

---

### ファイルのパーミッションを管理する

#### パーミッションを変更する（chmod）

```bash
chmod [オプション] モード ファイル名
```

!!! note "説明"
    ファイル・ディレクトリのパーミッション（読み書き実行権限）を変更します。
    数値表記（755など）とシンボル表記（u+x など）の両方が使えます。

```bash
# 全ユーザーに実行権限を付与する
chmod +x script.sh

# 所有者に全権限、他は読み取りのみ
chmod 744 file.txt

# Webディレクトリの標準パーミッション
chmod 755 /var/www/html

# 機密ファイルを所有者のみ読み書き可能にする
chmod 600 ~/.ssh/id_rsa

# ディレクトリ以下を再帰的に変更する
chmod -R 755 mydir/

# グループに書き込み権限を追加する
chmod g+w file.txt

# 他人の読み取り権限を削除する
chmod o-r file.txt
```

---

#### 所有者を変更する（chown）

```bash
chown [オプション] ユーザー[:グループ] ファイル名
```

!!! note "説明"
    ファイル・ディレクトリの所有者とグループを変更します。

```bash
# 所有者を変更する
sudo chown username file.txt

# 所有者とグループを同時に変更する
sudo chown username:groupname file.txt

# グループのみ変更する
sudo chown :groupname file.txt

# ディレクトリ以下を再帰的に変更する
sudo chown -R username:groupname /var/www/html

# シンボリックリンクも変更する
sudo chown -h username symlink
```

---

## 知ってると便利なコマンド

### シェルを使いこなす

#### コマンドの履歴を使う（history）

```bash
history [オプション]
```

!!! note "説明"
    実行したコマンドの履歴を表示します。
    `!!` で直前のコマンド、`!番号` で指定番号のコマンドを再実行できます。

```bash
# 履歴を表示する
history

# 直近20件だけ表示する
history 20

# 特定キーワードを含む履歴を検索する
history | grep "apt install"

# 直前のコマンドを再実行する
!!

# 特定番号のコマンドを再実行する（150番の場合）
!150

# 直前のコマンドを sudo で再実行する
sudo !!

# 履歴をクリアする
history -c
```

---

#### コマンドのパスを調べる（which / where / type）

```bash
which コマンド名
whereis コマンド名
type コマンド名
```

!!! note "説明"
    コマンドのインストールパスを調べます。
    複数バージョンがある場合の確認や、エイリアスとの判別に便利です。

```bash
# コマンドのパスを表示する
which python3

# マニュアルも含めた関連ファイルを表示する
whereis python3

# コマンドの種類（エイリアス・関数・組み込みなど）を表示する
type ls

# 全候補を表示する
which -a python
```

---

#### 標準出力をファイルとパイプ両方に送る（tee）

```bash
コマンド | tee [オプション] ファイル名
```

!!! note "説明"
    コマンドの出力をファイルに保存しながら同時に画面にも表示します。
    ログを残しながら作業したいときに便利です。

```bash
# 出力をファイルに保存しつつ画面にも表示する
ls -la | tee output.txt

# ファイルに追記する（上書きしない）
ls -la | tee -a output.txt

# 複数ファイルに同時に保存する
echo "test" | tee file1.txt file2.txt

# sudo コマンドの出力をroot所有ファイルに書き込む
echo "設定" | sudo tee /etc/myconfig.conf
```

---

#### 変数・エイリアスを管理する

```bash
# 変数の設定
export 変数名=値

# エイリアスの設定
alias 短縮名='コマンド'
```

!!! note "説明"
    `export` で環境変数を設定、`alias` でコマンドの短縮形を定義します。
    永続化するには `~/.bashrc` または `~/.zshrc` に記述します。

```bash
# 環境変数を設定する
export MY_VAR="hello"

# 変数を表示する
echo $MY_VAR

# 全環境変数を表示する
env
printenv

# エイリアスを定義する
alias ll='ls -la'
alias gs='git status'
alias ..='cd ..'

# 定義済みエイリアスを一覧表示する
alias

# エイリアスを削除する
unalias ll

# ~/.bashrc に永続保存する
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc
```

---

#### テキストを並び替え・重複削除する（sort / uniq）

```bash
sort [オプション] [ファイル名]
uniq [オプション] [ファイル名]
```

!!! note "説明"
    `sort` でテキスト行を並び替え、`uniq` で重複行を削除・カウントします。
    組み合わせてデータ集計によく使われます。

```bash
# アルファベット順にソートする
sort file.txt

# 逆順にソートする
sort -r file.txt

# 数値としてソートする
sort -n numbers.txt

# 数値の降順にソートする
sort -rn numbers.txt

# 2列目でソートする
sort -k2 data.txt

# 重複行を削除する（事前にsortが必要）
sort file.txt | uniq

# 重複行のみ表示する
sort file.txt | uniq -d

# 各行の出現回数を表示する
sort file.txt | uniq -c | sort -rn
```

---

#### ファイルを比較する（diff）

```bash
diff [オプション] ファイル1 ファイル2
```

!!! note "説明"
    2つのファイルの差分を表示します。
    `-u` で unified 形式（git diff に近い見た目）になります。

```bash
# 2ファイルの差分を表示する
diff file1.txt file2.txt

# unified形式で表示する（前後の行も表示）
diff -u file1.txt file2.txt

# ディレクトリを再帰的に比較する
diff -r dir1/ dir2/

# 差分をパッチファイルに保存する
diff -u original.txt modified.txt > patch.diff

# パッチを適用する
patch original.txt < patch.diff

# 空白の差異を無視する
diff -w file1.txt file2.txt
```

---

#### 文字数・行数・単語数を数える（wc）

```bash
wc [オプション] [ファイル名]
```

!!! note "説明"
    ファイルや標準入力の行数・単語数・バイト数を表示します。
    パイプと組み合わせて件数のカウントに使うことが多いです。

```bash
# 行数・単語数・バイト数を表示する
wc file.txt

# 行数だけ表示する
wc -l file.txt

# 単語数だけ表示する
wc -w file.txt

# 文字数だけ表示する
wc -m file.txt

# パイプと組み合わせてプロセス数を数える
ps aux | wc -l

# ファイル数を数える
ls | wc -l
```

---

### ジョブ・バックグラウンドを管理する

#### ジョブをバックグラウンドで実行する

```bash
コマンド &
nohup コマンド &
```

!!! note "説明"
    `&` でバックグラウンド実行、`nohup` でターミナルを閉じても処理を継続します。
    `jobs` で実行中のジョブを確認、`fg` でフォアグラウンドに戻せます。

```bash
# バックグラウンドで実行する
sleep 100 &

# 実行中のジョブを表示する
jobs

# ジョブをフォアグラウンドに戻す（ジョブ番号1の場合）
fg %1

# ジョブをバックグラウンドに送る（フォアグラウンド実行中に Ctrl+Z → bg）
bg %1

# ターミナルを閉じても実行継続する
nohup long_process.sh &

# nohupの出力を指定ファイルに保存する
nohup long_process.sh > output.log 2>&1 &

# 実行中のnohupプロセスを確認する
ps aux | grep long_process.sh
```

---

## オプション一覧

### ls オプション

| オプション | 説明 |
| --- | --- |
| `-l` | 詳細リスト形式で表示する |
| `-a` | ドットファイル（隠しファイル）も表示する |
| `-h` | ファイルサイズを人間が読みやすい単位で表示する |
| `-S` | ファイルサイズ順にソートする |
| `-t` | 更新日時順にソートする |
| `-r` | ソート順を逆にする |
| `-R` | ディレクトリを再帰的に表示する |
| `--color` | カラー表示を有効にする |

---

### grep オプション

| オプション | 説明 |
| --- | --- |
| `-i` | 大文字小文字を区別しない |
| `-v` | パターンに一致しない行を表示する |
| `-n` | 行番号を表示する |
| `-r` | ディレクトリを再帰的に検索する |
| `-l` | 一致したファイル名のみ表示する |
| `-c` | 一致した行数を表示する |
| `-A n` | 一致行の後n行も表示する |
| `-B n` | 一致行の前n行も表示する |
| `-C n` | 一致行の前後n行も表示する |
| `-E` | 拡張正規表現を使う |
| `-o` | 一致した部分のみ表示する |
| `-w` | 単語全体に一致する場合のみ表示する |

---

### find オプション

| オプション | 説明 |
| --- | --- |
| `-name` | ファイル名で検索する（大文字小文字を区別） |
| `-iname` | ファイル名で検索する（大文字小文字を区別しない） |
| `-type f` | ファイルのみ検索する |
| `-type d` | ディレクトリのみ検索する |
| `-mtime -n` | n日以内に更新されたファイルを検索する |
| `-mtime +n` | n日より前に更新されたファイルを検索する |
| `-size +n` | 指定サイズより大きいファイルを検索する |
| `-perm` | パーミッションで検索する |
| `-exec` | 検索結果に対してコマンドを実行する |
| `-not` | 条件を否定する |
| `-maxdepth` | 検索する深さを制限する |

---

### chmod 数値表記

| 値 | パーミッション |
| --- | --- |
| `777` | 全ユーザーに読み・書き・実行 |
| `755` | 所有者は全権限、他は読み・実行 |
| `644` | 所有者は読み・書き、他は読みのみ |
| `600` | 所有者のみ読み・書き |
| `400` | 所有者のみ読みのみ |
| `chmod +x` | 全員に実行権限を追加 |
| `chmod u+x` | 所有者にのみ実行権限を追加 |

---

## 設定

### シェル設定ファイル

#### .bashrc と .bash_profile の違い

!!! note "説明"
    - `.bashrc`: インタラクティブシェル起動時に読み込まれる（毎回のターミナル起動）
    - `.bash_profile`: ログインシェル起動時のみ読み込まれる（SSH ログイン時など）
    - `.zshrc`: zsh を使っている場合の設定ファイル

```bash
# ~/.bashrc を編集する
vim ~/.bashrc

# 設定を即時反映する（再起動不要）
source ~/.bashrc

# PATH にディレクトリを追加する（.bashrc に記述）
export PATH="$HOME/.local/bin:$PATH"

# エイリアスを永続化する（.bashrc に追記）
echo "alias ll='ls -la --color=auto'" >> ~/.bashrc

# プロンプトをカスタマイズする（.bashrc に記述）
export PS1='\u@\h:\w\$ '
```

---

### シェルオプションを設定する（set / shopt）

```bash
set [オプション]
shopt [オプション] オプション名
```

!!! note "説明"
    シェルの動作をカスタマイズします。
    `set -e` でエラー時にスクリプトを停止させるなど、シェルスクリプト作成時に重要です。

```bash
# エラー時にスクリプトを停止する（シェルスクリプトの先頭に記述）
set -e

# 未定義変数を使用したらエラーにする
set -u

# パイプのエラーも検知する
set -o pipefail

# デバッグモード（実行コマンドを表示する）
set -x

# シェルスクリプトの推奨先頭設定
set -euo pipefail

# globstarを有効にする（**でネスト検索）
shopt -s globstar

# 設定一覧を表示する
set -o
shopt
```

---

### ログローテーションを設定する（logrotate）

```bash
logrotate [オプション] 設定ファイル
```

!!! note "説明"
    ログファイルの自動ローテーション（圧縮・削除）を設定します。
    設定ファイルは `/etc/logrotate.d/` に配置します。

```bash
# 設定をテスト実行する（実際には変更しない）
logrotate --debug /etc/logrotate.conf

# 強制的にローテーションを実行する
logrotate --force /etc/logrotate.conf

# カスタム設定ファイルの例（/etc/logrotate.d/myapp）
# /var/log/myapp/*.log {
#     daily
#     rotate 7
#     compress
#     delaycompress
#     missingok
#     notifempty
#     create 644 www-data www-data
# }
```

---

## 頻出コマンド早引き

```bash
# ---- ファイル操作 ----
ls -la                          # 詳細一覧（隠しファイルも）
cp -r src/ dst/                 # ディレクトリごとコピー
mv old.txt new.txt              # ファイルを移動・名前変更
rm -rf mydir/                   # ディレクトリを強制削除
mkdir -p a/b/c                  # ネストディレクトリを一括作成

# ---- ファイル内容 ----
cat file.txt                    # ファイルを表示
less file.txt                   # ページ表示（q で終了）
tail -f /var/log/syslog         # ログをリアルタイム監視
head -n 20 file.txt             # 先頭20行を表示

# ---- 検索 ----
grep -rn "keyword" ./src/       # ディレクトリ内を再帰検索（行番号付き）
find . -name "*.log" -mtime -7  # 7日以内の.logファイルを検索
find . -type f -size +100M      # 100MB超のファイルを検索

# ---- テキスト処理 ----
sed -i 's/old/new/g' file.txt   # ファイル内の文字列を一括置換
awk '{print $2}' file.txt       # 2列目を抽出
sort file.txt | uniq -c | sort -rn  # 重複を数えて頻度順に表示

# ---- プロセス ----
ps aux | grep nginx             # プロセスを検索
kill -9 $(pgrep nginx)          # プロセスを強制終了
top                             # プロセスをリアルタイム監視

# ---- ネットワーク ----
ping -c 4 google.com            # 疎通確認（4回）
curl -I https://example.com     # HTTPヘッダーを確認
ss -tlnp                        # 待ち受けポートを確認

# ---- ディスク ----
df -h                           # ディスク使用量を確認
du -sh */                       # サブディレクトリのサイズを確認
du -h --max-depth=1 | sort -rh  # サイズ順にディレクトリ一覧

# ---- 権限 ----
chmod 755 script.sh             # 実行権限を付与
chmod +x script.sh              # 全員に実行権限を付与
sudo chown -R user:group /path  # 所有者を再帰的に変更

# ---- 圧縮・解凍 ----
tar -czf archive.tar.gz dir/    # gzip圧縮アーカイブを作成
tar -xf archive.tar.gz          # アーカイブを展開
tar -tf archive.tar.gz          # アーカイブの内容を確認

# ---- パッケージ管理（apt） ----
sudo apt update && sudo apt upgrade -y  # 一括アップデート
sudo apt install パッケージ名           # パッケージをインストール
sudo apt purge パッケージ名             # 設定ごと削除

# ---- システム情報 ----
uname -a                        # カーネル情報を表示
free -h                         # メモリ使用量を表示
lscpu                           # CPU情報を表示
```

---

!!! info "公式ドキュメント・参考リソース"
    - [Linux man pages online](https://man7.org/linux/man-pages/)
    - [GNU Coreutils ドキュメント](https://www.gnu.org/software/coreutils/manual/)
    - [The Linux Command Line（無料の書籍）](https://linuxcommand.org/tlcl.php)
    - [explainshell.com（コマンドを視覚的に解説）](https://explainshell.com/)
    - `man コマンド名` でターミナルからマニュアルを参照できます
