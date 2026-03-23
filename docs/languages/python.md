# Python

---

## インストール・アップデート・アンインストール

### Python 本体のインストール

#### Windows（winget）

```bash
winget install Python.Python.3
```

#### macOS（Homebrew）

```bash
brew install python
```

#### Ubuntu / Debian

```bash
sudo apt update && sudo apt install python3 python3-pip
```

#### pyenv（バージョン管理ツール）

```bash
# pyenv のインストール（Linux / macOS）
curl https://pyenv.run | bash

# 特定バージョンのインストール
pyenv install 3.12.0

# グローバルバージョンの設定
pyenv global 3.12.0

# ローカル（ディレクトリ単位）バージョンの設定
pyenv local 3.11.5
```

### バージョン確認

```bash
python --version
python3 --version
```

**使い方の例：**

```text
$ python --version
Python 3.12.0
```

### pip のインストール・アップデート

```bash
# pip 自体をアップデート
python -m pip install --upgrade pip

# pip のバージョン確認
pip --version
```

### uv（高速パッケージマネージャ）のインストール

```bash
# uv のインストール（推奨）
curl -Lsf https://astral.sh/uv/install.sh | sh

# バージョン確認
uv --version
```

### パッケージのインストール（pip）

```bash
# 1件インストール
pip install requests

# バージョン指定
pip install requests==2.31.0

# requirements.txt から一括インストール
pip install -r requirements.txt

# ローカルパッケージのインストール（開発用）
pip install -e .
```

### パッケージのインストール（uv）

```bash
# プロジェクトへのパッケージ追加
uv add requests

# バージョン指定
uv add requests==2.31.0

# 開発用依存として追加
uv add --dev pytest

# requirements.txt から同期
uv sync
```

### パッケージのアップデート

```bash
# pip：1件アップデート
pip install --upgrade requests

# pip：全パッケージアップデート（pip-review 使用）
pip install pip-review
pip-review --auto

# uv：全パッケージを最新に同期
uv sync --upgrade
```

### パッケージのアンインストール

```bash
# pip：1件削除
pip uninstall requests

# pip：確認なしで削除
pip uninstall -y requests

# pip：requirements.txt のパッケージを一括削除
pip uninstall -r requirements.txt -y

# uv：パッケージ削除
uv remove requests
```

---

## 基本コマンド

### スクリプトの実行

```bash
python script.py
python3 script.py
```

**使い方の例：**

```bash
python hello.py
# → Hello, World!
```

### インタラクティブシェル（REPL）の起動

```bash
python
python3
```

**使い方の例：**

```text
$ python
>>> print("Hello")
Hello
>>> 1 + 2
3
>>> exit()
```

### モジュールとして実行

```bash
python -m モジュール名
```

**使い方の例：**

```bash
# HTTP サーバーを起動（ポート8000）
python -m http.server 8000

# JSON を整形して表示
echo '{"key":"value"}' | python -m json.tool

# pip をモジュールとして実行
python -m pip install requests
```

### ワンライナー（1行コードの実行）

```bash
python -c "コード"
```

**使い方の例：**

```bash
python -c "print('Hello')"
python -c "import sys; print(sys.version)"
python -c "print(sum(range(1, 101)))"
```

### ヘルプの表示

```bash
# コマンドラインオプション一覧
python --help

# 組み込み関数・クラスのヘルプ（REPL内）
help(print)
help(list)
help("keywords")
```

---

## よく使うコマンド

### パッケージ一覧の確認

```bash
# インストール済み一覧
pip list

# requirements.txt 形式で出力
pip freeze

# requirements.txt として保存
pip freeze > requirements.txt
```

### パッケージ情報の確認

```bash
pip show requests
```

**使い方の例：**

```text
$ pip show requests
Name: requests
Version: 2.31.0
Location: /usr/lib/python3/dist-packages
Requires: certifi, charset-normalizer, idna, urllib3
```

### 仮想環境の作成・有効化・無効化

```bash
# 仮想環境の作成
python -m venv .venv

# 有効化（Linux / macOS）
source .venv/bin/activate

# 有効化（Windows PowerShell）
.venv\Scripts\Activate.ps1

# 無効化
deactivate
```

**使い方の例：**

```bash
python -m venv .venv
source .venv/bin/activate
pip install requests
deactivate
```

### uv による仮想環境管理

```bash
# プロジェクト初期化（pyproject.toml 生成）
uv init myproject

# 仮想環境を作成して依存をインストール
uv sync

# uv 環境内でコマンド実行
uv run python script.py
uv run pytest
```

### スクリプトへの引数渡し

```bash
python script.py 引数1 引数2
```

**使い方の例：**

```bash
python greet.py Alice 25
```

```python
# greet.py
import sys

# コマンドライン引数を受け取る
name = sys.argv[1]
age = sys.argv[2]
print(f"こんにちは、{name}さん（{age}歳）！")
```

### スクリプトのパス確認・検索

```bash
# Python 本体のパス
which python
which python3

# インストール先ディレクトリ
python -c "import site; print(site.getsitepackages())"
```

### タイムゾーン・ロケールの確認

```bash
python -c "import locale; print(locale.getlocale())"
python -c "import time; print(time.tzname)"
```

---

## 上級者向けコマンド

### プロファイリング（処理時間計測）

```bash
# cProfile で処理時間を計測
python -m cProfile script.py

# ソート順を指定（cumulative：累積時間順）
python -m cProfile -s cumulative script.py

# 結果をファイルに保存
python -m cProfile -o output.prof script.py

# pstats で保存結果を分析
python -m pstats output.prof
```

**使い方の例：**

```bash
python -m cProfile -s cumulative heavy_script.py | head -20
```

### timeit（小さなコードの速度計測）

```bash
python -m timeit "コード"
```

**使い方の例：**

```bash
python -m timeit "'-'.join(str(n) for n in range(100))"
python -m timeit -n 1000 "sum(range(1000))"
```

### pdb（デバッガの起動）

```bash
python -m pdb script.py
```

**使い方の例（主な pdb コマンド）：**

```text
(Pdb) n        # 次の行へ（ステップオーバー）
(Pdb) s        # 関数内に入る（ステップイン）
(Pdb) c        # 次のブレークポイントまで実行
(Pdb) p 変数名  # 変数の値を表示
(Pdb) l        # 現在行周辺のソースを表示
(Pdb) q        # 終了
```

### コードのコンパイル（.pyc 生成）

```bash
# 単一ファイル
python -m py_compile script.py

# ディレクトリ以下を一括コンパイル
python -m compileall ./src
```

### 逆コンパイル・逆アセンブル

```bash
python -m dis script.py
```

**使い方の例：**

```bash
python -c "import dis; dis.dis(lambda x: x + 1)"
```

### zipapp（スクリプトを zip に固める）

```bash
python -m zipapp myapp/ -o myapp.pyz -m "main:main"
python myapp.pyz
```

### AST（抽象構文木）の確認

```bash
python -c "import ast; print(ast.dump(ast.parse('x = 1 + 2')))"
```

### unittest の実行

```bash
# カレントディレクトリ以下のテストを自動検出して実行
python -m unittest discover

# 特定ファイルを実行
python -m unittest test_module.py

# 詳細表示
python -m unittest -v test_module
```

### pytest の実行（uv 経由）

```bash
uv add --dev pytest

# テスト実行
uv run pytest

# 詳細表示
uv run pytest -v

# 特定ファイルを実行
uv run pytest tests/test_main.py

# 失敗したテストだけ再実行
uv run pytest --lf
```

### 型チェック（mypy）

```bash
uv add --dev mypy
uv run mypy script.py
uv run mypy --strict script.py
```

### コードフォーマット（ruff）

```bash
uv add --dev ruff

# フォーマット
uv run ruff format .

# リント（コードチェック）
uv run ruff check .

# 自動修正
uv run ruff check --fix .
```

### ドキュメント生成（pydoc）

```bash
# ターミナルに表示
python -m pydoc モジュール名

# HTML を生成
python -m pydoc -w モジュール名

# ローカルサーバーで閲覧（ポート指定）
python -m pydoc -p 8080
```

---

## 知ってると便利なコマンド

### 環境変数の確認（os.environ）

```bash
python -c "import os; print(os.environ.get('PATH'))"
python -c "import os; [print(k, '=', v) for k, v in os.environ.items()]"
```

### Pythonのパスを確認

```bash
python -c "import sys; print('\n'.join(sys.path))"
```

### JSON のフォーマット（整形）

```bash
# 標準入力から
echo '{"a":1,"b":2}' | python -m json.tool

# ファイルから
python -m json.tool data.json

# インデント幅を指定
python -m json.tool --indent 2 data.json
```

### HTTP サーバー（静的ファイルを手軽に配信）

```bash
# デフォルトポート 8000
python -m http.server

# ポートを指定
python -m http.server 3000

# バインドするアドレスを指定
python -m http.server 8000 --bind 127.0.0.1
```

### SMTP デバッグサーバー（メール送信のテスト）

```bash
python -m smtpd -n -c DebuggingServer localhost:1025
```

### Base64 エンコード・デコード

```bash
# エンコード
python -c "import base64; print(base64.b64encode(b'hello').decode())"

# デコード
python -c "import base64; print(base64.b64decode('aGVsbG8=').decode())"
```

### カレンダーの表示

```bash
python -m calendar

# 特定の年月
python -c "import calendar; print(calendar.month(2025, 4))"
```

### UUID の生成

```bash
python -c "import uuid; print(uuid.uuid4())"
```

### ハッシュ値の生成

```bash
python -c "import hashlib; print(hashlib.md5(b'hello').hexdigest())"
python -c "import hashlib; print(hashlib.sha256(b'hello').hexdigest())"
```

### パッケージの依存関係を表示

```bash
pip show --verbose requests
pip check  # 依存関係の不整合チェック
```

### 標準ライブラリの場所を確認

```bash
python -c "import os; print(os.__file__)"
python -c "import json; print(json.__file__)"
```

### インタラクティブシェルをより快適に（IPython）

```bash
uv add ipython
uv run ipython
```

### ファイル内容をバイト単位で処理

```bash
python -c "
with open('file.txt', 'rb') as f:
    data = f.read()
    print(len(data), 'bytes')
"
```

### ランダム文字列の生成

```bash
python -c "import secrets; print(secrets.token_hex(16))"
python -c "import secrets; print(secrets.token_urlsafe(16))"
```

---

## オプション一覧

### `python` コマンドの主要オプション

| オプション | 説明 | 使い方の例 |
| --- | --- | --- |
| `-c コード` | 1行のコードを直接実行 | `python -c "print(1+1)"` |
| `-m モジュール` | モジュールをスクリプトとして実行 | `python -m http.server` |
| `-i` | スクリプト実行後にREPLを起動 | `python -i script.py` |
| `-u` | 標準出力・標準エラーをバッファなしに | `python -u script.py` |
| `-v` | インポートされたモジュールを詳細表示 | `python -v script.py` |
| `-vv` | より詳細なトレース表示 | `python -vv script.py` |
| `-O` | アサーションを無効化して最適化 | `python -O script.py` |
| `-OO` | docstring も削除して最適化 | `python -OO script.py` |
| `-B` | `.pyc` ファイルを生成しない | `python -B script.py` |
| `-S` | `site.py` の自動インポートを無効化 | `python -S script.py` |
| `-E` | 環境変数（PYTHONPATH等）を無視 | `python -E script.py` |
| `-W フィルタ` | 警告フィルタを設定 | `python -W ignore script.py` |
| `-X オプション` | 実装固有オプションを有効化 | `python -X utf8 script.py` |
| `--version` | バージョンを表示 | `python --version` |
| `--help` | ヘルプを表示 | `python --help` |

### `pip` コマンドの主要オプション

| オプション | 説明 | 使い方の例 |
| --- | --- | --- |
| `install` | パッケージをインストール | `pip install requests` |
| `install -U` | アップグレードしながらインストール | `pip install -U requests` |
| `install -r` | requirements.txt から一括インストール | `pip install -r requirements.txt` |
| `install -e` | 編集可能モードでインストール（開発用） | `pip install -e .` |
| `install --no-cache-dir` | キャッシュを使わずインストール | `pip install --no-cache-dir requests` |
| `install --user` | ユーザーディレクトリにインストール | `pip install --user requests` |
| `uninstall` | アンインストール | `pip uninstall requests` |
| `uninstall -y` | 確認なしでアンインストール | `pip uninstall -y requests` |
| `list` | インストール済みパッケージ一覧 | `pip list` |
| `list --outdated` | 更新可能なパッケージ一覧 | `pip list --outdated` |
| `show` | パッケージの詳細情報 | `pip show requests` |
| `freeze` | requirements.txt 形式で出力 | `pip freeze > requirements.txt` |
| `check` | 依存関係の整合性チェック | `pip check` |
| `search` | PyPI でパッケージを検索 | `pip search requests` |
| `download` | パッケージをダウンロードのみ | `pip download requests -d ./pkgs` |
| `wheel` | wheel ファイルを生成 | `pip wheel requests` |
| `cache list` | キャッシュ一覧を表示 | `pip cache list` |
| `cache purge` | キャッシュを全削除 | `pip cache purge` |

### `uv` コマンドの主要オプション

| オプション | 説明 | 使い方の例 |
| --- | --- | --- |
| `uv init` | プロジェクトを初期化 | `uv init myapp` |
| `uv add` | 依存パッケージを追加 | `uv add requests` |
| `uv add --dev` | 開発用パッケージを追加 | `uv add --dev pytest` |
| `uv remove` | パッケージを削除 | `uv remove requests` |
| `uv sync` | 環境を pyproject.toml に同期 | `uv sync` |
| `uv sync --upgrade` | 全パッケージを最新に更新して同期 | `uv sync --upgrade` |
| `uv run` | uv 環境内でコマンド実行 | `uv run python script.py` |
| `uv pip install` | pip 互換のインストール | `uv pip install requests` |
| `uv pip freeze` | pip freeze 相当の出力 | `uv pip freeze` |
| `uv python install` | Python バージョンをインストール | `uv python install 3.12` |
| `uv python list` | 利用可能な Python 一覧 | `uv python list` |
| `uv lock` | ロックファイルを生成・更新 | `uv lock` |
| `uv tree` | 依存ツリーを表示 | `uv tree` |
| `uv cache clean` | キャッシュを削除 | `uv cache clean` |

---

## 設定

### 環境変数による設定

| 環境変数 | 説明 | 設定例 |
| --- | --- | --- |
| `PYTHONPATH` | モジュール検索パスを追加 | `export PYTHONPATH=/mylib:$PYTHONPATH` |
| `PYTHONSTARTUP` | REPL 起動時に実行するスクリプト | `export PYTHONSTARTUP=~/.pythonrc` |
| `PYTHONDONTWRITEBYTECODE` | `.pyc` ファイルを生成しない | `export PYTHONDONTWRITEBYTECODE=1` |
| `PYTHONUNBUFFERED` | 出力のバッファリングを無効化 | `export PYTHONUNBUFFERED=1` |
| `PYTHONIOENCODING` | 標準入出力のエンコーディングを設定 | `export PYTHONIOENCODING=utf-8` |
| `PYTHONUTF8` | UTF-8 モードを有効化（Python 3.7+） | `export PYTHONUTF8=1` |
| `VIRTUAL_ENV` | 仮想環境のパス（自動設定） | （仮想環境有効化時に自動設定） |
| `PIP_DEFAULT_TIMEOUT` | pip のタイムアウト秒数 | `export PIP_DEFAULT_TIMEOUT=60` |
| `PIP_INDEX_URL` | デフォルトのパッケージインデックス URL | `export PIP_INDEX_URL=https://pypi.org/simple/` |
| `UV_PYTHON` | uv で使用する Python バージョン | `export UV_PYTHON=3.12` |

### `pip.ini` / `pip.conf` の設定

**場所（Linux / macOS）：** `~/.config/pip/pip.conf`

**場所（Windows）：** `%APPDATA%\pip\pip.ini`

```ini
[global]
# タイムアウトを設定（秒）
timeout = 60

# デフォルトインデックスを変更
index-url = https://pypi.org/simple/

# 追加のインデックスを設定
extra-index-url = https://private.example.com/simple/

# キャッシュディレクトリを変更
cache-dir = /tmp/pip-cache

# デフォルトで --user を有効化
user = yes

[install]
# インストール時は常にアップグレード確認
upgrade = false
```

### `pyproject.toml` の設定（uv プロジェクト）

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
# 開発用依存パッケージ
dev = [
    "pytest>=8.0.0",
    "mypy>=1.9.0",
    "ruff>=0.3.0",
]

[tool.uv]
# uv の設定
dev-dependencies = [
    "pytest>=8.0.0",
]

[tool.ruff]
# ruff リンターの設定
line-length = 88
target-version = "py311"

[tool.mypy]
# mypy 型チェックの設定
python_version = "3.11"
strict = true

[tool.pytest.ini_options]
# pytest の設定
testpaths = ["tests"]
addopts = "-v"
```

### `.python-version` ファイル（pyenv / uv）

```text
3.12.0
```

```bash
# pyenv でローカルバージョンを設定（.python-version が自動生成される）
pyenv local 3.12.0

# uv でも .python-version を参照
uv run python --version
```

### `sitecustomize.py`（起動時に自動実行）

**場所：** `site-packages/sitecustomize.py`

```python
# Python 起動時に自動で実行されるカスタマイズ
import sys

# デフォルトエンコーディングを UTF-8 に設定（Python 3.7 以前向け）
# sys.stdout.reconfigure(encoding='utf-8')

# デバッグ用：起動時にメッセージを表示
print("[sitecustomize] Python が起動しました", file=sys.stderr)
```

### REPL 起動時の自動実行スクリプト（`PYTHONSTARTUP`）

```bash
# ~/.bashrc または ~/.zshrc に追加
export PYTHONSTARTUP=~/.pythonrc
```

```python
# ~/.pythonrc
# REPL をより便利にする設定

import readline
import rlcompleter

# タブ補完を有効化
readline.parse_and_bind("tab: complete")

# 履歴ファイルを設定
import os
import atexit

histfile = os.path.join(os.path.expanduser("~"), ".python_history")
try:
    readline.read_history_file(histfile)
except FileNotFoundError:
    pass

atexit.register(readline.write_history_file, histfile)

# よく使うモジュールを事前インポート
import os
import sys
from pprint import pprint

print("Python REPL 準備完了（tab補完・履歴保存 有効）")
```

---

## 頻出コマンド早見表

```bash
# バージョン確認
python --version

# スクリプト実行
python script.py

# ワンライナー実行
python -c "print('Hello')"

# モジュールとして実行
python -m http.server 8000

# 仮想環境の作成
python -m venv .venv

# 仮想環境の有効化（Linux / macOS）
source .venv/bin/activate

# uv でプロジェクト初期化
uv init myapp

# uv でパッケージ追加
uv add requests

# uv でスクリプト実行
uv run python script.py

# pip でパッケージインストール
pip install requests

# pip 一覧表示
pip list

# pip 情報表示
pip show requests

# requirements.txt に書き出し
pip freeze > requirements.txt

# requirements.txt からインストール
pip install -r requirements.txt

# テスト実行（pytest）
uv run pytest -v

# JSON 整形
echo '{"a":1}' | python -m json.tool

# cProfile でプロファイリング
python -m cProfile -s cumulative script.py

# pdb でデバッグ
python -m pdb script.py
```

---

### 最終更新：2026年3月23日
