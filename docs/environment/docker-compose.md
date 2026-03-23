# Docker Compose

このページでは、Docker Composeでよく使用するコマンドを目的別にまとめています。

## 基本コマンド

Docker Composeを利用する上で、最も基本的なコマンドです。

### コンテナの作成と起動

`docker-compose.yml` ファイルの内容に従って、コンテナを作成し起動します。

```bash
# フォアグラウンドで起動する（ログがそのまま表示される）
docker compose up
```

```bash
# バックグラウンドで起動する（通常はこちらをよく使います）
docker compose up -d
```

### コンテナの停止

稼働中のコンテナを停止します。コンテナやネットワークは削除されません。

```bash
# サービス群を停止する
docker compose stop
```

### コンテナの停止とリソースの削除

コンテナを停止し、作成されたコンテナ、ネットワーク、ボリューム（オプション）を削除します。

```bash
# コンテナとネットワークを削除する
docker compose down
```

```bash
# ボリュームも一緒に削除する（データが消えるので注意）
docker compose down -v
```

## よく使うコマンド

開発時や運用時によく使用するコマンドです。

### コンテナの状態確認

現在起動している（または停止している）コンテナの一覧と状態を確認します。

```bash
# コンテナのステータスを確認する
docker compose ps
```

```bash
# すべてのコンテナ（停止中も含む）を確認する
docker compose ps -a
```

### ログの確認

各コンテナの標準出力を確認します。エラーの原因調査などに役立ちます。

```bash
# すべてのサービスのログを表示する
docker compose logs
```

```bash
# 継続してログを監視する（Ctrl+Cで終了）
docker compose logs -f
```

```bash
# 特定のサービス（例: db）のログのみを表示する
docker compose logs db
```

### コンテナ内でのコマンド実行

起動しているコンテナの中で、任意のコマンドを実行します。

```bash
# webコンテナの中でbashを実行し、中に入る
docker compose exec web bash
```

```bash
# dbコンテナでMySQLのクライアントを起動する
docker compose exec db mysql -u root -p
```

## 上級者向けコマンド

特定の状況や、より細やかな制御が必要な場合に使用するコマンドです。

### イメージのビルド

`Dockerfile` を元に、イメージをビルドします。

```bash
# キャッシュを使用せずにイメージを再ビルドする
docker compose build --no-cache
```

```bash
# ビルドしてからコンテナを起動する（変更を反映させたい場合）
docker compose up -d --build
```

### コンテナの再起動

設定変更後などに、コンテナを再起動します。

```bash
# すべてのサービスを再起動する
docker compose restart
```

```bash
# 特定のサービス（例: app）のみを再起動する
docker compose restart app
```

### 複数のComposeファイルの指定

環境（開発、本番など）によって設定を分けたい場合、複数のファイルを結合して使用できます。

```bash
# 基本設定と本番用設定をマージして起動する
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

## 知ってると便利なコマンド

作業効率を上げる、便利なコマンド群です。

### コンテナのプロセス確認

コンテナ内で実行されているプロセスを確認できます。

```bash
# 実行中のプロセスを確認する
docker compose top
```

### コンテナへのファイルコピー

ホストマシンとコンテナ間でファイルをコピーします。

```bash
# ホストのファイルをコンテナにコピーする
docker compose cp ./local_file.txt web:/app/container_file.txt
```

### 設定ファイルの検証

`docker-compose.yml` の構文が正しいか、どのように解釈されているかを確認します。

```bash
# 設定内容を検証して出力する
docker compose config
```

```bash
# サービス名の一覧だけを出力する
docker compose config --services
```

```bash
# 設定ファイルの編集が必要な場合は、vimなどのエディタで開く
vim docker-compose.yml
```

## 参考リンク

さらに詳しい情報は、公式ドキュメントをご参照ください。

- [Docker Compose コマンドラインリファレンス](https://docs.docker.com/compose/reference/)
  - コマンドの全オプションが確認できます。
  - 最新のアップデート情報も掲載されています。
