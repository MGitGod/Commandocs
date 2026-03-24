# PowerShell

Windows / macOS / Linux で動作する PowerShell 7（pwsh）および Windows PowerShell 5.1 の
コマンドレットをまとめたリファレンスです。
基本構文・説明・使用例をセットで記載しています。

---

## インストール・アップデート・アンインストール

### PowerShell 7 をインストールする

=== "Windows（winget）"

    ```powershell
    winget install --id Microsoft.PowerShell --source winget
    ```

=== "Windows（ユーザースコープ）"

    ```powershell
    winget install --id Microsoft.PowerShell --scope user
    ```

=== "macOS（Homebrew）"

    ```bash
    brew install --cask powershell
    ```

=== "Linux（Ubuntu）"

    ```bash
    sudo apt-get install -y powershell
    ```

!!! note "説明"
    winget は Windows 標準のパッケージマネージャーです。`--scope user` を付けると管理者権限なしでインストールできます。

#### 使用例

```powershell
# インストール後にバージョンを確認する
pwsh --version
$PSVersionTable
```

---

### PowerShell 7 をアップデートする

```powershell
winget upgrade --id Microsoft.PowerShell
```

!!! note "説明"
    インストール済みの PowerShell 7 を最新バージョンへ更新します。

#### 使用例

```powershell
# アップデート可能な一覧を確認してから実行する
winget upgrade
winget upgrade --id Microsoft.PowerShell
```

---

### PowerShell 7 をアンインストールする

```powershell
winget uninstall --id Microsoft.PowerShell
```

!!! note "説明"
    PowerShell 7 をシステムから削除します。Windows PowerShell 5.1 は削除されません。

#### 使用例

```powershell
winget uninstall --id Microsoft.PowerShell
```

---

### モジュールをインストールする（Install-Module）

```powershell
Install-Module -Name <モジュール名> -Scope CurrentUser
Install-Module -Name <モジュール名> -Scope CurrentUser -Force
Install-Module -Name <モジュール名> -Scope CurrentUser -AllowClobber
```

!!! note "説明"
    PowerShell Gallery（PSGallery）から任意のモジュールをダウンロードしてインストールします。`-Scope CurrentUser` を付けると管理者権限が不要です。

#### 使用例

```powershell
Install-Module -Name posh-git -Scope CurrentUser
Install-Module -Name PSReadLine -Scope CurrentUser -Force
Install-Module -Name Az -Scope CurrentUser -AllowClobber
```

---

### モジュールをアップデートする（Update-Module）

```powershell
Update-Module -Name <モジュール名>
Get-InstalledModule | Update-Module
```

!!! note "説明"
    インストール済みモジュールを最新バージョンに更新します。引数を省略するとすべてのモジュールを一括更新します。

#### 使用例

```powershell
Update-Module -Name posh-git
Get-InstalledModule | Update-Module
```

---

### モジュールをアンインストールする（Uninstall-Module）

```powershell
Uninstall-Module -Name <モジュール名>
Uninstall-Module -Name <モジュール名> -RequiredVersion <バージョン>
```

!!! note "説明"
    インストール済みモジュールを削除します。`-RequiredVersion` でバージョンを指定できます。

#### 使用例

```powershell
Uninstall-Module -Name posh-git
Uninstall-Module -Name Az -RequiredVersion 1.0.0
```

---

## 基本コマンド

### ヘルプを表示する（Get-Help）

```powershell
Get-Help <コマンド名>
Get-Help <コマンド名> -Full
Get-Help <コマンド名> -Examples
Get-Help <コマンド名> -Online
```

!!! note "説明"
    コマンドレットの使い方・パラメーター・使用例を表示します。`-Online` でブラウザから最新ドキュメントを開けます。オフライン用ヘルプは `Update-Help -Force` で更新できます。

#### 使用例

```powershell
Get-Help Get-Process
Get-Help Get-Process -Full
Get-Help Get-Process -Examples
Get-Help Get-Process -Online
Update-Help -Force
```

---

### コマンドを検索する（Get-Command）

```powershell
Get-Command
Get-Command -Name <パターン>
Get-Command -Verb <動詞>
Get-Command -Noun <名詞>
Get-Command -Module <モジュール名>
```

!!! note "説明"
    使えるコマンドレット・関数・エイリアスを一覧表示します。ワイルドカード（`*`）が使えます。

#### 使用例

```powershell
Get-Command -Name *process*
Get-Command -Verb Get
Get-Command -Noun Process
Get-Command -Module Microsoft.PowerShell.Management
```

---

### エイリアスを確認する（Get-Alias）

```powershell
Get-Alias
Get-Alias -Name <エイリアス名>
Get-Alias -Definition <コマンドレット名>
```

!!! note "説明"
    短縮コマンド（エイリアス）の一覧を表示します。`ls`・`cd`・`cat` などの Unix 風エイリアスもここで確認できます。

#### 使用例

```powershell
Get-Alias -Name ls
Get-Alias -Definition Get-ChildItem
Get-Alias | Where-Object { $_.Definition -like "Get-*" }
```

---

### 画面をクリアする（Clear-Host）

```powershell
Clear-Host
```

!!! note "説明"
    コンソール画面の表示をクリアします。エイリアス：`cls` / `clear`

#### 使用例

```powershell
Clear-Host
cls
```

---

### バージョンを確認する（$PSVersionTable）

```powershell
$PSVersionTable
$PSVersionTable.PSVersion
```

!!! note "説明"
    PowerShell のバージョン・OS・CLR バージョンなどの情報を表示します。スクリプト内でバージョン分岐をするときにも使います。

#### 使用例

```powershell
$PSVersionTable
$PSVersionTable.PSVersion.Major
pwsh --version
```

---

## ファイル・ディレクトリ操作

### ファイル・ディレクトリを一覧表示する（Get-ChildItem）

```powershell
Get-ChildItem
Get-ChildItem -Path <パス>
Get-ChildItem -Recurse
Get-ChildItem -Filter <パターン>
Get-ChildItem -Hidden
Get-ChildItem -File
Get-ChildItem -Directory
```

!!! note "説明"
    指定したパスのファイルとフォルダを一覧表示します。エイリアス：`ls` / `dir` / `gci`

#### 使用例

```powershell
Get-ChildItem -Path C:\Users -Recurse -Filter *.log
Get-ChildItem . -Hidden
Get-ChildItem -File | Sort-Object LastWriteTime -Descending
Get-ChildItem -Directory | Where-Object { $_.Name -like "test*" }
```

---

### 現在のディレクトリを確認する（Get-Location）

```powershell
Get-Location
```

!!! note "説明"
    現在作業中のディレクトリのパスを表示します。エイリアス：`pwd`

#### 使用例

```powershell
Get-Location
(Get-Location).Path
Get-Location | Set-Clipboard
```

---

### ディレクトリを移動する（Set-Location）

```powershell
Set-Location <パス>
Set-Location ..
Set-Location ~
Set-Location -
```

!!! note "説明"
    作業ディレクトリを変更します。`-` で直前のディレクトリに戻れます。エイリアス：`cd` / `chdir` / `sl`

#### 使用例

```powershell
Set-Location C:\Users
Set-Location ..
Set-Location ~
cd .\Documents
```

---

### ファイル内容を表示する（Get-Content）

```powershell
Get-Content <ファイルパス>
Get-Content <ファイルパス> -TotalCount <行数>
Get-Content <ファイルパス> -Tail <行数>
Get-Content <ファイルパス> -Wait
Get-Content <ファイルパス> -Encoding UTF8
```

!!! note "説明"
    ファイルの内容を表示します。`-Tail` で末尾 N 行、`-Wait` でリアルタイム追従（ログ監視）ができます。エイリアス：`cat` / `type` / `gc`

#### 使用例

```powershell
Get-Content .\readme.txt
Get-Content .\log.txt -Tail 20
Get-Content .\app.log -Wait
Get-Content .\data.csv -TotalCount 5
```

---

### ファイルにテキストを書き込む（Set-Content / Add-Content）

```powershell
Set-Content -Path <ファイルパス> -Value <テキスト>
Set-Content -Path <ファイルパス> -Value <テキスト> -Encoding UTF8
Add-Content -Path <ファイルパス> -Value <テキスト>
```

!!! note "説明"
    `Set-Content` はファイルを上書き、`Add-Content` は末尾に追記します。

#### 使用例

```powershell
Set-Content -Path .\output.txt -Value "Hello, World!"
Set-Content -Path .\result.txt -Value $result -Encoding UTF8
Add-Content -Path .\log.txt -Value "$(Get-Date) - 処理完了"
```

---

### ファイル・フォルダをコピーする（Copy-Item）

```powershell
Copy-Item <コピー元> <コピー先>
Copy-Item <コピー元> <コピー先> -Recurse
Copy-Item <コピー元> <コピー先> -Force
```

!!! note "説明"
    ファイルやフォルダをコピーします。フォルダをコピーする場合は `-Recurse` が必要です。エイリアス：`cp` / `copy` / `cpi`

#### 使用例

```powershell
Copy-Item .\file.txt .\backup\file.txt
Copy-Item .\src -Destination .\dst -Recurse
Copy-Item .\config.json .\config.backup.json -Force
```

---

### ファイル・フォルダを移動・名前変更する（Move-Item）

```powershell
Move-Item <移動元> <移動先>
Move-Item <移動元> <移動先> -Force
```

!!! note "説明"
    ファイルやフォルダを移動します。同じフォルダ内で別名に変更すると名前変更になります。エイリアス：`mv` / `move` / `mi`

#### 使用例

```powershell
Move-Item .\old.txt .\archive\old.txt
Move-Item .\old_name.txt .\new_name.txt
Move-Item .\temp -Destination .\archive -Force
```

---

### ファイル・フォルダを削除する（Remove-Item）

```powershell
Remove-Item <パス>
Remove-Item <パス> -Recurse
Remove-Item <パス> -Force
Remove-Item <パス> -Recurse -Force
```

!!! note "説明"
    ファイルやフォルダを削除します。フォルダを中身ごと削除するには `-Recurse` が必要です。エイリアス：`rm` / `del` / `erase` / `ri`

#### 使用例

```powershell
Remove-Item .\temp.txt
Remove-Item .\old_folder -Recurse -Force
Remove-Item .\*.log -Force
```

---

### ファイル・フォルダを新規作成する（New-Item）

```powershell
New-Item -Path <パス> -ItemType File
New-Item -Path <パス> -ItemType Directory
New-Item -Path <パス> -ItemType File -Value <初期内容>
```

!!! note "説明"
    ファイルまたはフォルダを新規作成します。`-Force` で中間ディレクトリも一緒に作れます。エイリアス：`ni`

#### 使用例

```powershell
New-Item -Path .\notes.txt -ItemType File
New-Item -Path .\MyProject -ItemType Directory
New-Item -Path .\hello.txt -ItemType File -Value "Hello!"
New-Item -Path .\src\lib -ItemType Directory -Force
```

---

## プロセス・サービス管理

### プロセス一覧を表示する（Get-Process）

```powershell
Get-Process
Get-Process -Name <プロセス名>
Get-Process -Id <PID>
```

!!! note "説明"
    実行中のプロセスを一覧表示します。CPU・メモリ使用量も確認できます。エイリアス：`ps` / `gps`

#### 使用例

```powershell
Get-Process
Get-Process -Name chrome
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
Get-Process | Where-Object { $_.WorkingSet -gt 500MB }
```

---

### プロセスを終了する（Stop-Process）

```powershell
Stop-Process -Name <プロセス名>
Stop-Process -Id <PID>
Stop-Process -Name <プロセス名> -Force
```

!!! note "説明"
    実行中のプロセスを終了します。`-Force` で応答しないプロセスも強制終了できます。エイリアス：`kill` / `spps`

#### 使用例

```powershell
Stop-Process -Name notepad
Stop-Process -Id 1234 -Force
Get-Process -Name chrome | Stop-Process
```

---

### サービスを一覧表示する（Get-Service）

```powershell
Get-Service
Get-Service -Name <サービス名>
Get-Service -DisplayName <表示名>
```

!!! note "説明"
    Windows サービスの状態（Running / Stopped など）を一覧表示します。エイリアス：`gsv`

#### 使用例

```powershell
Get-Service
Get-Service -Name wuauserv
Get-Service | Where-Object { $_.Status -eq "Running" }
```

---

### サービスを開始・停止・再起動する

```powershell
Start-Service -Name <サービス名>
Stop-Service -Name <サービス名>
Restart-Service -Name <サービス名>
```

!!! note "説明"
    Windows サービスを操作します。管理者権限が必要なサービスもあります。

#### 使用例

```powershell
Start-Service -Name Spooler
Stop-Service -Name Spooler
Restart-Service -Name W32Time
Get-Service -Name wuauserv | Stop-Service
```

---

## テキスト・データ処理

### パターンで行を検索する（Select-String）

```powershell
Select-String -Path <ファイルパス> -Pattern <正規表現>
Select-String -Path <ファイルパス> -Pattern <パターン> -CaseSensitive
Select-String -Path <ファイルパス> -Pattern <パターン> -NotMatch
<コマンド> | Select-String <パターン>
```

!!! note "説明"
    ファイルや出力からパターンに一致する行を抽出します（Unix の `grep` 相当）。正規表現が使えます。エイリアス：`sls`

#### 使用例

```powershell
Select-String -Path .\*.log -Pattern "ERROR"
Select-String -Path .\app.log -Pattern "timeout" -CaseSensitive
Get-Content .\access.log | Select-String "404"
```

---

### オブジェクトをフィルタリングする（Where-Object）

```powershell
<コマンド> | Where-Object { <条件式> }
<コマンド> | Where-Object -Property <プロパティ> -EQ <値>
```

!!! note "説明"
    条件に一致するオブジェクトだけを取り出します。`$_` で現在のオブジェクトを参照します。エイリアス：`where` / `?`

#### 使用例

```powershell
Get-Process | Where-Object { $_.CPU -gt 10 }
Get-Service | Where-Object { $_.Status -eq "Running" }
Get-ChildItem | Where-Object { $_.Length -gt 1MB }
Get-ChildItem | Where-Object { $_.LastWriteTime -gt (Get-Date).AddDays(-7) }
```

---

### プロパティを選択する（Select-Object）

```powershell
<コマンド> | Select-Object <プロパティ名1>, <プロパティ名2>
<コマンド> | Select-Object -First <件数>
<コマンド> | Select-Object -Last <件数>
<コマンド> | Select-Object -Unique
<コマンド> | Select-Object -ExpandProperty <プロパティ名>
```

!!! note "説明"
    オブジェクトから必要なプロパティだけを取り出します。`-ExpandProperty` で値だけを文字列として取り出せます。エイリアス：`select`

#### 使用例

```powershell
Get-Process | Select-Object Name, CPU, WorkingSet
Get-ChildItem | Select-Object -First 5
Get-Process | Select-Object -ExpandProperty Name
Get-ChildItem | Select-Object Name, @{Name="SizeMB"; Expression={[math]::Round($_.Length/1MB,2)}}
```

---

### 並び替えをする（Sort-Object）

```powershell
<コマンド> | Sort-Object <プロパティ名>
<コマンド> | Sort-Object <プロパティ名> -Descending
<コマンド> | Sort-Object <プロパティ1>, <プロパティ2>
```

!!! note "説明"
    オブジェクトのプロパティを基準に並び替えます。デフォルトは昇順で、`-Descending` で降順になります。エイリアス：`sort`

#### 使用例

```powershell
Get-ChildItem | Sort-Object LastWriteTime -Descending
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
Get-Service | Sort-Object Status, Name
```

---

### 繰り返し処理をする（ForEach-Object）

```powershell
<コマンド> | ForEach-Object { <処理> }
1..<数> | ForEach-Object { <処理> }
```

!!! note "説明"
    パイプラインの各要素に対して処理を繰り返します。`$_` で現在の要素を参照します。エイリアス：`foreach` / `%`

#### 使用例

```powershell
1..5 | ForEach-Object { Write-Host "番号: $_" }
Get-ChildItem *.txt | ForEach-Object { Copy-Item $_ .\backup\ }
Get-Process | ForEach-Object { "$($_.Name): $($_.CPU)" }
```

---

### グループ化する（Group-Object）

```powershell
<コマンド> | Group-Object <プロパティ名>
<コマンド> | Group-Object <プロパティ名> -NoElement
```

!!! note "説明"
    指定したプロパティの値でオブジェクトをグループ化し、件数を集計します。

#### 使用例

```powershell
Get-Process | Group-Object -Property Company
Get-ChildItem | Group-Object Extension
Get-Service | Group-Object Status
```

---

### 集計する（Measure-Object）

```powershell
<コマンド> | Measure-Object
<コマンド> | Measure-Object -Property <プロパティ名> -Sum -Average -Maximum -Minimum
```

!!! note "説明"
    件数・合計・平均・最大・最小を集計します。ファイルサイズの合計などに便利です。

#### 使用例

```powershell
Get-ChildItem -Recurse -File | Measure-Object -Property Length -Sum
Get-Process | Measure-Object -Property CPU -Average -Maximum
(Get-Content .\file.txt | Measure-Object -Line).Lines
```

---

### プロパティ・メソッドを確認する（Get-Member）

```powershell
<コマンド> | Get-Member
<コマンド> | Get-Member -MemberType Property
<コマンド> | Get-Member -MemberType Method
```

!!! note "説明"
    オブジェクトが持つプロパティとメソッドの一覧を表示します。新しいコマンドを使うときに何ができるか把握するのに役立ちます。エイリアス：`gm`

#### 使用例

```powershell
Get-Process | Get-Member
Get-Process | Get-Member -MemberType Property
"Hello" | Get-Member
(Get-Date) | Get-Member -MemberType Method
```

---

## CSV・JSON・XML 操作

### CSV を読み込む（Import-Csv）

```powershell
Import-Csv -Path <CSVファイルパス>
Import-Csv -Path <CSVファイルパス> -Encoding UTF8
Import-Csv -Path <CSVファイルパス> -Delimiter <区切り文字>
```

!!! note "説明"
    CSV ファイルをオブジェクトの配列として読み込みます。ヘッダ行が自動でプロパティ名になります。

#### 使用例

```powershell
$users = Import-Csv -Path .\users.csv -Encoding UTF8
$users | Where-Object { $_.Age -gt 30 }
Import-Csv .\data.csv -Delimiter "`t"
```

---

### CSV に書き出す（Export-Csv）

```powershell
<コマンド> | Export-Csv -Path <出力パス> -NoTypeInformation
<コマンド> | Export-Csv -Path <出力パス> -NoTypeInformation -Encoding UTF8
<コマンド> | Export-Csv -Path <出力パス> -Append
```

!!! note "説明"
    オブジェクトを CSV ファイルに書き出します。`-NoTypeInformation` で先頭の型情報行を省略できます。

#### 使用例

```powershell
Get-Process | Select-Object Name, CPU, Id | Export-Csv .\procs.csv -NoTypeInformation
Import-Csv .\users.csv | Where-Object { $_.Age -gt 30 } | Export-Csv .\senior.csv -NoTypeInformation -Encoding UTF8
```

---

### JSON を読み込む（ConvertFrom-Json）

```powershell
Get-Content <JSONファイルパス> | ConvertFrom-Json
```

!!! note "説明"
    JSON 文字列を PowerShell オブジェクトに変換します。プロパティへのドットアクセスができるようになります。

#### 使用例

```powershell
$config = Get-Content .\config.json | ConvertFrom-Json
$config.version
$config.database.host
```

---

### JSON に変換する（ConvertTo-Json）

```powershell
<コマンド> | ConvertTo-Json
<コマンド> | ConvertTo-Json -Depth <深さ>
<コマンド> | ConvertTo-Json -Compress
```

!!! note "説明"
    PowerShell オブジェクトを JSON 文字列に変換します。深くネストしたオブジェクトは `-Depth` で展開レベルを指定します（デフォルトは 2）。

#### 使用例

```powershell
@{ name = "test"; value = 123 } | ConvertTo-Json
Get-Process | Select-Object Name, Id | ConvertTo-Json -Depth 3
$obj | ConvertTo-Json -Compress | Set-Content .\output.json
```

---

### XML を読み込む

```powershell
[xml]$xml = Get-Content <XMLファイルパス>
```

!!! note "説明"
    XML ファイルを型付きオブジェクトとして読み込み、ドットアクセスで要素を参照できます。

#### 使用例

```powershell
[xml]$doc = Get-Content .\settings.xml
$doc.root.item
$doc.SelectNodes("//item[@type='active']")
```

---

## ネットワーク・Web 操作

### 接続を確認する（Test-Connection）

```powershell
Test-Connection <ホスト名またはIPアドレス>
Test-Connection <ホスト名> -Count <回数>
Test-Connection <ホスト名> -Quiet
```

!!! note "説明"
    ICMP（ping）でホストへの接続を確認します。`-Quiet` は `$true` / `$false` を返すので条件分岐に使えます。

#### 使用例

```powershell
Test-Connection google.com
Test-Connection 8.8.8.8 -Count 3
if (Test-Connection github.com -Quiet) { Write-Host "接続OK" }
```

---

### URL からファイルをダウンロードする（Invoke-WebRequest）

```powershell
Invoke-WebRequest -Uri <URL> -OutFile <出力ファイルパス>
Invoke-WebRequest -Uri <URL> -UseBasicParsing
```

!!! note "説明"
    HTTP/HTTPS リクエストを送り、レスポンスを取得します。`-OutFile` でファイルとして保存できます。エイリアス：`iwr` / `curl` / `wget`

#### 使用例

```powershell
Invoke-WebRequest -Uri "https://example.com/file.zip" -OutFile .\file.zip
$res = Invoke-WebRequest -Uri "https://example.com" -UseBasicParsing
$res.StatusCode
```

---

### REST API を呼び出す（Invoke-RestMethod）

```powershell
Invoke-RestMethod -Uri <URL> -Method Get
Invoke-RestMethod -Uri <URL> -Method Post -Body <ボディ> -ContentType "application/json"
Invoke-RestMethod -Uri <URL> -Headers @{ Authorization = "Bearer <トークン>" }
```

!!! note "説明"
    REST API にリクエストを送り、JSON レスポンスを自動でオブジェクトに変換します。エイリアス：`irm`

#### 使用例

```powershell
$repo = Invoke-RestMethod -Uri "https://api.github.com/repos/PowerShell/PowerShell"
$repo.stargazers_count

$body = @{ title = "テスト" } | ConvertTo-Json
Invoke-RestMethod -Uri "https://api.example.com/items" -Method Post -Body $body -ContentType "application/json"
```

---

## ユーティリティコマンド

### クリップボードにコピーする（Set-Clipboard）

```powershell
<コマンド> | Set-Clipboard
Set-Clipboard -Value <テキスト>
```

!!! note "説明"
    コマンドの出力やテキストをクリップボードにコピーします。

#### 使用例

```powershell
Get-Location | Set-Clipboard
"コピーしたいテキスト" | Set-Clipboard
Get-Content .\log.txt | Set-Clipboard
```

---

### クリップボードから取得する（Get-Clipboard）

```powershell
Get-Clipboard
```

!!! note "説明"
    クリップボードの現在の内容を取得します。他のアプリからコピーしたテキストも取得できます。

#### 使用例

```powershell
Get-Clipboard
$text = Get-Clipboard
```

---

### 処理時間を計測する（Measure-Command）

```powershell
Measure-Command { <処理> }
```

!!! note "説明"
    スクリプトブロックの実行時間を計測します。パフォーマンスの比較などに使います。

#### 使用例

```powershell
Measure-Command { Get-ChildItem C:\ -Recurse }
Measure-Command { 1..10000 | ForEach-Object { $_ * 2 } }
```

---

### 処理を一時停止する（Start-Sleep）

```powershell
Start-Sleep -Seconds <秒数>
Start-Sleep -Milliseconds <ミリ秒>
```

!!! note "説明"
    指定した時間だけ処理を待機します。スクリプト内でのポーリングや間隔制御に使います。

#### 使用例

```powershell
Start-Sleep -Seconds 3
Start-Sleep -Milliseconds 500
while ($true) { Get-Date; Start-Sleep -Seconds 5 }
```

---

### ハッシュ値を計算する（Get-FileHash）

```powershell
Get-FileHash <ファイルパス>
Get-FileHash <ファイルパス> -Algorithm <アルゴリズム>
```

!!! note "説明"
    ファイルのハッシュ値を計算します。ダウンロードしたファイルの整合性確認などに使います。
    アルゴリズム：`SHA1` / `SHA256`（デフォルト）/ `SHA384` / `SHA512` / `MD5`

#### 使用例

```powershell
Get-FileHash .\installer.exe
Get-FileHash .\installer.exe -Algorithm MD5
Get-FileHash .\installer.exe -Algorithm SHA256 | Select-Object Hash
```

---

### グリッド形式で表示する（Out-GridView）

```powershell
<コマンド> | Out-GridView
<コマンド> | Out-GridView -Title <タイトル>
<コマンド> | Out-GridView -PassThru
```

!!! note "説明"
    結果を GUI のグリッド（表）で表示します。フィルターや並び替えもできます。`-PassThru` で選択した行をパイプラインに渡せます。

#### 使用例

```powershell
Get-Process | Out-GridView -Title "実行中のプロセス"
Get-Service | Out-GridView -PassThru | Stop-Service
Get-ChildItem | Out-GridView
```

---

### エラー処理をする（try / catch / finally）

```powershell
try {
    <処理> -ErrorAction Stop
} catch {
    Write-Host "エラー: $($_.Exception.Message)"
} finally {
    Write-Host "後処理"
}
```

!!! note "説明"
    例外が発生した場合の処理を定義します。`-ErrorAction Stop` を付けないとエラーが `catch` に届かないことがあります。

#### 使用例

```powershell
try {
    Get-Item .\missing.txt -ErrorAction Stop
} catch [System.IO.FileNotFoundException] {
    Write-Host "ファイルが見つかりません" -ForegroundColor Red
} catch {
    Write-Host "予期しないエラー: $_" -ForegroundColor Yellow
} finally {
    Write-Host "処理を終了します"
}
```

---

### バックグラウンドジョブを実行する（Start-Job）

```powershell
$job = Start-Job -ScriptBlock { <処理> }
Get-Job
Wait-Job -Job $job
Receive-Job -Job $job
Remove-Job -Job $job
```

!!! note "説明"
    処理をバックグラウンドで実行します。時間のかかる処理を非同期で動かすのに使います。

#### 使用例

```powershell
$job = Start-Job -ScriptBlock { Start-Sleep 5; "完了" }
Get-Job
Wait-Job $job
Receive-Job $job
Remove-Job $job
```

---

### リモートでコマンドを実行する（Invoke-Command）

```powershell
Invoke-Command -ComputerName <ホスト名> -ScriptBlock { <コマンド> }
Invoke-Command -ComputerName <ホスト名> -FilePath <スクリプトパス>
Invoke-Command -Session <セッション> -ScriptBlock { <コマンド> }
```

!!! note "説明"
    別のコンピューターでコマンドを実行します。WinRM が有効になっている必要があります。

#### 使用例

```powershell
Invoke-Command -ComputerName Server01 -ScriptBlock { Get-Process }
Invoke-Command -ComputerName Server01, Server02 -ScriptBlock { hostname }
```

---

## オプション（共通パラメーター）

### 共通パラメーター一覧

多くのコマンドレットで共通して使えるパラメーターです。

| オプション | 説明 |
| --- | --- |
| `-Verbose` | 詳細な処理メッセージを表示する |
| `-Debug` | デバッグ用の詳細メッセージを表示する |
| `-ErrorAction Stop` | エラー発生時に例外をスローして処理を中断する |
| `-ErrorAction SilentlyContinue` | エラーを無視して処理を続行する |
| `-ErrorAction Continue` | エラーメッセージを表示して続行する（デフォルト） |
| `-ErrorAction Inquire` | エラー時にユーザーに確認を求める |
| `-WarningAction SilentlyContinue` | 警告を非表示にする |
| `-WhatIf` | 実際には実行せず、実行した場合の動作をシミュレートする |
| `-Confirm` | 実行前に確認プロンプトを表示する |
| `-Force` | 確認なしで強制的に処理を実行する |
| `-PassThru` | 処理した結果のオブジェクトをパイプラインに渡す |
| `-OutVariable <変数名>` | 出力を変数にも同時に保存する |

#### 使用例

```powershell
Remove-Item .\important.txt -WhatIf
Copy-Item .\src .\dst -Force -Verbose
Get-ChildItem -Recurse -ErrorAction SilentlyContinue
```

---

## 設定

### プロファイルを作成・編集する

```powershell
Test-Path $PROFILE
New-Item -Path $PROFILE -ItemType File -Force
code $PROFILE
. $PROFILE
```

!!! note "説明"
    PowerShell の起動時に自動読み込みされる設定ファイルです。エイリアスや環境変数の初期化をここに書いておきます。

#### 使用例

```powershell
# プロファイルの場所を確認する
$PROFILE

# プロファイルが存在するか確認する
Test-Path $PROFILE

# プロファイルを新規作成する
New-Item -Path $PROFILE -ItemType File -Force

# VS Code で開く
code $PROFILE

# プロファイルをリロードする
. $PROFILE
```

---

### スクリプト実行ポリシーを設定する（Set-ExecutionPolicy）

```powershell
Get-ExecutionPolicy
Get-ExecutionPolicy -List
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Set-ExecutionPolicy Bypass -Scope Process
```

!!! note "説明"
    スクリプトの実行を制御するポリシーを設定します。`CurrentUser` スコープなら管理者権限は不要です。

ポリシーの種類：

| ポリシー | 説明 |
| --- | --- |
| `Restricted` | スクリプト実行を禁止（デフォルト） |
| `AllSigned` | 署名済みスクリプトのみ実行可 |
| `RemoteSigned` | ローカルスクリプトはそのまま、リモートは署名必須 |
| `Unrestricted` | すべてのスクリプトを実行可 |
| `Bypass` | ポリシーを無視してすべて実行 |

#### 使用例

```powershell
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Set-ExecutionPolicy Bypass -Scope Process -Force
```

---

### PSReadLine を設定する

```powershell
Set-PSReadLineOption -PredictionSource History
Set-PSReadLineOption -PredictionViewStyle ListView
Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete
```

!!! note "説明"
    コマンド入力補完・履歴表示・キーバインドをカスタマイズします。プロファイルに書いておくと毎回自動で適用されます。

#### 使用例

```powershell
# 履歴を元に入力補完を有効にする
Set-PSReadLineOption -PredictionSource History

# 補完候補をリスト形式で表示する
Set-PSReadLineOption -PredictionViewStyle ListView

# Tab でメニュー補完に切り替える
Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete

# 色をカスタマイズする
Set-PSReadLineOption -Colors @{
    Command   = "Cyan"
    Parameter = "DarkYellow"
    String    = "Green"
    Comment   = "DarkGreen"
}
```

---

### 文字エンコードを UTF-8 に設定する

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
[Console]::InputEncoding  = [System.Text.Encoding]::UTF8
$OutputEncoding            = [System.Text.Encoding]::UTF8
```

!!! note "説明"
    コンソールの入出力エンコードを UTF-8 に統一します。日本語環境では文字化け対策として必須です。プロファイルに書いておくと自動で適用されます。

#### 使用例

```powershell
# プロファイルにそのまま貼り付けて使う
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
[Console]::InputEncoding  = [System.Text.Encoding]::UTF8
$OutputEncoding            = [System.Text.Encoding]::UTF8
```

---

### デフォルトパラメーターを設定する（$PSDefaultParameterValues）

```powershell
$PSDefaultParameterValues["<コマンド>:<パラメーター>"] = <値>
```

!!! note "説明"
    特定のコマンドレットのパラメーターにデフォルト値を設定します。毎回 `-Encoding UTF8` などを書く手間が省けます。プロファイルに書いておくのがおすすめです。

#### 使用例

```powershell
$PSDefaultParameterValues["Export-Csv:Encoding"]          = "UTF8"
$PSDefaultParameterValues["Export-Csv:NoTypeInformation"] = $true
$PSDefaultParameterValues["Out-File:Encoding"]            = "UTF8"
```

---

## 頻出コマンド早引き

```powershell
# ファイルを再帰検索する
Get-ChildItem -Recurse -Filter *.log

# ファイル末尾をリアルタイムで監視する
Get-Content .\app.log -Tail 20 -Wait

# テキストを検索する（grep 相当）
Select-String -Path .\*.log -Pattern "ERROR"

# CPU 使用率が高いプロセスを上位 10 件表示する
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10

# 実行中のサービスを一覧表示する
Get-Service | Where-Object { $_.Status -eq "Running" }

# JSON を読んでプロパティを取得する
(Get-Content .\config.json | ConvertFrom-Json).version

# CSV を読んでフィルタリングして書き出す
Import-Csv .\users.csv | Where-Object { $_.Age -gt 30 } | Export-Csv .\result.csv -NoTypeInformation

# REST API を呼び出す
(Invoke-RestMethod -Uri "https://api.github.com/repos/PowerShell/PowerShell").stargazers_count

# 処理時間を計測する
Measure-Command { Get-ChildItem C:\ -Recurse }

# 出力をクリップボードにコピーする
Get-Location | Set-Clipboard

# ファイルの SHA256 ハッシュを確認する
Get-FileHash .\installer.exe -Algorithm SHA256

# バックグラウンドジョブを実行して結果を受け取る
$job = Start-Job -ScriptBlock { Start-Sleep 10; "完了" }; Wait-Job $job; Receive-Job $job

# プロファイルをリロードする
. $PROFILE
```

---

!!! info "公式ドキュメント"
    - [PowerShell 公式ドキュメント（英語）](https://learn.microsoft.com/en-us/powershell/)
    - [PowerShell 公式ドキュメント（日本語）](https://learn.microsoft.com/ja-jp/powershell/)
    - [PowerShell GitHub リポジトリ](https://github.com/PowerShell/PowerShell)
    - [PowerShell Gallery（モジュール検索）](https://www.powershellgallery.com/)
