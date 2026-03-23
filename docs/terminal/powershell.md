# PowerShell

PowerShell の主要コマンド（コマンドレット）をまとめたリファレンスです。
Windows / PowerShell 7（pwsh）両対応。コマンドはすべてコピーしてそのまま使えます。

---

## インストール・アップデート・アンインストール

### PowerShell 7（pwsh）のインストール

winget（Windows パッケージマネージャー）でインストールするのが最も簡単です。

```powershell
winget install --id Microsoft.PowerShell --source winget
```

スコープをユーザー単位に限定してインストールする場合：

```powershell
winget install --id Microsoft.PowerShell --scope user
```

### PowerShell 7 のアップデート

```powershell
winget upgrade --id Microsoft.PowerShell
```

### PowerShell 7 のアンインストール

```powershell
winget uninstall --id Microsoft.PowerShell
```

### モジュールのインストール（PowerShellGet）

PSGallery から任意のモジュールをインストールします。

```powershell
Install-Module -Name <モジュール名> -Scope CurrentUser
```

使い方の例：

```powershell
Install-Module -Name posh-git -Scope CurrentUser
```

### モジュールのアップデート

```powershell
Update-Module -Name <モジュール名>
```

すべてのモジュールを一括アップデートする場合：

```powershell
Get-InstalledModule | Update-Module
```

### モジュールのアンインストール

```powershell
Uninstall-Module -Name <モジュール名>
```

バージョンを指定してアンインストールする場合：

```powershell
Uninstall-Module -Name <モジュール名> -RequiredVersion <バージョン>
```

---

## 基本コマンド

### ヘルプを表示する（Get-Help）

コマンドレットの使い方を確認します。

```powershell
Get-Help <コマンド名>
```

使い方の例：

```powershell
Get-Help Get-Process
Get-Help Get-Process -Full
Get-Help Get-Process -Examples
Get-Help Get-Process -Online
```

オフライン用のヘルプデータを更新する場合：

```powershell
Update-Help -Force
```

### コマンドを検索する（Get-Command）

使えるコマンドレット・関数・エイリアスを一覧表示します。

```powershell
Get-Command
Get-Command -Name <パターン>
Get-Command -Verb Get
Get-Command -Noun Process
```

使い方の例：

```powershell
Get-Command -Name *process*
Get-Command -Module Microsoft.PowerShell.Management
```

### エイリアス一覧（Get-Alias）

短縮コマンド（エイリアス）の一覧を表示します。

```powershell
Get-Alias
Get-Alias -Name <エイリアス名>
Get-Alias -Definition <コマンドレット名>
```

使い方の例：

```powershell
Get-Alias -Name ls
Get-Alias -Definition Get-ChildItem
```

### 画面クリア（Clear-Host）

```powershell
Clear-Host
```

エイリアス：`cls` / `clear`

---

## よく使うコマンド

### ファイル・ディレクトリ一覧（Get-ChildItem）

```powershell
Get-ChildItem
Get-ChildItem -Path <パス>
Get-ChildItem -Recurse
Get-ChildItem -Filter <パターン>
Get-ChildItem -Hidden
Get-ChildItem -File
Get-ChildItem -Directory
```

使い方の例：

```powershell
Get-ChildItem -Path C:\Users -Recurse -Filter *.log
Get-ChildItem . -Hidden
```

エイリアス：`ls` / `dir` / `gci`

### 現在のディレクトリを確認（Get-Location）

```powershell
Get-Location
```

エイリアス：`pwd`

### ディレクトリ移動（Set-Location）

```powershell
Set-Location <パス>
Set-Location ..
Set-Location ~
```

エイリアス：`cd` / `chdir` / `sl`

### ファイル内容を表示（Get-Content）

```powershell
Get-Content <ファイルパス>
Get-Content <ファイルパス> -TotalCount <行数>
Get-Content <ファイルパス> -Tail <行数>
Get-Content <ファイルパス> -Wait
```

使い方の例：

```powershell
Get-Content .\log.txt -Tail 20
Get-Content .\app.log -Wait
```

エイリアス：`cat` / `type` / `gc`

### ファイルにテキストを書き込む（Set-Content / Add-Content）

上書きして書き込む場合：

```powershell
Set-Content -Path <ファイルパス> -Value <テキスト>
```

末尾に追記する場合：

```powershell
Add-Content -Path <ファイルパス> -Value <テキスト>
```

使い方の例：

```powershell
Set-Content -Path .\output.txt -Value "Hello, World!"
Add-Content -Path .\log.txt -Value "$(Get-Date) - 処理完了"
```

### ファイル・フォルダのコピー（Copy-Item）

```powershell
Copy-Item <コピー元> <コピー先>
Copy-Item <コピー元> <コピー先> -Recurse
Copy-Item <コピー元> <コピー先> -Force
```

使い方の例：

```powershell
Copy-Item .\file.txt .\backup\file.txt
Copy-Item .\src -Destination .\dst -Recurse
```

エイリアス：`cp` / `copy` / `cpi`

### ファイル・フォルダの移動・名前変更（Move-Item）

```powershell
Move-Item <移動元> <移動先>
```

使い方の例：

```powershell
Move-Item .\old.txt .\archive\old.txt
Move-Item .\old_name.txt .\new_name.txt
```

エイリアス：`mv` / `move` / `mi`

### ファイル・フォルダの削除（Remove-Item）

```powershell
Remove-Item <パス>
Remove-Item <パス> -Recurse
Remove-Item <パス> -Force
Remove-Item <パス> -Recurse -Force
```

使い方の例：

```powershell
Remove-Item .\temp.txt
Remove-Item .\old_folder -Recurse -Force
```

エイリアス：`rm` / `del` / `erase` / `rd` / `ri`

### 新規ファイル・フォルダの作成（New-Item）

```powershell
New-Item -Path <パス> -ItemType File
New-Item -Path <パス> -ItemType Directory
New-Item -Path <パス> -ItemType File -Value <内容>
```

使い方の例：

```powershell
New-Item -Path .\notes.txt -ItemType File
New-Item -Path .\MyProject -ItemType Directory
New-Item -Path .\hello.txt -ItemType File -Value "Hello!"
```

エイリアス：`ni`

### プロセス一覧（Get-Process）

```powershell
Get-Process
Get-Process -Name <プロセス名>
Get-Process -Id <PID>
```

使い方の例：

```powershell
Get-Process -Name chrome
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
```

エイリアス：`ps` / `gps`

### プロセスを終了（Stop-Process）

```powershell
Stop-Process -Name <プロセス名>
Stop-Process -Id <PID>
Stop-Process -Name <プロセス名> -Force
```

使い方の例：

```powershell
Stop-Process -Name notepad
Stop-Process -Id 1234 -Force
```

エイリアス：`kill` / `spps`

### サービス一覧・操作（Get-Service / Start-Service / Stop-Service）

```powershell
Get-Service
Get-Service -Name <サービス名>
Start-Service -Name <サービス名>
Stop-Service -Name <サービス名>
Restart-Service -Name <サービス名>
```

使い方の例：

```powershell
Get-Service -Name wuauserv
Stop-Service -Name Spooler
Restart-Service -Name W32Time
```

### 変数の代入と表示

```powershell
$変数名 = <値>
$変数名
Write-Output $変数名
```

使い方の例：

```powershell
$name = "PowerShell"
$num = 42
Write-Output "こんにちは、$name！"
```

### 文字列のフォーマット出力（Write-Host）

```powershell
Write-Host <テキスト>
Write-Host <テキスト> -ForegroundColor <色>
Write-Host <テキスト> -BackgroundColor <色>
```

使い方の例：

```powershell
Write-Host "エラーが発生しました" -ForegroundColor Red
Write-Host "処理完了" -ForegroundColor Green
```

---

## 上級者向けコマンド

### パイプラインとフィルタリング（Where-Object）

条件に一致するオブジェクトだけ取り出します。

```powershell
<コマンド> | Where-Object { <条件式> }
```

使い方の例：

```powershell
Get-Process | Where-Object { $_.CPU -gt 10 }
Get-Service | Where-Object { $_.Status -eq "Running" }
Get-ChildItem | Where-Object { $_.Length -gt 1MB }
```

エイリアス：`where` / `?`

### プロパティの選択（Select-Object）

```powershell
<コマンド> | Select-Object <プロパティ名1>, <プロパティ名2>
<コマンド> | Select-Object -First <件数>
<コマンド> | Select-Object -Last <件数>
<コマンド> | Select-Object -Unique
<コマンド> | Select-Object -ExpandProperty <プロパティ名>
```

使い方の例：

```powershell
Get-Process | Select-Object Name, CPU, WorkingSet | Sort-Object CPU -Descending
Get-ChildItem | Select-Object -First 5
```

エイリアス：`select`

### 並び替え（Sort-Object）

```powershell
<コマンド> | Sort-Object <プロパティ名>
<コマンド> | Sort-Object <プロパティ名> -Descending
<コマンド> | Sort-Object <プロパティ1>, <プロパティ2>
```

使い方の例：

```powershell
Get-ChildItem | Sort-Object LastWriteTime -Descending
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
```

エイリアス：`sort`

### グループ化・集計（Group-Object / Measure-Object）

グループ化する場合：

```powershell
<コマンド> | Group-Object <プロパティ名>
```

集計する場合：

```powershell
<コマンド> | Measure-Object
<コマンド> | Measure-Object -Property <プロパティ名> -Sum -Average -Maximum -Minimum
```

使い方の例：

```powershell
Get-Process | Group-Object -Property Company
Get-ChildItem -Recurse -File | Measure-Object -Property Length -Sum
```

### オブジェクトのプロパティ確認（Get-Member）

オブジェクトが持つプロパティ・メソッドを確認します。

```powershell
<コマンド> | Get-Member
<コマンド> | Get-Member -MemberType Property
<コマンド> | Get-Member -MemberType Method
```

使い方の例：

```powershell
Get-Process | Get-Member
"Hello" | Get-Member
```

エイリアス：`gm`

### ForEach-Object（繰り返し処理）

```powershell
<コマンド> | ForEach-Object { <処理> }
```

使い方の例：

```powershell
1..5 | ForEach-Object { Write-Host "番号: $_" }
Get-ChildItem *.txt | ForEach-Object { Copy-Item $_ .\backup\ }
```

エイリアス：`foreach` / `%`

### CSV の読み書き

CSV から読み込む場合：

```powershell
Import-Csv -Path <CSVファイルパス>
Import-Csv -Path <CSVファイルパス> -Encoding UTF8
```

CSV に書き出す場合：

```powershell
<コマンド> | Export-Csv -Path <出力パス> -NoTypeInformation
<コマンド> | Export-Csv -Path <出力パス> -NoTypeInformation -Encoding UTF8
```

使い方の例：

```powershell
$data = Import-Csv -Path .\users.csv -Encoding UTF8
$data | Where-Object { $_.Age -gt 30 } | Export-Csv -Path .\senior.csv -NoTypeInformation
```

### JSON の読み書き

JSON から読み込む場合：

```powershell
Get-Content <JSONファイルパス> | ConvertFrom-Json
```

JSON に変換する場合：

```powershell
<コマンド> | ConvertTo-Json
<コマンド> | ConvertTo-Json -Depth <深さ>
```

使い方の例：

```powershell
$config = Get-Content .\config.json | ConvertFrom-Json
$config.version
$obj = @{ name = "test"; value = 123 }
$obj | ConvertTo-Json | Set-Content .\output.json
```

### XML の読み書き

XML として読み込む場合：

```powershell
[xml]$xml = Get-Content <XMLファイルパス>
```

XML に変換する場合：

```powershell
<コマンド> | ConvertTo-Xml
```

使い方の例：

```powershell
[xml]$doc = Get-Content .\settings.xml
$doc.root.item
```

### レジストリへのアクセス

```powershell
Get-Item -Path HKLM:\Software\<キーパス>
Get-ItemProperty -Path HKLM:\Software\<キーパス>
Set-ItemProperty -Path HKCU:\Software\<キーパス> -Name <名前> -Value <値>
New-Item -Path HKCU:\Software\<キーパス>
Remove-Item -Path HKCU:\Software\<キーパス> -Recurse
```

使い方の例：

```powershell
Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion"
```

### リモートコマンド実行（Invoke-Command）

```powershell
Invoke-Command -ComputerName <ホスト名> -ScriptBlock { <コマンド> }
Invoke-Command -ComputerName <ホスト名> -FilePath <スクリプトパス>
```

使い方の例：

```powershell
Invoke-Command -ComputerName Server01 -ScriptBlock { Get-Process }
```

### ジョブの実行（Start-Job / Get-Job / Receive-Job）

バックグラウンドでジョブを実行します。

```powershell
$job = Start-Job -ScriptBlock { <処理> }
Get-Job
Receive-Job -Job $job
Wait-Job -Job $job
Remove-Job -Job $job
```

使い方の例：

```powershell
$job = Start-Job -ScriptBlock { Start-Sleep 5; "完了" }
Wait-Job $job
Receive-Job $job
```

### エラーハンドリング（try / catch / finally）

```powershell
try {
    <処理>
} catch {
    Write-Host "エラー: $_"
} finally {
    Write-Host "後処理"
}
```

使い方の例：

```powershell
try {
    Get-Item .\missing.txt -ErrorAction Stop
} catch {
    Write-Host "ファイルが見つかりません: $($_.Exception.Message)" -ForegroundColor Red
} finally {
    Write-Host "処理を終了します"
}
```

### スクリプトブロックの呼び出し（Invoke-Expression）

文字列をコマンドとして実行します。

```powershell
Invoke-Expression <コマンド文字列>
```

使い方の例：

```powershell
$cmd = "Get-Process -Name chrome"
Invoke-Expression $cmd
```

エイリアス：`iex`

---

## 知ってると便利なコマンド

### 文字列の検索（Select-String）

ファイルや出力からパターンに一致する行を抽出します（grep 相当）。

```powershell
Select-String -Path <ファイルパス> -Pattern <正規表現>
Select-String -Path <ファイルパス> -Pattern <パターン> -CaseSensitive
<コマンド> | Select-String <パターン>
```

使い方の例：

```powershell
Select-String -Path .\*.log -Pattern "ERROR"
Get-Content .\app.log | Select-String "timeout"
```

エイリアス：`sls`

### クリップボードへのコピー（Set-Clipboard）

```powershell
<コマンド> | Set-Clipboard
Set-Clipboard -Value <テキスト>
```

使い方の例：

```powershell
Get-Location | Set-Clipboard
"コピーしたいテキスト" | Set-Clipboard
```

### クリップボードから取得（Get-Clipboard）

```powershell
Get-Clipboard
```

### 処理時間を計測（Measure-Command）

```powershell
Measure-Command { <処理> }
```

使い方の例：

```powershell
Measure-Command { Get-ChildItem C:\ -Recurse }
```

### 一時停止（Start-Sleep）

```powershell
Start-Sleep -Seconds <秒数>
Start-Sleep -Milliseconds <ミリ秒>
```

使い方の例：

```powershell
Start-Sleep -Seconds 3
Start-Sleep -Milliseconds 500
```

### 環境変数の確認・設定

環境変数を一覧表示する場合：

```powershell
Get-ChildItem Env:
$Env:<変数名>
```

環境変数を設定する場合（セッション内のみ）：

```powershell
$Env:<変数名> = <値>
```

使い方の例：

```powershell
$Env:PATH
$Env:MY_APP_DIR = "C:\MyApp"
```

### ネットワーク接続確認（Test-Connection）

```powershell
Test-Connection <ホスト名またはIPアドレス>
Test-Connection <ホスト名> -Count <回数>
Test-Connection <ホスト名> -Quiet
```

使い方の例：

```powershell
Test-Connection google.com
Test-Connection 8.8.8.8 -Count 3
Test-Connection github.com -Quiet
```

### URL からファイルをダウンロード（Invoke-WebRequest）

```powershell
Invoke-WebRequest -Uri <URL> -OutFile <出力ファイルパス>
```

使い方の例：

```powershell
Invoke-WebRequest -Uri "https://example.com/file.zip" -OutFile .\file.zip
```

エイリアス：`iwr` / `curl` / `wget`

### REST API を叩く（Invoke-RestMethod）

```powershell
Invoke-RestMethod -Uri <URL> -Method Get
Invoke-RestMethod -Uri <URL> -Method Post -Body <ボディ> -ContentType "application/json"
```

使い方の例：

```powershell
$res = Invoke-RestMethod -Uri "https://api.github.com/repos/PowerShell/PowerShell"
$res.name
$res.stargazers_count
```

エイリアス：`irm`

### ハッシュ値の計算（Get-FileHash）

```powershell
Get-FileHash <ファイルパス>
Get-FileHash <ファイルパス> -Algorithm <アルゴリズム>
```

アルゴリズムの種類：`SHA1` / `SHA256` / `SHA384` / `SHA512` / `MD5`

使い方の例：

```powershell
Get-FileHash .\installer.exe
Get-FileHash .\installer.exe -Algorithm MD5
```

### テキストをグリッド形式で表示（Out-GridView）

```powershell
<コマンド> | Out-GridView
<コマンド> | Out-GridView -Title <タイトル>
<コマンド> | Out-GridView -PassThru
```

使い方の例：

```powershell
Get-Process | Out-GridView -Title "実行中のプロセス"
Get-Service | Out-GridView -PassThru | Stop-Service
```

### プロファイルの場所を確認

```powershell
$PROFILE
$PROFILE.AllUsersAllHosts
$PROFILE.CurrentUserAllHosts
$PROFILE.CurrentUserCurrentHost
```

### スクリプト実行ポリシーの確認と変更（Set-ExecutionPolicy）

現在のポリシーを確認する場合：

```powershell
Get-ExecutionPolicy
Get-ExecutionPolicy -List
```

ポリシーを変更する場合：

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Set-ExecutionPolicy Bypass -Scope Process
```

ポリシーの種類：

| ポリシー | 説明 |
| --- | --- |
| `Restricted` | スクリプト実行を禁止（デフォルト） |
| `AllSigned` | 署名済みスクリプトのみ実行可 |
| `RemoteSigned` | ローカルスクリプトはそのまま、リモートは署名必須 |
| `Unrestricted` | すべてのスクリプトを実行可 |
| `Bypass` | ポリシーを無視してすべて実行 |

### 再起動・シャットダウン・ロック

再起動する場合：

```powershell
Restart-Computer
Restart-Computer -Force
```

シャットダウンする場合：

```powershell
Stop-Computer
Stop-Computer -Force
```

画面をロックする場合：

```powershell
rundll32.exe user32.dll,LockWorkStation
```

---

## オプション（共通フラグ）

各コマンドレットで使える共通パラメーターの一覧です。

| オプション | 説明 |
| --- | --- |
| `-Verbose` | 詳細な処理メッセージを表示する |
| `-Debug` | デバッグ用の詳細メッセージを表示する |
| `-ErrorAction Stop` | エラー発生時に例外をスローして処理を中断する |
| `-ErrorAction SilentlyContinue` | エラーを無視して処理を続行する |
| `-ErrorAction Continue` | エラーメッセージを表示して続行する（デフォルト） |
| `-ErrorAction Inquire` | エラー時にユーザーに確認を求める |
| `-WarningAction SilentlyContinue` | 警告を非表示にする |
| `-InformationAction Continue` | 情報メッセージを表示する |
| `-WhatIf` | 実際には実行せず、実行した場合の動作をシミュレートする |
| `-Confirm` | 実行前に確認プロンプトを表示する |
| `-Force` | 確認なしで強制的に処理を実行する |
| `-PassThru` | 処理した結果のオブジェクトをパイプラインに渡す |
| `-OutVariable <変数名>` | 出力を変数にも同時に保存する |
| `-PipelineVariable <変数名>` | パイプラインの各要素を変数にバインドする |

使い方の例：

```powershell
Remove-Item .\important.txt -WhatIf
Copy-Item .\src .\dst -Force -Verbose
Get-Process | Where-Object { $_.CPU -gt 5 } -ErrorAction SilentlyContinue
```

---

## 設定

### プロファイルの作成・編集

プロファイルが存在するか確認する場合：

```powershell
Test-Path $PROFILE
```

プロファイルを新規作成する場合：

```powershell
New-Item -Path $PROFILE -ItemType File -Force
```

プロファイルを VS Code で開く場合：

```powershell
code $PROFILE
```

プロファイルをメモ帳で開く場合：

```powershell
notepad $PROFILE
```

プロファイルを即時リロードする場合：

```powershell
. $PROFILE
```

### プロファイルに書くと便利な設定例

```powershell
# ===== PowerShell プロファイル設定 =====

# プロンプトのカスタマイズ
function prompt {
    $path = $(Get-Location).Path.Replace($HOME, "~")
    "PS $path> "
}

# よく使うエイリアスを登録
Set-Alias -Name which -Value Get-Command
Set-Alias -Name touch -Value New-Item

# 環境変数の設定
$Env:EDITOR = "code"

# PSReadLine オプション（補完・履歴設定）
Set-PSReadLineOption -PredictionSource History
Set-PSReadLineOption -PredictionViewStyle ListView
Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete

# Git 情報を表示（posh-git を使用する場合）
# Import-Module posh-git
```

### PSReadLine の設定（入力補完・キーバインド）

```powershell
# 履歴を元にした入力補完を有効にする
Set-PSReadLineOption -PredictionSource History

# 補完候補をリスト形式で表示する
Set-PSReadLineOption -PredictionViewStyle ListView

# Tab キーでメニュー補完に切り替える
Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete

# 色のカスタマイズ
Set-PSReadLineOption -Colors @{
    Command   = "Cyan"
    Parameter = "DarkYellow"
    String    = "Green"
    Comment   = "DarkGreen"
}
```

### モジュールパスの確認・追加

モジュールのインストールパスを確認する場合：

```powershell
$Env:PSModulePath -split ";"
```

モジュールパスを追加する場合（セッション内のみ）：

```powershell
$Env:PSModulePath += ";C:\MyModules"
```

### PowerShell のバージョン確認

```powershell
$PSVersionTable
$PSVersionTable.PSVersion
```

### カルチャー（言語・ロケール）の確認と設定

```powershell
Get-Culture
[System.Threading.Thread]::CurrentThread.CurrentCulture = "ja-JP"
```

### コンソールの文字エンコードを UTF-8 に設定

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
[Console]::InputEncoding  = [System.Text.Encoding]::UTF8
$OutputEncoding            = [System.Text.Encoding]::UTF8
```

プロファイルに書いておくと毎回自動で設定されます。

### デフォルトのパラメーター値を設定（$PSDefaultParameterValues）

```powershell
# すべての Export-Csv で自動的に UTF8 + ヘッダなしを適用する
$PSDefaultParameterValues["Export-Csv:Encoding"]           = "UTF8"
$PSDefaultParameterValues["Export-Csv:NoTypeInformation"]  = $true

# すべての Out-File で UTF8 を適用する
$PSDefaultParameterValues["Out-File:Encoding"] = "UTF8"
```

---

> 📝 **メモ**
> PowerShell 7（pwsh）は Windows・macOS・Linux すべてで動作します。
> Windows PowerShell 5.1 との互換性に注意が必要なコマンドがあります（例：`-Encoding UTF8BOM` の挙動の違いなど）。
