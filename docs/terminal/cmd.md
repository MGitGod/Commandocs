# Windows CMD

Windows に標準搭載されているコマンドプロンプト（`cmd.exe`）のコマンドをまとめたリファレンスです。
ファイル操作・ネットワーク診断・バッチ処理など、日常の作業から上級者向けの管理作業まで網羅しています。

---

## 起動・終了・バージョン確認

### コマンドプロンプトを起動する

```cmd
cmd
```

!!! note "起動方法"
    `Win + R` キーを押して「ファイル名を指定して実行」ダイアログに `cmd` と入力するか、スタートメニューで「cmd」と検索して起動します。

    ```cmd
    REM 管理者権限で起動（右クリック → 「管理者として実行」が基本）
    REM 別ウィンドウで新しいコマンドプロンプトを開く
    start cmd

    REM コマンドを実行して即閉じる
    cmd /c dir C:\Users

    REM コマンドを実行してウィンドウを残す
    cmd /k ipconfig
    ```

---

### バージョンを確認する

```cmd
ver
```

!!! note "Windows のバージョンを表示する"

    ```cmd
    REM コマンドプロンプト／Windows のバージョンを表示
    ver

    REM 詳細なシステム情報を表示
    winver
    ```

---

### コマンドプロンプトを終了する

```cmd
exit
```

!!! note "セッションを終了する"

    ```cmd
    REM コマンドプロンプトを閉じる
    exit

    REM 終了コードを指定して閉じる（バッチスクリプト内で使用）
    exit /b 0
    ```

---

## ファイル・ディレクトリ操作

### ディレクトリ内容を一覧表示する

```cmd
dir [パス] [オプション]
```

!!! note "ファイルやフォルダの一覧を確認する"

    ```cmd
    REM カレントディレクトリの一覧表示
    dir

    REM 特定のパスの一覧表示
    dir C:\Users\username\Documents

    REM サブディレクトリも含めて再帰的に表示
    dir /s

    REM 隠しファイルも含めて表示
    dir /a

    REM ファイルのみ表示（ディレクトリを除く）
    dir /a-d

    REM 更新日時でソートして表示
    dir /o:d

    REM ファイルサイズでソートして表示（降順）
    dir /o:-s

    REM ワイルドカードで .txt ファイルのみ表示
    dir *.txt

    REM シンプルなファイル名のみ表示（幅広表示）
    dir /w
    ```

---

### カレントディレクトリを移動する

```cmd
cd [パス]
```

!!! note "ディレクトリ間を移動する"

    ```cmd
    REM ホームディレクトリに移動
    cd %USERPROFILE%

    REM 1つ上のディレクトリへ移動
    cd ..

    REM 2つ上のディレクトリへ移動
    cd ..\..

    REM 絶対パスで移動（同じドライブ内）
    cd C:\Users\username\Desktop

    REM 別のドライブに移動（まずドライブを切り替える）
    D:
    cd D:\Projects

    REM パスにスペースが含まれる場合はダブルクォートで囲む
    cd "C:\Program Files\MyApp"
    ```

---

### ディレクトリを作成する

```cmd
mkdir [ディレクトリ名]
```

!!! note "フォルダを作成する"

    ```cmd
    REM 単一ディレクトリを作成
    mkdir MyFolder

    REM 省略形
    md MyFolder

    REM 複数のディレクトリを一度に作成
    mkdir folderA folderB folderC

    REM 入れ子のディレクトリを一度に作成
    mkdir parent\child\grandchild
    ```

---

### ディレクトリを削除する

```cmd
rmdir [ディレクトリ名] [オプション]
```

!!! note "フォルダを削除する"

    ```cmd
    REM 空のディレクトリを削除
    rmdir MyFolder

    REM 省略形
    rd MyFolder

    REM 中身があるディレクトリも強制削除（確認なし）
    rmdir /s /q MyFolder
    ```

---

### ファイルをコピーする

```cmd
copy [コピー元] [コピー先]
```

!!! note "ファイルを複製する"

    ```cmd
    REM ファイルを別のフォルダにコピー
    copy file.txt D:\Backup\

    REM 名前を変えてコピー
    copy file.txt file_backup.txt

    REM 複数ファイルをまとめてコピー（ワイルドカード）
    copy *.txt D:\Backup\

    REM 上書き確認なしでコピー（/y オプション）
    copy /y file.txt D:\Backup\

    REM テキストファイルを連結してコピー
    copy file1.txt + file2.txt merged.txt
    ```

---

### ファイルを移動・名前変更する

```cmd
move [移動元] [移動先]
```

!!! note "ファイルを移動または名前変更する"

    ```cmd
    REM ファイルを別フォルダへ移動
    move report.txt D:\Archive\

    REM ファイル名を変更する
    move oldname.txt newname.txt

    REM 複数ファイルを移動（ワイルドカード）
    move *.log D:\Logs\

    REM 上書き確認なしで移動
    move /y file.txt D:\Archive\
    ```

---

### ファイルを削除する

```cmd
del [ファイル名] [オプション]
```

!!! note "ファイルを削除する"

    ```cmd
    REM ファイルを削除（確認あり）
    del file.txt

    REM 確認なしで強制削除
    del /q file.txt

    REM ワイルドカードで一括削除
    del *.tmp

    REM サブディレクトリ内のファイルも一括削除
    del /s /q *.log

    REM 読み取り専用ファイルも強制削除
    del /f file.txt
    ```

---

### ファイル・フォルダの名前を変更する

```cmd
ren [旧名] [新名]
```

!!! note "ファイルまたはフォルダの名前を変更する"

    ```cmd
    REM ファイル名を変更
    ren oldfile.txt newfile.txt

    REM 拡張子を一括変換（同フォルダ内）
    ren *.txt *.bak

    REM フォルダ名を変更
    ren OldFolder NewFolder
    ```

---

### ファイルの内容を表示する

```cmd
type [ファイル名]
```

!!! note "テキストファイルの内容を確認する"

    ```cmd
    REM ファイルの内容を表示
    type readme.txt

    REM 長いファイルをページ単位で表示
    type readme.txt | more

    REM 複数ファイルを連続表示
    type file1.txt file2.txt
    ```

---

### ディレクトリツリーを表示する

```cmd
tree [パス] [オプション]
```

!!! note "フォルダ構造をツリー形式で確認する"

    ```cmd
    REM カレントディレクトリのツリーを表示
    tree

    REM ファイルも含めて表示
    tree /f

    REM ASCII文字で表示（リダイレクト用）
    tree /a

    REM ファイルも含めてテキストファイルに保存
    tree /f /a > structure.txt
    ```

---

### ファイルを比較する

```cmd
fc [ファイル1] [ファイル2] [オプション]
```

!!! note "2つのファイルの差分を確認する"

    ```cmd
    REM テキストファイルを比較
    fc file1.txt file2.txt

    REM バイナリ比較
    fc /b image1.png image2.png

    REM 大文字・小文字を区別しない比較
    fc /c file1.txt file2.txt

    REM 行番号付きで差分を表示
    fc /n file1.txt file2.txt
    ```

---

### ファイルの属性を設定する

```cmd
attrib [属性] [ファイル名]
```

!!! note "ファイルの読み取り専用・隠しなどの属性を変更する"

    ```cmd
    REM 読み取り専用にする
    attrib +r file.txt

    REM 読み取り専用を解除する
    attrib -r file.txt

    REM 隠しファイルにする
    attrib +h file.txt

    REM サブディレクトリも含めて属性変更
    attrib +r *.txt /s
    ```

---

### 大容量コピーを行う

```cmd
xcopy [コピー元] [コピー先] [オプション]
```

!!! note "ディレクトリごとコピーする（強化版 copy コマンド）"

    ```cmd
    REM ディレクトリごとコピー（サブディレクトリも）
    xcopy C:\Projects D:\Backup\Projects /s /e /i

    REM 更新されたファイルのみコピー
    xcopy C:\Projects D:\Backup\Projects /s /e /d

    REM 確認なしで上書き
    xcopy C:\Projects D:\Backup\Projects /s /e /y
    ```

---

### 高機能コピーを行う

```cmd
robocopy [コピー元] [コピー先] [オプション]
```

!!! note "信頼性の高いバックアップ・同期コピーを行う"

    ```cmd
    REM ディレクトリを完全にミラーリング（削除も同期）
    robocopy C:\Source D:\Backup /mir

    REM ログファイルを出力しながらコピー
    robocopy C:\Source D:\Backup /e /log:copy_log.txt

    REM 変更されたファイルのみコピー
    robocopy C:\Source D:\Backup /e /xo

    REM 特定の拡張子を除外してコピー
    robocopy C:\Source D:\Backup /e /xf *.tmp *.log

    REM 再試行回数とウェイトを指定
    robocopy C:\Source D:\Backup /e /r:3 /w:5
    ```

---

## テキスト処理・出力操作

### テキストを出力する

```cmd
echo [テキスト]
```

!!! note "画面にテキストを出力する／空行を入れる"

    ```cmd
    REM テキストを出力
    echo Hello, World!

    REM 空行を出力
    echo.

    REM ファイルに書き込む（上書き）
    echo 新しい内容 > output.txt

    REM ファイルに追記する
    echo 追記内容 >> output.txt

    REM コマンドのエコーを非表示にする（バッチ先頭に記述）
    @echo off
    ```

---

### テキストを検索する

```cmd
find "検索文字列" [ファイル名]
```

!!! note "ファイルや出力から文字列を検索する"

    ```cmd
    REM ファイル内を検索
    find "error" log.txt

    REM 大文字・小文字を区別しない検索
    find /i "error" log.txt

    REM 一致した行番号を表示
    find /n "error" log.txt

    REM 一致しない行を表示（否定検索）
    find /v "error" log.txt

    REM コマンド出力をパイプでフィルタリング
    dir | find "2024"
    ```

---

### 正規表現でテキストを検索する

```cmd
findstr "パターン" [ファイル名]
```

!!! note "find より高機能なテキスト検索を行う"

    ```cmd
    REM 正規表現で検索
    findstr "error[0-9]" log.txt

    REM 大文字・小文字を区別しない検索
    findstr /i "warning" log.txt

    REM サブディレクトリも含めて検索
    findstr /s /i "TODO" *.txt

    REM 複数のキーワードをOR検索
    findstr "error\|warning" log.txt

    REM 行番号付きで表示
    findstr /n "error" log.txt
    ```

---

### 出力をページ単位で表示する

```cmd
more [ファイル名]
```

!!! note "長い出力をスクロールしながら確認する"

    ```cmd
    REM ファイルをページ表示
    more readme.txt

    REM コマンド出力をパイプでページ表示
    dir /s | more

    REM 特定行から表示開始
    more +10 readme.txt
    ```

---

### 出力を並び替える

```cmd
sort [ファイル名]
```

!!! note "テキストをアルファベット順にソートする"

    ```cmd
    REM ファイルの内容を並び替えてて表示
    sort names.txt

    REM 逆順に並び替え
    sort /r names.txt

    REM コマンド出力を並び替える
    dir /b | sort

    REM 特定列からソート（3文字目以降）
    sort /+3 names.txt
    ```

---

### クリップボードにコピーする

```cmd
[コマンド] | clip
```

!!! note "コマンドの出力をクリップボードに送る"

    ```cmd
    REM ディレクトリ一覧をクリップボードにコピー
    dir | clip

    REM ファイルの内容をクリップボードにコピー
    type file.txt | clip

    REM ipconfig の結果をコピー
    ipconfig | clip
    ```

---

## システム情報・管理

### システム情報を表示する

```cmd
systeminfo
```

!!! note "OS・ハードウェア情報を一覧表示する"

    ```cmd
    REM システム情報を表示
    systeminfo

    REM CSV形式で出力
    systeminfo /fo csv > sysinfo.csv

    REM リモートコンピュータの情報を表示
    systeminfo /s RemotePC /u domain\user /p password
    ```

---

### 実行中プロセスを確認する

```cmd
tasklist [オプション]
```

!!! note "プロセスの一覧を確認する"

    ```cmd
    REM プロセス一覧を表示
    tasklist

    REM 特定のプロセスを検索
    tasklist | find "notepad"

    REM 詳細情報（モジュール含む）を表示
    tasklist /v

    REM CSV形式で出力
    tasklist /fo csv > processes.csv

    REM サービス情報も表示
    tasklist /svc
    ```

---

### プロセスを強制終了する

```cmd
taskkill /im [プロセス名] または /pid [PID]
```

!!! note "応答しないプロセスを終了する"

    ```cmd
    REM プロセス名で終了
    taskkill /im notepad.exe

    REM PID で終了
    taskkill /pid 1234

    REM 強制終了（/f オプション）
    taskkill /f /im chrome.exe

    REM 子プロセスも含めて終了
    taskkill /f /t /im myapp.exe
    ```

---

### ログオン中のユーザーを確認する

```cmd
whoami
```

!!! note "現在のユーザー名やグループ情報を確認する"

    ```cmd
    REM 現在のユーザー名を表示
    whoami

    REM 詳細情報（グループ・権限）を表示
    whoami /all

    REM グループ一覧を表示
    whoami /groups

    REM 特権一覧を表示
    whoami /priv
    ```

---

### 日付・時刻を表示する

```cmd
date
time
```

!!! note "システムの日付・時刻を確認または変更する"

    ```cmd
    REM 現在の日付を表示（変更なし）
    date /t

    REM 現在の時刻を表示（変更なし）
    time /t

    REM 日付を変更する（管理者権限が必要）
    date 2024/12/31

    REM 時刻を変更する（管理者権限が必要）
    time 09:00:00
    ```

---

### 画面をクリアする

```cmd
cls
```

!!! note "コマンドプロンプトの画面を消去する"

    ```cmd
    REM 画面をクリア
    cls
    ```

---

## ネットワーク操作

### ネットワーク設定を確認する

```cmd
ipconfig [オプション]
```

!!! note "IP アドレス・MACアドレスなどを確認する"

    ```cmd
    REM ネットワーク設定を表示
    ipconfig

    REM 詳細情報（DNS・MACアドレス）を表示
    ipconfig /all

    REM DNS キャッシュを消去する
    ipconfig /flushdns

    REM IP アドレスを更新する（DHCP）
    ipconfig /release
    ipconfig /renew

    REM DNS キャッシュの内容を表示
    ipconfig /displaydns
    ```

---

### 接続を確認する

```cmd
ping [ホスト名またはIPアドレス]
```

!!! note "ネットワーク接続・応答速度を確認する"

    ```cmd
    REM Google に ping を送信
    ping google.com

    REM 回数を指定して ping
    ping -n 10 google.com

    REM 無限に ping（Ctrl+C で停止）
    ping -t google.com

    REM パケットサイズを指定
    ping -l 1024 google.com

    REM 結果をファイルに保存
    ping google.com > ping_result.txt
    ```

---

### 通信経路を確認する

```cmd
tracert [ホスト名またはIPアドレス]
```

!!! note "パケットが通過するルートを調べる"

    ```cmd
    REM 経路を追跡
    tracert google.com

    REM 逆引きをしないで高速表示
    tracert -d google.com

    REM ホップ数の上限を設定
    tracert -h 15 google.com
    ```

---

### ネットワーク接続状況を確認する

```cmd
netstat [オプション]
```

!!! note "TCP/UDP の接続状態・ポート使用状況を確認する"

    ```cmd
    REM すべての接続とリスニングポートを表示
    netstat -a

    REM プロセスID も表示（管理者権限推奨）
    netstat -ano

    REM 統計情報を表示
    netstat -s

    REM 特定のポートを検索
    netstat -ano | find "8080"

    REM 接続をリアルタイム更新（5秒ごと）
    netstat -a 5
    ```

---

### DNS の名前解決を確認する

```cmd
nslookup [ドメイン名]
```

!!! note "ドメイン名と IP アドレスを相互変換する"

    ```cmd
    REM ドメインのIPアドレスを調べる
    nslookup google.com

    REM 特定のDNSサーバーを指定して問い合わせる
    nslookup google.com 8.8.8.8

    REM 逆引き（IP→ドメイン）
    nslookup 8.8.8.8

    REM インタラクティブモードで起動
    nslookup
    ```

---

### ネットワーク共有・ユーザー管理をする

```cmd
net [サブコマンド]
```

!!! note "ネットワークリソースやユーザーアカウントを管理する"

    ```cmd
    REM 共有フォルダの一覧を表示
    net share

    REM フォルダを共有する
    net share ShareName=C:\SharedFolder

    REM 共有を削除する
    net share ShareName /delete

    REM ユーザー一覧を表示
    net user

    REM 新しいローカルユーザーを作成
    net user username password /add

    REM サービスを開始・停止する
    net start "サービス名"
    net stop "サービス名"

    REM 現在のネットワーク接続を表示
    net use
    ```

---

## 環境変数・設定

### 環境変数を表示・設定する

```cmd
set [変数名[=値]]
```

!!! note "環境変数の確認・追加・変更を行う"

    ```cmd
    REM すべての環境変数を表示
    set

    REM 特定の変数を表示
    set PATH

    REM 変数を一時的に設定する（セッション内のみ有効）
    set MY_VAR=HelloWorld

    REM 変数を使用する
    echo %MY_VAR%

    REM 変数を削除する
    set MY_VAR=

    REM ユーザーから入力を受け取る
    set /p INPUT_VALUE=値を入力してください:

    REM 計算結果を変数に代入する
    set /a RESULT=10+5
    echo %RESULT%
    ```

---

### 永続的な環境変数を設定する

```cmd
setx [変数名] [値]
```

!!! note "再起動後も残る環境変数を設定する"

    ```cmd
    REM ユーザー環境変数を設定（現在のユーザー）
    setx MY_VAR "C:\MyPath"

    REM システム環境変数を設定（管理者権限が必要）
    setx MY_VAR "C:\MyPath" /m

    REM PATH に新しいパスを追加する
    setx PATH "%PATH%;C:\NewTool\bin"
    ```

---

### コンソールのタイトルを変更する

```cmd
title [タイトル文字列]
```

!!! note "ウィンドウのタイトルバーを変更する"

    ```cmd
    REM タイトルを変更する
    title 作業用コンソール

    REM バッチスクリプト内でタイトルを設定
    title %~n0 - 実行中
    ```

---

### コンソールの色を変更する

```cmd
color [背景色][文字色]
```

!!! note "コンソールの配色を変更する（16進数 0〜F）"

    ```cmd
    REM 黒背景に緑文字（マトリックス風）
    color 0A

    REM 白背景に黒文字
    color F0

    REM デフォルト（黒背景に白文字）に戻す
    color
    ```

    | コード | 色 |
    | --- | --- |
    | 0 | 黒 |
    | 1 | 青 |
    | 2 | 緑 |
    | 3 | 水色 |
    | 4 | 赤 |
    | 5 | 紫 |
    | 6 | 黄 |
    | 7 | 白 |
    | 8〜F | 各色の明るいバージョン |

---

### プロンプトの表示を変更する

```cmd
prompt [文字列]
```

!!! note "コマンドプロンプトの入力促進記号をカスタマイズする"

    ```cmd
    REM 現在時刻を表示するプロンプト
    prompt $T$G

    REM パスと > を表示（デフォルト相当）
    prompt $P$G

    REM デフォルトに戻す
    prompt
    ```

    | 記号 | 意味 |
    | --- | --- |
    | `$P` | カレントパス |
    | `$G` | `>` 記号 |
    | `$T` | 現在時刻 |
    | `$D` | 現在日付 |
    | `$L` | `<` 記号 |

---

## バッチスクリプト

### 条件分岐を行う

```cmd
if [条件] (処理) else (別の処理)
```

!!! note "条件によって実行するコマンドを切り替える"

    ```cmd
    REM ファイルが存在するか確認
    if exist "C:\file.txt" (
        echo ファイルが存在します
    ) else (
        echo ファイルが存在しません
    )

    REM 文字列の比較
    if "%1"=="" (
        echo 引数が指定されていません
    ) else (
        echo 引数: %1
    )

    REM 数値の比較
    if %ERRORLEVEL% equ 0 (
        echo 正常終了しました
    ) else (
        echo エラーが発生しました
    )
    ```

---

### 繰り返し処理を行う

```cmd
for [変数] in (リスト) do [処理]
```

!!! note "ファイルやリストに対して繰り返し処理を行う"

    ```cmd
    REM リストの各要素に対して処理
    for %%i in (apple banana cherry) do echo %%i

    REM カレントディレクトリの .txt ファイルを処理
    for %%f in (*.txt) do echo %%f

    REM サブディレクトリも含めて再帰処理
    for /r "C:\Projects" %%f in (*.log) do del "%%f"

    REM 数値のループ（1〜5）
    for /l %%i in (1,1,5) do echo %%i

    REM コマンドの出力を行ごとに処理
    for /f "tokens=*" %%i in ('dir /b') do echo %%i
    ```

---

### ラベルとジャンプを使う

```cmd
:ラベル名
goto ラベル名
```

!!! note "バッチスクリプト内で処理の流れを制御する"

    ```cmd
    @echo off
    set /p CHOICE=続けますか？ (y/n):
    if /i "%CHOICE%"=="y" goto :続ける
    echo キャンセルしました
    exit /b 0

    :続ける
    echo 処理を続けます
    ```

---

### バッチスクリプトの基本テンプレート

```cmd
@echo off
setlocal enabledelayedexpansion
```

!!! note "バッチスクリプトの推奨テンプレート"

    ```cmd
    @echo off
    REM ===================================
    REM スクリプト名: backup.bat
    REM 説明: ファイルをバックアップする
    REM ===================================
    setlocal enabledelayedexpansion

    REM 変数の設定
    set SOURCE=C:\Source
    set DEST=D:\Backup

    REM 処理開始
    echo バックアップを開始します...
    robocopy "%SOURCE%" "%DEST%" /mir /log:backup.log

    if %ERRORLEVEL% leq 1 (
        echo バックアップが完了しました
    ) else (
        echo エラーが発生しました。backup.log を確認してください
        exit /b 1
    )

    endlocal
    ```

---

## レジストリ・サービス管理

### レジストリを操作する

```cmd
reg [サブコマンド] [キー] [オプション]
```

!!! note "Windowsレジストリの読み書き・バックアップを行う（管理者権限推奨）"

    ```cmd
    REM レジストリキーの内容を表示
    reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run

    REM レジストリの値を追加・変更
    reg add HKCU\Software\MyApp /v Setting /t REG_SZ /d "value" /f

    REM レジストリの値を削除
    reg delete HKCU\Software\MyApp /v Setting /f

    REM レジストリをファイルにエクスポート（バックアップ）
    reg export HKCU\Software\MyApp backup.reg

    REM バックアップファイルをインポート
    reg import backup.reg
    ```

---

### サービスを管理する

```cmd
sc [サブコマンド] [サービス名]
```

!!! note "Windowsサービスの制御を行う（管理者権限が必要）"

    ```cmd
    REM サービスの状態を確認
    sc query Spooler

    REM サービスを開始する
    sc start Spooler

    REM サービスを停止する
    sc stop Spooler

    REM サービスのスタートアップを自動に変更
    sc config Spooler start= auto

    REM サービス一覧を表示
    sc query type= all state= all
    ```

---

## 上級者向けコマンド

### システムファイルを検査する

```cmd
sfc /scannow
```

!!! note "壊れたシステムファイルを検出・修復する（管理者権限が必要）"

    ```cmd
    REM システムファイルをスキャンして修復
    sfc /scannow

    REM スキャンのみ行い修復しない
    sfc /verifyonly

    REM 特定のファイルのみ検証
    sfc /verifyfile=C:\Windows\System32\kernel32.dll
    ```

---

### ディスク修復を行う

```cmd
chkdsk [ドライブ] [オプション]
```

!!! note "ディスクエラーを検出・修復する（管理者権限が必要）"

    ```cmd
    REM C ドライブのエラーをチェック（読み取りのみ）
    chkdsk C:

    REM エラーを自動修復する（要再起動）
    chkdsk C: /f

    REM 不良セクタを検索して修復する
    chkdsk C: /r

    REM 次回起動時にチェックをスケジュール
    chkdsk C: /f /r /x
    ```

---

### 管理者権限でコマンドを実行する

```cmd
runas /user:Administrator [コマンド]
```

!!! note "別のユーザー権限でコマンドを実行する"

    ```cmd
    REM 管理者として特定のコマンドを実行
    runas /user:Administrator "cmd /k ipconfig /all"

    REM ドメインユーザーとして実行
    runas /user:domain\username "notepad.exe"
    ```

---

### 実行ファイルのパスを調べる

```cmd
where [コマンド名]
```

!!! note "コマンドの実体がどこにあるかを確認する"

    ```cmd
    REM python の場所を調べる
    where python

    REM git の場所を調べる
    where git

    REM すべての一致を表示
    where /r C:\ python.exe
    ```

---

### DISM でシステムを修復する

```cmd
dism /online /cleanup-image /restorehealth
```

!!! note "Windows イメージの健全性を確認・修復する（管理者権限が必要）"

    ```cmd
    REM システムの健全性をスキャンして修復
    dism /online /cleanup-image /restorehealth

    REM 健全性をスキャンのみ（修復しない）
    dism /online /cleanup-image /scanhealth

    REM システムイメージの健全性を確認
    dism /online /cleanup-image /checkhealth
    ```

---

### Powershell コマンドを実行する

```cmd
powershell -command "[PSコマンド]"
```

!!! note "cmd から PowerShell コマンドを呼び出す"

    ```cmd
    REM PowerShell でファイル一覧を取得
    powershell -command "Get-ChildItem"

    REM PowerShell スクリプトを実行
    powershell -executionpolicy bypass -file script.ps1

    REM PowerShell ウィンドウを起動
    powershell
    ```

---

## 知ってると便利なコマンド

### コマンド履歴を管理する（doskey）

```cmd
doskey [マクロ名=[コマンド]]
```

!!! note "コマンドのエイリアスを作成する／履歴を管理する"

    ```cmd
    REM エイリアスを作成（ls で dir を実行）
    doskey ls=dir

    REM 複数コマンドのマクロを作成
    doskey cleanup=del /q *.tmp $T del /q *.log

    REM 定義済みマクロ一覧を表示
    doskey /macros

    REM コマンド履歴を表示
    doskey /history
    ```

---

### コマンドのヘルプを表示する

```cmd
[コマンド名] /?
help [コマンド名]
```

!!! note "各コマンドの使い方を確認する"

    ```cmd
    REM dir コマンドのヘルプを表示
    dir /?

    REM help コマンドで表示
    help dir

    REM 利用可能なコマンド一覧を表示
    help
    ```

---

### ファイルの関連付けを確認する

```cmd
assoc [拡張子]
```

!!! note "拡張子とファイルタイプの関連付けを確認する"

    ```cmd
    REM .txt の関連付けを表示
    assoc .txt

    REM すべての関連付けを一覧表示
    assoc

    REM 関連付けを変更する（管理者権限が必要）
    assoc .txt=txtfile
    ```

---

### タイムアウト処理をする

```cmd
timeout /t [秒数]
```

!!! note "バッチスクリプトで待機処理を入れる"

    ```cmd
    REM 5秒待機（キー入力でスキップ可）
    timeout /t 5

    REM 10秒待機（スキップ不可）
    timeout /t 10 /nobreak

    REM 無限に待機（Ctrl+C で停止）
    timeout /t -1
    ```

---

### ユーザーに選択を求める

```cmd
choice /c [選択肢] /m [メッセージ]
```

!!! note "バッチスクリプトでユーザーに選択入力を求める"

    ```cmd
    REM はい・いいえを選択させる
    choice /c YN /m "続けますか？"
    if %ERRORLEVEL%==1 echo はい を選択しました
    if %ERRORLEVEL%==2 echo いいえ を選択しました

    REM 3択の選択（タイムアウト付き）
    choice /c 123 /t 10 /d 1 /m "オプションを選んでください"
    ```

---

### コマンド出力をリダイレクトする

```cmd
[コマンド] > [ファイル]
[コマンド] >> [ファイル]
[コマンド] 2> [ファイル]
```

!!! note "出力・エラーをファイルに保存する"

    ```cmd
    REM 標準出力をファイルに書き込む（上書き）
    dir > filelist.txt

    REM 標準出力をファイルに追記する
    echo 追記 >> filelist.txt

    REM エラー出力をファイルに保存する
    dir C:\NoExist 2> error.txt

    REM 標準出力とエラー出力を同じファイルに保存する
    dir 2>&1 > all_output.txt

    REM 出力を破棄する（NUL に捨てる）
    ping google.com > nul
    ```

---

### パイプでコマンドをつなぐ

```cmd
[コマンド1] | [コマンド2]
```

!!! note "コマンドの出力を次のコマンドの入力として渡す"

    ```cmd
    REM プロセス一覧からメモ帳を探す
    tasklist | find "notepad"

    REM ログファイルのエラー行を数える
    find /c "error" app.log

    REM ディレクトリ一覧をソートしてページ表示
    dir /b | sort | more

    REM ネットワーク接続から特定ポートを確認
    netstat -ano | findstr ":80"
    ```

---

## よく使うオプション一覧

| オプション | コマンド例 | 説明 |
| --- | --- | --- |
| `/?` | `dir /?` | ヘルプを表示 |
| `/s` | `dir /s` | サブディレクトリも対象にする |
| `/q` | `del /q` | 確認プロンプトを非表示 |
| `/f` | `del /f` | 強制実行 |
| `/a` | `dir /a` | 隠しファイルも含める |
| `/y` | `copy /y` | 上書きを自動で許可 |
| `/r` | `chkdsk /r` | 不良セクタも修復 |
| `/n` | `find /n` | 行番号を表示 |
| `/i` | `find /i` | 大文字・小文字を区別しない |
| `/v` | `find /v` | 一致しない行を表示 |
| `/b` | `dir /b` | ファイル名のみ表示 |
| `/t` | `ping -t` | 継続して実行 |
| `/c` | `cmd /c` | コマンドを実行して終了 |
| `/k` | `cmd /k` | コマンドを実行してウィンドウを維持 |
| `/m` | `setx /m` | システム全体に適用 |

---

## 頻出コマンド早引き

```cmd
REM === ファイル操作 ===
dir                        REM カレントの一覧
dir /s /b *.txt            REM サブフォルダも含めて .txt を検索
mkdir newfolder            REM フォルダ作成
copy src.txt dst.txt       REM ファイルコピー
move file.txt D:\folder\   REM ファイル移動
del /q *.tmp               REM 一時ファイル削除
robocopy src dst /mir      REM フォルダのミラーリング

REM === テキスト操作 ===
type file.txt              REM ファイル内容を表示
find /i "error" log.txt    REM 文字列検索
findstr /s /i "TODO" *.txt REM 再帰検索
dir | clip                 REM 一覧をクリップボードにコピー

REM === システム情報 ===
systeminfo                 REM システム情報
tasklist | find "名前"     REM プロセス検索
taskkill /f /im name.exe   REM プロセス強制終了
whoami /all                REM ユーザーと権限一覧

REM === ネットワーク ===
ipconfig /all              REM ネットワーク設定詳細
ipconfig /flushdns         REM DNS キャッシュ消去
ping -n 4 google.com       REM 疎通確認
netstat -ano               REM 接続状態確認
tracert google.com         REM 経路確認
nslookup google.com        REM DNS 名前解決

REM === 環境変数 ===
set                        REM 全環境変数を表示
echo %PATH%                REM PATH を表示
set MY_VAR=value           REM 一時的に変数をセット
setx MY_VAR "value"        REM 永続的に変数をセット

REM === 管理操作（管理者権限が必要） ===
sfc /scannow               REM システムファイル修復
chkdsk C: /f               REM ディスク修復（要再起動）
dism /online /cleanup-image /restorehealth  REM イメージ修復
```

---

!!! info "公式ドキュメント"
    - [Windows コマンド一覧（Microsoft Learn）](https://learn.microsoft.com/ja-jp/windows-server/administration/windows-commands/windows-commands)
    - [バッチファイル リファレンス](https://learn.microsoft.com/ja-jp/windows-server/administration/windows-commands/cmd)
    - [PowerShell ドキュメント](https://learn.microsoft.com/ja-jp/powershell/)
