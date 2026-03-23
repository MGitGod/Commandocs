# Ollama CLI

Ollama CLI はローカル環境で LLMのダウンロード・実行・管理・公開を行うためのツールです。

## 主な用途

- LLMモデルの実行
- モデルの管理
- カスタムモデルの作成
- APIサーバーの起動
- モデルの共有

---

## 基本コマンド

Ollama CLI の基本操作やヘルプ表示を行うコマンドです。

- **CLIヘルプを表示 (help)**

  ```bash
  ollama help
  ```

- **バージョンを表示 (version)**

  ```bash
  ollama --version
  ```

- **バージョンを短縮表示 (version)**

  ```bash
  ollama -v
  ```

---

## モデル実行

ローカルで LLM を起動して対話するコマンドです。

- **モデルを実行 (run)**

  ```bash
  ollama run <model>

  # 例
  ollama run llama3
  ```

- **プロンプト付き実行 (run + prompt)**

  ```bash
  ollama run llama3 "Hello"
  ```

- **ファイル内容を入力 (run + ファイル入力)**

  ```bash
  ollama run llama3 < prompt.txt
  ```

- **temperature 指定 (run + temperature)**

  ```bash
  ollama run llama3 --temperature 0.7
  ```

- **system prompt 指定 (run + system prompt)**

  ```bash
  ollama run llama3 --system "You are a helpful assistant"
  ```

- **context 保持 (run + keepalive)**

  ```bash
  ollama run llama3 --keepalive 10m
  ```

---

## モデル管理

ローカルに保存されているモデルを管理するコマンドです。

- **インストール済みモデル一覧 (list)**

  ```bash
  ollama list
  ```

- **一覧を短縮表示 (ls)**

  ```bash
  ollama ls
  ```

- **モデル情報表示 (show)**

  ```bash
  ollama show <model>

  # 例
  ollama show llama3
  ```

- **モデル削除 (rm)**

  ```bash
  ollama rm <model>

  # 例
  ollama rm llama3
  ```

- **モデルコピー (cp)**

  ```bash
  ollama cp <source> <target>

  # 例
  ollama cp llama3 my-assistant
  ```

- **モデルダウンロード (pull)**

  ```bash
  ollama pull <model>

  # 例
  ollama pull mistral
  ```

- **モデルアップロード (push)**

  ```bash
  ollama push <model>
  ```

---

## モデル作成

Modelfile を使ってカスタムモデルを作成するコマンドです。

- **モデル作成 (create)**

  ```bash
  ollama create <model> -f Modelfile

  # 例
  ollama create mybot -f Modelfile
  ```

- **ログ表示付きで作成 (create verbose)**

  ```bash
  ollama create mybot -f Modelfile --verbose
  ```

---

## モデル実行管理

現在実行中のモデルの管理コマンドです。

- **実行中モデル一覧 (ps)**

  ```bash
  ollama ps
  ```

- **実行中モデル停止 (stop)**

  ```bash
  ollama stop <model>

  # 例
  ollama stop llama3
  ```

- **全モデル停止 (stop all)**

  ```bash
  ollama stop --all
  ```

---

## サーバー管理

Ollama の API サーバーを起動するコマンドです。

- **API サーバー起動 (serve)**

  ```bash
  ollama serve
  ```

- **ポート指定で起動 (serve + port)**

  ```bash
  OLLAMA_HOST=0.0.0.0:11434 ollama serve
  ```

- **バックグラウンド起動 (serve + background)**

  ```bash
  nohup ollama serve &
  ```

---

## レジストリ操作

Ollama Hub とモデルを共有するコマンドです。

- **モデル公開 (push)**

  ```bash
  ollama push <model>
  ```

- **モデル取得 (pull)**

  ```bash
  ollama pull <model>
  ```

- **バージョン指定で取得 (pull + version)**

  ```bash
  ollama pull llama3:8b
  ```

---

## Modelfile 操作

カスタム LLM を作るための設定ファイルです。

- **モデル作成 (create model)**

  ```bash
  ollama create mymodel -f Modelfile
  ```

- **Modelfile の例**

  ```bash
  FROM llama3

  PARAMETER temperature 0.7

  SYSTEM You are a helpful assistant.
  ```

---

## モデル情報取得

モデルの構成情報を確認するコマンドです。

- **パラメータ表示 (show parameters)**

  ```bash
  ollama show llama3
  ```

- **Modelfile 表示 (show modelfile)**

  ```bash
  ollama show llama3 --modelfile
  ```

- **ライセンス表示 (show license)**

  ```bash
  ollama show llama3 --license
  ```

---

## 環境変数

Ollama の挙動を制御する環境変数です。

- **ポート設定**

  ```bash
  export OLLAMA_HOST=0.0.0.0:11434
  ```

- **GPU 設定**

  ```bash
  export OLLAMA_GPU=1
  ```

- **keepalive 設定**

  ```bash
  export OLLAMA_KEEP_ALIVE=10m
  ```

---

## API 連携

Ollama は HTTP API で操作可能です。

- **generate**

  ```bash
  curl http://localhost:11434/api/generate -d '{
    "model": "llama3",
    "prompt": "Hello"
  }'
  ```

- **chat**

  ```bash
  curl http://localhost:11434/api/chat -d '{
    "model": "llama3",
    "messages": [
      {"role":"user","content":"Hello"}
    ]
  }'
  ```

- **embeddings**

  ```bash
  curl http://localhost:11434/api/embeddings -d '{
    "model": "nomic-embed-text",
    "prompt": "hello world"
  }'
  ```

---

## よく使う開発ワークフロー

- **モデルダウンロード**

  ```bash
  ollama pull llama3
  ```

- **モデル実行**

  ```bash
  ollama run llama3
  ```

- **モデル一覧**

  ```bash
  ollama list
  ```

- **実行中モデル確認**

  ```bash
  ollama ps
  ```

- **モデル停止**

  ```bash
  ollama stop llama3
  ```

- **モデル削除**

  ```bash
  ollama rm llama3
  ```
