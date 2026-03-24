# Python

Python の公式インタープリタ・パッケージマネージャ（`pip` / `uv`）のコマンドを体系的にまとめたリファレンスです。

---

## インストール・アップデート・アンインストール

### Python をインストールする

=== "Windows（winget）"

    ```bash
    winget install Python.Python.3
    ```

=== "macOS（Homebrew）"

    ```bash
    brew install python
    ```

=== "Ubuntu / Debian"

    ```bash
    sudo apt update && sudo apt install python3 python3-pip
    ```

=== "pyenv（バージョン管理）"

    ```bash
    # pyenv 本体をインストール
    curl https://pyenv.run | bash

    # 特定バージョンをインストール
    pyenv install 3.12.0

    # グローバルバージョンを設定
    pyenv global 3.12.0

    # ディレクトリ単位で設定
    pyenv local 3.11.5
    ```

!!! note "説明"
    OS ごとのパッケージマネージャで Python 本体をインストールします。
    `pyenv` を使うと複数バージョンを切り替えて管理できます。

#### 使用例

```bash
# インストール後にバージョンを確認
python --version
python3 --version
```

---

### pip をアップデートする

```bash
python -m pip install --upgrade pip
```

!!! note "説明"
    pip 自体のバージョンを最新に更新します。`python -m pip` 形式で呼び出すと、現在の仮想環境に紐づいた pip が確実に使われます。

#### 使用例

```bash
# アップデートしてバージョンを確認
python -m pip install --upgrade pip
pip --version
```

---

### uv をインストールする

```bash
curl -Lsf https://astral.sh/uv/install.sh | sh
```

!!! note "説明"
    Rust 製の高速パッケージマネージャです。`pip` より大幅に高速で、仮想環境管理・ロックファイル生成・Python バージョン管理も一本化できます。

#### 使用例

```bash
# インストール後にバージョン確認
uv --version

# プロジェクトを初期化して使い始める
uv init myapp
cd myapp
uv add requests
```

---

### パッケージをインストールする

```bash
# pip
pip install パッケージ名

# uv（推奨）
uv add パッケージ名
```

!!! note "説明"
    `pip install` は単体インストール、`uv add` はプロジェクトの `pyproject.toml` に依存として記録しながらインストールします。

#### 使用例

```bash
# pip でインストール
pip install requests
pip install "requests==2.31.0"
pip install -r requirements.txt
pip install -e .              # 開発用（編集可能モード）

# uv でインストール
uv add requests
uv add "requests==2.31.0"
uv add --dev pytest           # 開発用依存として追加
```

---

### パッケージをアップデートする

```bash
# pip
pip install --upgrade パッケージ名

# uv
uv sync --upgrade
```

!!! note "説明"
    `pip install -U`（`--upgrade` の省略形）で特定パッケージを更新します。`uv sync --upgrade` はロックファイルを再解決してすべてを最新化します。

#### 使用例

```bash
# pip：1件アップデート
pip install --upgrade requests

# pip：更新可能なパッケージ一覧を確認
pip list --outdated

# uv：全パッケージを最新化
uv sync --upgrade
```

---

### パッケージをアンインストールする

```bash
# pip
pip uninstall パッケージ名

# uv
uv remove パッケージ名
```

!!! note "説明"
    `pip uninstall` はインタラクティブに確認を求めます。`-y` フラグで確認をスキップできます。

#### 使用例

```bash
# pip：確認あり
pip uninstall requests

# pip：確認なし（-y）
pip uninstall -y requests

# pip：requirements.txt のパッケージを一括削除
pip uninstall -r requirements.txt -y

# uv：プロジェクトから削除
uv remove requests
```

---

## 基本コマンド

### スクリプトを実行する

```bash
python script.py
```

!!! note "説明"
    `.py` ファイルを Python インタープリタで実行します。`python3` コマンドが必要な環境もあります。

#### 使用例

```bash
python hello.py
python3 hello.py

# 引数を渡して実行
python script.py 引数1 引数2

# 環境変数を設定して実行
PYTHONPATH=./lib python script.py
```

---

### インタラクティブシェル（REPL）を起動する

```bash
python
```

!!! note "説明"
    引数なしで `python` を起動すると対話シェル（REPL）が開きます。コードを1行ずつ実行して結果を確認できます。終了は `exit()` または `quit()`。

#### 使用例

```text
$ python
>>> print("Hello, World!")
Hello, World!
>>> 1 + 2
3
>>> import sys; sys.version
'3.12.0 ...'
>>> exit()
```

---

### モジュールをスクリプトとして実行する

```bash
python -m モジュール名
```

!!! note "説明"
    `-m` フラグでインストール済みモジュールをコマンドとして実行します。標準ライブラリのツールをワンコマンドで使えます。

#### 使用例

```bash
# 静的ファイルサーバーを起動（ポート 8000）
python -m http.server 8000

# JSON を整形して表示
echo '{"a":1,"b":2}' | python -m json.tool

# pip をモジュールとして実行（仮想環境で確実に使う）
python -m pip install requests

# pdb デバッガを起動
python -m pdb script.py

# cProfile でプロファイリング
python -m cProfile -s cumulative script.py
```

---

### ワンライナーを実行する

```bash
python -c "コード"
```

!!! note "説明"
    `-c` フラグで1行のコードをコマンドラインから直接実行します。シェルスクリプトとの組み合わせに便利です。

#### 使用例

```bash
python -c "print('Hello')"
python -c "import sys; print(sys.version)"
python -c "print(sum(range(1, 101)))"
python -c "import os; print(os.getcwd())"
```

---

### ヘルプを表示する

```bash
python --help
```

!!! note "説明"
    コマンドラインオプション一覧を表示します。REPL 内では `help()` 関数で組み込み関数・モジュールのドキュメントを確認できます。

#### 使用例

```bash
# コマンドラインオプション一覧
python --help

# REPL 内でのヘルプ確認
python -c "help(print)"
python -c "help(list)"
```

---

## よく使うコマンド

### 仮想環境を作成・有効化する

```bash
python -m venv .venv
source .venv/bin/activate
```

!!! note "説明"
    プロジェクト専用の独立した Python 環境を作成します。有効化することで、そのプロジェクトにだけパッケージをインストールできます。

#### 使用例

```bash
# 仮想環境を作成
python -m venv .venv

# 有効化（Linux / macOS）
source .venv/bin/activate

# 有効化（Windows PowerShell）
.venv\Scripts\Activate.ps1

# 無効化
deactivate
```

---

### uv でプロジェクトを管理する

```bash
uv init プロジェクト名
uv sync
uv run python script.py
```

!!! note "説明"
    `uv init` で `pyproject.toml` を生成し、`uv sync` で依存パッケージをインストールします。`uv run` で仮想環境内のコマンドを実行します。

#### 使用例

```bash
# プロジェクトを初期化
uv init myapp
cd myapp

# パッケージを追加して同期
uv add fastapi
uv add --dev pytest ruff mypy

# スクリプトを実行
uv run python main.py

# テストを実行
uv run pytest -v

# 依存ツリーを確認
uv tree
```

---

### パッケージ一覧を確認する

```bash
pip list
pip freeze
```

!!! note "説明"
    `pip list` はインストール済みパッケージを表形式で表示します。`pip freeze` は `requirements.txt` 形式で出力します。

#### 使用例

```bash
# インストール済み一覧
pip list

# 更新可能なパッケージ一覧
pip list --outdated

# requirements.txt 形式で表示
pip freeze

# requirements.txt として保存
pip freeze > requirements.txt

# requirements.txt からインストール
pip install -r requirements.txt
```

---

### パッケージ情報を確認する

```bash
pip show パッケージ名
```

!!! note "説明"
    パッケージのバージョン・依存関係・インストール場所を確認します。`pip check` で依存関係の整合性チェックもできます。

#### 使用例

```bash
pip show requests
pip show --verbose requests

# 依存関係の整合性をチェック
pip check
```

---

### スクリプト実行後に REPL を起動する

```bash
python -i script.py
```

!!! note "説明"
    スクリプトを実行した後、そのスコープを保ったまま REPL に入ります。変数やオブジェクトをインタラクティブに検査するのに便利です。

#### 使用例

```bash
python -i script.py

# スクリプト内で定義した変数をそのまま使える
>>> print(my_variable)
>>> some_function()
```

---

### JSON を整形する

```bash
echo 'JSON文字列' | python -m json.tool
```

!!! note "説明"
    標準ライブラリの `json.tool` モジュールで JSON を整形・検証します。API レスポンスの確認などに便利です。

#### 使用例

```bash
# 標準入力から整形
echo '{"name":"Alice","age":30}' | python -m json.tool

# ファイルから整形
python -m json.tool data.json

# インデント幅を指定
python -m json.tool --indent 2 data.json
```

---

### HTTP サーバーを起動する

```bash
python -m http.server ポート番号
```

!!! note "説明"
    カレントディレクトリを静的ファイルサーバーとして公開します。HTML の確認やファイル共有に手軽に使えます。

#### 使用例

```bash
# ポート 8000 で起動（デフォルト）
python -m http.server

# ポートを指定
python -m http.server 3000

# ループバックアドレスにのみバインド（外部に公開しない）
python -m http.server 8000 --bind 127.0.0.1
```

---

## 上級者向けコマンド

### cProfile でプロファイリングする

```bash
python -m cProfile script.py
```

!!! note "説明"
    関数ごとの呼び出し回数・実行時間を計測します。ボトルネック特定に使います。`-s` でソート順を指定できます。

#### 使用例

```bash
# 累積時間順にソートして表示
python -m cProfile -s cumulative script.py

# 結果をファイルに保存
python -m cProfile -o output.prof script.py

# 上位20件だけ確認
python -m cProfile -s cumulative script.py | head -20
```

---

### timeit で速度を計測する

```bash
python -m timeit "コード"
```

!!! note "説明"
    小さなコードスニペットの実行時間を統計的に計測します。複数回実行して最良・平均値を算出します。

#### 使用例

```bash
python -m timeit "'-'.join(str(n) for n in range(100))"
python -m timeit "'-'.join([str(n) for n in range(100)])"

# 実行回数を指定
python -m timeit -n 1000 "sum(range(1000))"

# セットアップコードを指定
python -m timeit -s "import re" "re.match('[0-9]+', '123')"
```

---

### pdb でデバッグする

```bash
python -m pdb script.py
```

!!! note "説明"
    Python 標準のコマンドラインデバッガです。ブレークポイント設定・ステップ実行・変数確認ができます。

#### 使用例

```bash
python -m pdb script.py
```

```text
(Pdb) n        # 次の行へ（ステップオーバー）
(Pdb) s        # 関数内へ入る（ステップイン）
(Pdb) c        # 次のブレークポイントまで実行
(Pdb) p 変数名  # 変数の値を表示
(Pdb) l        # 現在行周辺のソースを表示
(Pdb) b 行番号  # ブレークポイントを設定
(Pdb) q        # デバッガを終了
```

---

### dis でバイトコードを確認する

```bash
python -m dis script.py
```

!!! note "説明"
    Python ソースコードをバイトコード（CPython の内部命令）に変換して表示します。最適化の研究やコードの挙動理解に使います。

#### 使用例

```bash
python -m dis script.py

# REPL でも使える
python -c "import dis; dis.dis(lambda x: x * 2)"
```

---

### compileall でコンパイルする

```bash
python -m compileall ディレクトリ
```

!!! note "説明"
    `.py` ファイルをバイトコード（`.pyc`）にコンパイルします。初回インポート時間の短縮に使います。

#### 使用例

```bash
# ディレクトリ以下を一括コンパイル
python -m compileall ./src

# 単一ファイルをコンパイル
python -m py_compile script.py
```

---

### pydoc でドキュメントを生成・確認する

```bash
python -m pydoc モジュール名
```

!!! note "説明"
    Python モジュールのドキュメントをターミナルや HTML で表示します。`-p` でローカルサーバーを起動できます。

#### 使用例

```bash
# ターミナルに表示
python -m pydoc os
python -m pydoc requests

# HTML ファイルとして出力
python -m pydoc -w os

# ローカルサーバーで全モジュールを閲覧
python -m pydoc -p 8080
```

---

### zipapp でスクリプトをまとめる

```bash
python -m zipapp ディレクトリ -o 出力ファイル.pyz -m "モジュール:関数"
```

!!! note "説明"
    Python スクリプトをひとつの `.pyz` ファイルに固めて配布可能にします。

#### 使用例

```bash
python -m zipapp myapp/ -o myapp.pyz -m "main:main"
python myapp.pyz
```

---

### mypy で型チェックする

```bash
uv run mypy script.py
```

!!! note "説明"
    静的型チェッカーです。型アノテーションを検証してバグを事前に検出します。`--strict` で厳格モードになります。

#### 使用例

```bash
uv add --dev mypy

uv run mypy script.py
uv run mypy --strict script.py
uv run mypy ./src
```

---

### ruff でコードを整形・チェックする

```bash
uv run ruff check .
uv run ruff format .
```

!!! note "説明"
    Rust 製の高速リンター＆フォーマッタです。`flake8` / `isort` / `black` の役割を一本化できます。

#### 使用例

```bash
uv add --dev ruff

# リント（コードチェック）
uv run ruff check .

# 自動修正
uv run ruff check --fix .

# フォーマット
uv run ruff format .

# 変更内容をプレビュー（実際には変更しない）
uv run ruff format --diff .
```

---

### pytest でテストを実行する

```bash
uv run pytest
```

!!! note "説明"
    Python の標準的なテストフレームワークです。`tests/` ディレクトリや `test_*.py` を自動検出して実行します。

#### 使用例

```bash
uv add --dev pytest

# 全テストを実行
uv run pytest

# 詳細表示
uv run pytest -v

# 特定ファイルを実行
uv run pytest tests/test_main.py

# 失敗したテストだけ再実行
uv run pytest --lf

# カバレッジ付きで実行
uv add --dev pytest-cov
uv run pytest --cov=src
```

---

## 知ってると便利なコマンド

### UUID を生成する

```bash
python -c "import uuid; print(uuid.uuid4())"
```

!!! note "説明"
    衝突しないランダムな識別子（UUID v4）を生成します。データベースの主キーやセッション ID に使われます。

#### 使用例

```bash
python -c "import uuid; print(uuid.uuid4())"
# → e3b0c442-98fc-1c14-9afb-f4c8996fb924

# 複数生成
python -c "import uuid; [print(uuid.uuid4()) for _ in range(5)]"
```

---

### ランダム文字列を生成する

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

!!! note "説明"
    暗号学的に安全なランダム文字列を生成します。`secrets` モジュールはパスワードやトークン生成に適しています。

#### 使用例

```bash
# 16進数文字列（32バイト = 64文字）
python -c "import secrets; print(secrets.token_hex(32))"

# URL セーフ文字列
python -c "import secrets; print(secrets.token_urlsafe(32))"

# ランダムなパスワード（英数字 + 記号）
python -c "
import secrets, string
chars = string.ascii_letters + string.digits + string.punctuation
print(''.join(secrets.choice(chars) for _ in range(20)))
"
```

---

### ハッシュ値を計算する

```bash
python -c "import hashlib; print(hashlib.sha256(b'データ').hexdigest())"
```

!!! note "説明"
    `hashlib` モジュールで MD5・SHA-256 などのハッシュ値を計算します。ファイルの整合性チェックに使います。

#### 使用例

```bash
python -c "import hashlib; print(hashlib.md5(b'hello').hexdigest())"
python -c "import hashlib; print(hashlib.sha256(b'hello').hexdigest())"

# ファイルのハッシュを計算
python -c "
import hashlib
with open('file.txt', 'rb') as f:
    print(hashlib.sha256(f.read()).hexdigest())
"
```

---

### Base64 エンコード・デコードする

```bash
python -c "import base64; print(base64.b64encode(b'テキスト').decode())"
```

!!! note "説明"
    バイナリデータを ASCII テキストに変換します。メール添付やデータ URI などで使われます。

#### 使用例

```bash
# エンコード
python -c "import base64; print(base64.b64encode(b'hello').decode())"
# → aGVsbG8=

# デコード
python -c "import base64; print(base64.b64decode('aGVsbG8=').decode())"
# → hello
```

---

### Python のパスを確認する

```bash
python -c "import sys; print('\n'.join(sys.path))"
```

!!! note "説明"
    モジュール検索パスの一覧を表示します。インポートエラーが起きたときに確認します。

#### 使用例

```bash
# モジュール検索パス
python -c "import sys; print('\n'.join(sys.path))"

# Python 本体の場所
which python

# 標準ライブラリの場所
python -c "import os; print(os.__file__)"

# site-packages の場所
python -c "import site; print(site.getsitepackages())"
```

---

### 環境変数を確認する

```bash
python -c "import os; print(os.environ.get('変数名'))"
```

!!! note "説明"
    `os.environ` で環境変数にアクセスします。`.get()` を使うと、変数が存在しない場合に `None` を返せます。

#### 使用例

```bash
python -c "import os; print(os.environ.get('PATH'))"
python -c "import os; print(os.environ.get('PYTHONPATH', '未設定'))"

# 全環境変数を一覧表示
python -c "import os; [print(f'{k}={v}') for k, v in sorted(os.environ.items())]"
```

---

### カレンダーを表示する

```bash
python -m calendar
```

!!! note "説明"
    標準ライブラリの `calendar` モジュールで年間・月間カレンダーをターミナルに表示します。

#### 使用例

```bash
# 今年のカレンダーを表示
python -m calendar

# 特定の年月を表示
python -c "import calendar; print(calendar.month(2025, 12))"
```

---

### SMTP デバッグサーバーを起動する

```bash
python -m smtpd -n -c DebuggingServer localhost:1025
```

!!! note "説明"
    受信したメールをターミナルに出力するだけのダミー SMTP サーバーです。メール送信機能の開発・テストに使います。

#### 使用例

```bash
# デバッグ用 SMTP サーバーを起動
python -m smtpd -n -c DebuggingServer localhost:1025

# アプリ側で localhost:1025 に送信すると、ターミナルに内容が表示される
```

---

### IPython でリッチな REPL を使う

```bash
uv run ipython
```

!!! note "説明"
    標準 REPL を拡張した対話環境です。シンタックスハイライト・自動補完・マジックコマンドが使えます。

#### 使用例

```bash
uv add ipython
uv run ipython
```

```text
# よく使うマジックコマンド（IPython 内）
%timeit sum(range(1000))   # 速度計測
%run script.py             # スクリプト実行
%history                   # コマンド履歴
%who                       # 定義済み変数一覧
%pwd                       # カレントディレクトリ
```

---

## オプション一覧

### `python` コマンドの主要オプション

| オプション | 説明 |
| --- | --- |
| `-c コード` | 1行のコードを直接実行 |
| `-m モジュール` | モジュールをスクリプトとして実行 |
| `-i` | スクリプト実行後に REPL を起動 |
| `-u` | 標準出力・標準エラーをバッファなしに |
| `-v` | インポートされたモジュールを詳細表示 |
| `-vv` | より詳細なトレース表示 |
| `-O` | アサーションを無効化して最適化 |
| `-OO` | docstring も削除して最適化 |
| `-B` | `.pyc` ファイルを生成しない |
| `-S` | `site.py` の自動インポートを無効化 |
| `-E` | 環境変数（PYTHONPATH 等）を無視 |
| `-W フィルタ` | 警告フィルタを設定 |
| `-X utf8` | UTF-8 モードを有効化 |
| `--version` | バージョンを表示 |
| `--help` | ヘルプを表示 |

### `pip` コマンドの主要オプション

| サブコマンド | 説明 |
| --- | --- |
| `install パッケージ` | パッケージをインストール |
| `install -U パッケージ` | アップグレードしながらインストール |
| `install -r requirements.txt` | requirements.txt から一括インストール |
| `install -e .` | 編集可能モードでインストール（開発用） |
| `install --no-cache-dir` | キャッシュを使わずインストール |
| `install --user` | ユーザーディレクトリにインストール |
| `uninstall パッケージ` | アンインストール |
| `uninstall -y パッケージ` | 確認なしでアンインストール |
| `list` | インストール済みパッケージ一覧 |
| `list --outdated` | 更新可能なパッケージ一覧 |
| `show パッケージ` | パッケージの詳細情報 |
| `freeze` | requirements.txt 形式で出力 |
| `check` | 依存関係の整合性チェック |
| `download パッケージ` | ダウンロードのみ（インストールしない） |
| `cache list` | キャッシュ一覧を表示 |
| `cache purge` | キャッシュを全削除 |

### `uv` コマンドの主要オプション

| サブコマンド | 説明 |
| --- | --- |
| `uv init` | プロジェクトを初期化 |
| `uv add パッケージ` | 依存パッケージを追加 |
| `uv add --dev パッケージ` | 開発用パッケージを追加 |
| `uv remove パッケージ` | パッケージを削除 |
| `uv sync` | 環境を pyproject.toml に同期 |
| `uv sync --upgrade` | 全パッケージを最新化して同期 |
| `uv run コマンド` | uv 環境内でコマンド実行 |
| `uv pip install` | pip 互換のインストール |
| `uv pip freeze` | pip freeze 相当の出力 |
| `uv python install 3.12` | Python バージョンをインストール |
| `uv python list` | 利用可能な Python 一覧 |
| `uv lock` | ロックファイルを生成・更新 |
| `uv tree` | 依存ツリーを表示 |
| `uv cache clean` | キャッシュを削除 |

### 環境変数一覧

| 環境変数 | 説明 |
| --- | --- |
| `PYTHONPATH` | モジュール検索パスを追加 |
| `PYTHONSTARTUP` | REPL 起動時に実行するスクリプトのパス |
| `PYTHONDONTWRITEBYTECODE` | `1` にすると `.pyc` ファイルを生成しない |
| `PYTHONUNBUFFERED` | `1` にすると出力のバッファリングを無効化 |
| `PYTHONIOENCODING` | 標準入出力のエンコーディングを設定 |
| `PYTHONUTF8` | `1` にすると UTF-8 モードを有効化 |
| `PIP_DEFAULT_TIMEOUT` | pip のタイムアウト秒数 |
| `PIP_INDEX_URL` | デフォルトのパッケージインデックス URL |
| `UV_PYTHON` | uv で使用する Python バージョン |

---

## 設定

### pip.conf を設定する

```ini
[global]
# タイムアウト（秒）
timeout = 60

# デフォルトインデックス URL
index-url = https://pypi.org/simple/

# 追加インデックス（プライベートリポジトリ）
extra-index-url = https://private.example.com/simple/

# キャッシュディレクトリ
cache-dir = /tmp/pip-cache

[install]
upgrade = false
```

!!! note "説明"
    `pip.conf`（Linux / macOS）または `pip.ini`（Windows）に設定を書くと、毎回オプションを指定しなくて済みます。

#### ファイルの場所

```text
Linux / macOS : ~/.config/pip/pip.conf
Windows       : %APPDATA%\pip\pip.ini
```

---

### pyproject.toml を設定する

```toml
[project]
name = "myapp"
version = "0.1.0"
description = "サンプルプロジェクト"
readme = "README.md"

# Python バージョン要件
requires-python = ">=3.11"

# 本番依存パッケージ
dependencies = [
    "requests>=2.31.0",
    "fastapi>=0.110.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.0.0",
    "mypy>=1.9.0",
    "ruff>=0.3.0",
]

[tool.uv]
dev-dependencies = [
    "pytest>=8.0.0",
]

[tool.ruff]
# リンター設定
line-length = 88
target-version = "py311"

[tool.mypy]
# 型チェック設定
python_version = "3.11"
strict = true

[tool.pytest.ini_options]
# テスト設定
testpaths = ["tests"]
addopts = "-v"
```

!!! note "説明"
    `uv init` で生成される設定ファイルです。`pip` / `mypy` / `ruff` / `pytest` の設定を1ファイルにまとめられます。

#### 使用例

```bash
# pyproject.toml を元に環境を同期
uv sync

# 開発用依存も含めて同期
uv sync --all-extras
```

---

### .python-version を設定する

```text
3.12.0
```

!!! note "説明"
    `pyenv local` や `uv` が参照するバージョン指定ファイルです。プロジェクトルートに置くと、そのディレクトリで自動的に指定バージョンが使われます。

#### 使用例

```bash
# pyenv でローカルバージョンを設定（.python-version を自動生成）
pyenv local 3.12.0

# uv も .python-version を参照して Python を選択
uv run python --version
```

---

### PYTHONSTARTUP で REPL をカスタマイズする

```bash
# ~/.bashrc または ~/.zshrc に追記
export PYTHONSTARTUP=~/.pythonrc
```

```python
# ~/.pythonrc
import readline
import rlcompleter
import os
import atexit
from pprint import pprint  # noqa: F401（REPL 内でそのまま使えるようにしておく）

# タブ補完を有効化
readline.parse_and_bind("tab: complete")

# コマンド履歴をファイルに保存
histfile = os.path.join(os.path.expanduser("~"), ".python_history")
try:
    readline.read_history_file(histfile)
except FileNotFoundError:
    pass

atexit.register(readline.write_history_file, histfile)

print("Python REPL 準備完了（tab補完・履歴保存 有効）")
```

!!! note "説明"
    `PYTHONSTARTUP` に指定したスクリプトは REPL 起動時に自動で実行されます。補完・履歴・よく使うインポートを自動化できます。

#### 使用例

```bash
# 設定後に REPL を起動すると自動でカスタマイズが適用される
python
# → Python REPL 準備完了（tab補完・履歴保存 有効）
```

---

## 頻出コマンド早引き

```bash
# ── バージョン確認 ────────────────────────────────────────
python --version

# ── スクリプト実行 ────────────────────────────────────────
python script.py
python -m モジュール名
python -c "print('Hello')"
python -i script.py            # 実行後に REPL を起動

# ── 仮想環境 ──────────────────────────────────────────────
python -m venv .venv
source .venv/bin/activate      # 有効化（Linux / macOS）
deactivate                     # 無効化

# ── uv ────────────────────────────────────────────────────
uv init myapp
uv add requests
uv add --dev pytest ruff mypy
uv sync
uv run python script.py
uv run pytest -v
uv tree

# ── pip ───────────────────────────────────────────────────
pip install requests
pip install -r requirements.txt
pip freeze > requirements.txt
pip list --outdated
pip show requests
pip uninstall -y requests

# ── デバッグ・計測 ────────────────────────────────────────
python -m pdb script.py
python -m cProfile -s cumulative script.py
python -m timeit "sum(range(1000))"

# ── 便利ツール ────────────────────────────────────────────
python -m http.server 8000
echo '{"a":1}' | python -m json.tool
python -c "import uuid; print(uuid.uuid4())"
python -c "import secrets; print(secrets.token_urlsafe(32))"
```

!!! info "公式ドキュメント"
    - [Python 公式ドキュメント（日本語）](https://docs.python.org/ja/3/)
    - [pip ドキュメント](https://pip.pypa.io/en/stable/)
    - [uv ドキュメント](https://docs.astral.sh/uv/)
    - [pyenv GitHub](https://github.com/pyenv/pyenv)
    - [ruff ドキュメント](https://docs.astral.sh/ruff/)
    - [mypy ドキュメント](https://mypy.readthedocs.io/en/stable/)
    - [pytest ドキュメント](https://docs.pytest.org/en/stable/)
