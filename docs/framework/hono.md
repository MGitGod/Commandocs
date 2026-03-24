# Hono

Hono は Cloudflare Workers・Deno・Bun・Node.js など複数のランタイムで動作する、軽量かつ高速な Web フレームワークです。
CLI ツール `create-hono` によるプロジェクト生成から、アプリケーション API まで幅広いコマンドを収録しています。

---

## インストール・アップデート・アンインストール

### プロジェクトを新規作成する

=== "bun"

    ```bash
    bun create hono@latest my-app
    ```

=== "npm"

    ```bash
    npm create hono@latest my-app
    ```

=== "pnpm"

    ```bash
    pnpm create hono@latest my-app
    ```

=== "yarn"

    ```bash
    yarn create hono my-app
    ```

=== "Deno"

    ```bash
    deno run -A npm:create-hono my-app
    ```

!!! note "プロジェクト作成"
    テンプレートの選択肢は `cloudflare-workers` / `nodejs` / `bun` / `deno` / `nextjs` / `lambda` など多数あります。
    対話式プロンプトで選択するか、`--template` オプションで直接指定できます。

#### 使用例

```bash
# テンプレートを直接指定して作成
bun create hono@latest my-api --template cloudflare-workers

# カレントディレクトリに作成
bun create hono@latest .

# npm で最新版を使ってプロジェクトを作成
npm create hono@latest my-project
cd my-project
npm install
```

---

### Hono をパッケージとして追加する

=== "bun"

    ```bash
    bun add hono
    ```

=== "npm"

    ```bash
    npm install hono
    ```

=== "pnpm"

    ```bash
    pnpm add hono
    ```

=== "Deno"

    ```json
    {
      "imports": {
        "hono": "npm:hono"
      }
    }
    ```

!!! note "パッケージ追加"
    既存プロジェクトに Hono を追加する場合に使用します。
    Hono は TypeScript ファースト設計のため、型定義は本体に含まれています。

#### 使用例

```bash
# Hono 本体を追加
bun add hono

# 特定バージョンを指定
bun add hono@4.6.0

# 最新版を明示して追加
bun add hono@latest
```

---

### Hono をアップデートする

=== "bun"

    ```bash
    bun update hono
    ```

=== "npm"

    ```bash
    npm update hono
    ```

=== "pnpm"

    ```bash
    pnpm update hono
    ```

!!! note "アップデート"
    `bun update` は `package.json` の範囲内で最新バージョンに更新します。
    メジャーバージョンアップは `bun add hono@latest` で行います。

#### 使用例

```bash
# Hono を最新版にアップデート
bun update hono

# 最新メジャーバージョンへ強制アップデート
bun add hono@latest

# バージョンを確認
bun pm ls | grep hono
```

---

### Hono をアンインストールする

=== "bun"

    ```bash
    bun remove hono
    ```

=== "npm"

    ```bash
    npm uninstall hono
    ```

=== "pnpm"

    ```bash
    pnpm remove hono
    ```

!!! note "アンインストール"
    `package.json` から依存関係も同時に削除されます。

#### 使用例

```bash
bun remove hono
```

---

## 開発サーバー・実行

### 開発サーバーを起動する

=== "Cloudflare Workers"

    ```bash
    npx wrangler dev
    bunx wrangler dev
    ```

=== "Bun"

    ```bash
    bun run dev
    bun run --hot src/index.ts
    ```

=== "Node.js (tsx)"

    ```bash
    npx tsx watch src/index.ts
    ```

=== "Deno"

    ```bash
    deno run --allow-net --allow-read --watch src/index.ts
    ```

!!! note "開発サーバー"
    Cloudflare Workers テンプレートでは `wrangler dev` が標準です。
    Bun テンプレートでは `--hot` オプションでホットリロードが有効になります。

#### 使用例

```bash
# Cloudflare Workers でローカル開発
npx wrangler dev

# ポートを指定して起動（wrangler）
npx wrangler dev --port 8787

# リモート環境に接続して開発（Cloudflare バインディングを使う場合）
npx wrangler dev --remote

# Bun でホットリロードしながら開発
bun run --hot src/index.ts

# Node.js でウォッチモード起動
npx tsx watch src/index.ts
```

---

## ビルド・デプロイ

### ビルドする

=== "Cloudflare Workers"

    ```bash
    npx wrangler deploy --dry-run
    ```

=== "Bun"

    ```bash
    bun build src/index.ts --outdir dist
    ```

=== "Node.js (esbuild)"

    ```bash
    npx esbuild src/index.ts --bundle --outfile=dist/index.js
    ```

!!! note "ビルド"
    Cloudflare Workers では `--dry-run` フラグでデプロイせずにビルドのみ実行できます。

#### 使用例

```bash
# ドライランでビルド確認（Cloudflare）
npx wrangler deploy --dry-run --outdir dist

# Bun でバンドル＆ミニファイ
bun build src/index.ts --outdir dist --minify

# esbuild で CommonJS 向けにバンドル
npx esbuild src/index.ts --bundle --platform=node --outfile=dist/index.js
```

---

### デプロイする

=== "Cloudflare Workers"

    ```bash
    npx wrangler deploy
    bunx wrangler deploy
    ```

=== "Deno Deploy"

    ```bash
    deployctl deploy --project=my-project src/index.ts
    ```

=== "AWS Lambda"

    ```bash
    # SST を使う場合
    npx sst deploy
    ```

!!! note "デプロイ"
    Cloudflare Workers へのデプロイは `wrangler.toml` の設定が必要です。
    初回は `npx wrangler login` で認証してください。

#### 使用例

```bash
# Cloudflare Workers へデプロイ
npx wrangler deploy

# 環境を指定してデプロイ
npx wrangler deploy --env production

# Deno Deploy へデプロイ
deployctl deploy --project=my-hono-app src/index.ts

# wrangler の認証（初回のみ）
npx wrangler login

# デプロイ済みワーカーの一覧表示
npx wrangler deployments list
```

---

## 基本コマンド（Hono API）

### アプリケーションを初期化する

```ts
import { Hono } from 'hono'

const app = new Hono()
```

!!! note "アプリ初期化"
    `new Hono()` でアプリケーションインスタンスを作成します。
    ベースパスを指定する場合は `.basePath('/api')` を使います。

#### 使用例

```ts
import { Hono } from 'hono'

// 基本的な初期化
const app = new Hono()

// ベースパスを設定
const api = new Hono().basePath('/api')

// 型付き Bindings / Variables を定義（Cloudflare Workers 向け）
type Bindings = {
  DB: D1Database
  KV: KVNamespace
}

type Variables = {
  user: { id: string; name: string }
}

const app = new Hono<{ Bindings: Bindings; Variables: Variables }>()

export default app
```

---

### GET ルートを定義する

```ts
app.get('/path', (c) => {
  return c.json({ message: 'Hello' })
})
```

!!! note "GET ルート"
    `c` はコンテキストオブジェクトです。リクエスト・レスポンス操作の起点になります。
    第2引数のコールバックはハンドラー関数と呼びます。

#### 使用例

```ts
// シンプルなテキストレスポンス
app.get('/', (c) => c.text('Hello Hono!'))

// JSON レスポンス
app.get('/users', (c) => {
  return c.json([{ id: 1, name: 'Alice' }])
})

// パスパラメータを使う
app.get('/users/:id', (c) => {
  const id = c.req.param('id')
  return c.json({ id })
})

// ステータスコードを指定
app.get('/not-found', (c) => {
  return c.json({ error: 'Not Found' }, 404)
})
```

---

### POST ルートを定義する

```ts
app.post('/path', async (c) => {
  const body = await c.req.json()
  return c.json(body, 201)
})
```

!!! note "POST ルート"
    リクエストボディは `await c.req.json()` で取得します（非同期処理）。
    ハンドラーを `async` 関数にする必要があります。

#### 使用例

```ts
// JSON ボディを受け取る
app.post('/users', async (c) => {
  const { name, email } = await c.req.json()
  return c.json({ id: 2, name, email }, 201)
})

// フォームデータを受け取る
app.post('/form', async (c) => {
  const body = await c.req.parseBody()
  return c.json({ data: body })
})
```

---

### PUT / PATCH / DELETE ルートを定義する

```ts
app.put('/path/:id', async (c) => { /* 全更新 */ })
app.patch('/path/:id', async (c) => { /* 部分更新 */ })
app.delete('/path/:id', (c) => { /* 削除 */ })
app.all('/path', (c) => { /* 全メソッドをキャッチ */ })
```

!!! note "その他のメソッド"
    REST API の設計に合わせてメソッドを使い分けます。
    `app.all()` を使うと全メソッドをキャッチできます。

#### 使用例

```ts
// 全更新（PUT）
app.put('/users/:id', async (c) => {
  const id = c.req.param('id')
  const body = await c.req.json()
  return c.json({ id, ...body })
})

// 部分更新（PATCH）
app.patch('/users/:id', async (c) => {
  const id = c.req.param('id')
  const body = await c.req.json()
  return c.json({ id, ...body })
})

// 削除（DELETE）
app.delete('/users/:id', (c) => {
  const id = c.req.param('id')
  return c.json({ deleted: id })
})

// 全メソッドをキャッチ
app.all('/catch-all', (c) => c.text(`Method: ${c.req.method}`))
```

---

## リクエスト操作

### パスパラメータを取得する

```ts
const id = c.req.param('id')
```

!!! note "パスパラメータ"
    `:id` のように `:` で始まる部分がパスパラメータです。
    `c.req.param()` で全パラメータを一度にオブジェクトとして取得することもできます。

#### 使用例

```ts
app.get('/posts/:postId/comments/:commentId', (c) => {
  // 個別に取得
  const postId = c.req.param('postId')
  const commentId = c.req.param('commentId')

  // 全パラメータを一度に取得
  const params = c.req.param()
  // => { postId: '...', commentId: '...' }

  return c.json({ postId, commentId })
})
```

---

### クエリパラメータを取得する

```ts
const q = c.req.query('q')
```

!!! note "クエリパラメータ"
    URL の `?key=value` 部分を取得します。
    同一キーの複数値（`?tag=a&tag=b`）は `c.req.queries('tag')` で配列として取得できます。

#### 使用例

```ts
app.get('/search', (c) => {
  // 単一値を取得
  const q = c.req.query('q')        // ?q=hono
  const page = c.req.query('page')  // ?page=2

  // 複数値を配列で取得（?tag=a&tag=b）
  const tags = c.req.queries('tag') // => ['a', 'b']

  // 全クエリをオブジェクトで取得
  const all = c.req.query()         // => { q: 'hono', page: '2' }

  return c.json({ q, page, tags })
})
```

---

### リクエストボディを取得する

```ts
const data = await c.req.json()
```

!!! note "リクエストボディ"
    `c.req.json()` は JSON、`c.req.text()` はテキスト、
    `c.req.parseBody()` はフォームデータ、`c.req.arrayBuffer()` はバイナリを取得します。

#### 使用例

```ts
app.post('/data', async (c) => {
  // JSON ボディ
  const json = await c.req.json()

  // テキストボディ
  const text = await c.req.text()

  // フォームデータ（application/x-www-form-urlencoded / multipart）
  const form = await c.req.parseBody()

  // ArrayBuffer（バイナリデータ）
  const buffer = await c.req.arrayBuffer()

  // Blob
  const blob = await c.req.blob()

  return c.json({ received: true })
})
```

---

### ヘッダーを取得する

```ts
const auth = c.req.header('Authorization')
```

!!! note "リクエストヘッダー"
    ヘッダー名は大文字・小文字を区別しません。
    `c.req.header()` で全ヘッダーをオブジェクトとして取得できます。

#### 使用例

```ts
app.get('/me', (c) => {
  // 特定ヘッダーを取得
  const auth = c.req.header('Authorization')
  const contentType = c.req.header('Content-Type')

  // 全ヘッダーを取得
  const headers = c.req.header()

  return c.json({ auth, contentType })
})
```

---

### URL・メソッド・パスを取得する

```ts
const url = c.req.url
const path = c.req.path
const method = c.req.method
```

!!! note "URL 情報"
    `c.req.url` は完全な URL 文字列、`c.req.path` はパス部分のみを返します。

#### 使用例

```ts
app.use('*', async (c, next) => {
  console.log(c.req.method)   // 'GET'
  console.log(c.req.url)      // 'http://localhost:8787/users?page=1'
  console.log(c.req.path)     // '/users'
  await next()
})
```

---

## レスポンス操作

### JSON レスポンスを返す

```ts
return c.json(data, statusCode)
```

!!! note "JSON レスポンス"
    第2引数にステータスコードを指定できます（省略時は 200）。
    第3引数に追加のレスポンスヘッダーを渡せます。

#### 使用例

```ts
// 200 OK
return c.json({ message: 'OK' })

// 201 Created
return c.json({ id: 1 }, 201)

// 404 Not Found
return c.json({ error: 'Not Found' }, 404)

// ヘッダーも同時に指定
return c.json({ data: 'ok' }, 200, { 'X-Custom': 'value' })
```

---

### テキスト・HTML レスポンスを返す

```ts
return c.text('Hello')
return c.html('<h1>Hello</h1>')
```

!!! note "テキスト・HTML レスポンス"
    `c.text()` は `Content-Type: text/plain`、
    `c.html()` は `Content-Type: text/html` を自動でセットします。

#### 使用例

```ts
// プレーンテキスト
app.get('/ping', (c) => c.text('pong'))

// HTML レスポンス
app.get('/page', (c) => c.html('<h1>Hello Hono!</h1>'))

// ステータスコードを指定
app.get('/error', (c) => c.text('Server Error', 500))
```

---

### リダイレクトする

```ts
return c.redirect('/new-path')
```

!!! note "リダイレクト"
    デフォルトは 302 一時リダイレクトです。
    恒久リダイレクトは第2引数に `301` を指定します。

#### 使用例

```ts
// 302 一時リダイレクト（デフォルト）
app.get('/old', (c) => c.redirect('/new'))

// 301 恒久リダイレクト
app.get('/old-page', (c) => c.redirect('https://example.com', 301))
```

---

### レスポンスヘッダーを設定する

```ts
c.header('X-Custom-Header', 'value')
```

!!! note "レスポンスヘッダー"
    `c.header()` はレスポンスを返す前に呼び出します。
    複数のヘッダーを追加する場合は繰り返し呼び出せます。

#### 使用例

```ts
app.get('/api', (c) => {
  c.header('X-Response-Time', '10ms')
  c.header('Cache-Control', 'no-cache')
  return c.json({ data: 'ok' })
})
```

---

## ミドルウェア

### ミドルウェアを登録する

```ts
app.use('/path/*', async (c, next) => {
  // 前処理（リクエスト）
  await next()
  // 後処理（レスポンス）
})
```

!!! note "ミドルウェア"
    `await next()` の前がリクエスト処理、後がレスポンス処理です。
    全ルートに適用する場合はパスを `'*'` にします。

#### 使用例

```ts
// 全ルートにロガーを適用
app.use('*', async (c, next) => {
  console.log(`[${c.req.method}] ${c.req.path}`)
  await next()
  console.log(`=> ${c.res.status}`)
})

// 特定パスにのみ認証を適用
app.use('/admin/*', async (c, next) => {
  const token = c.req.header('Authorization')
  if (!token) return c.json({ error: 'Unauthorized' }, 401)
  await next()
})
```

---

### ビルトインミドルウェアを使う

```ts
import { logger } from 'hono/logger'
import { cors } from 'hono/cors'
import { bearerAuth } from 'hono/bearer-auth'
```

!!! note "ビルトインミドルウェア"
    Hono には多数のミドルウェアが含まれています。
    追加インストール不要で `hono/xxx` からインポートできます。

#### 使用例

```ts
import { Hono } from 'hono'
import { logger } from 'hono/logger'
import { cors } from 'hono/cors'
import { prettyJSON } from 'hono/pretty-json'
import { compress } from 'hono/compress'
import { etag } from 'hono/etag'
import { bearerAuth } from 'hono/bearer-auth'
import { basicAuth } from 'hono/basic-auth'
import { jwt } from 'hono/jwt'
import { cache } from 'hono/cache'
import { timeout } from 'hono/timeout'
import { requestId } from 'hono/request-id'
import { secureHeaders } from 'hono/secure-headers'
import { csrf } from 'hono/csrf'
import { bodyLimit } from 'hono/body-limit'

const app = new Hono()

// リクエストログを出力
app.use('*', logger())

// CORS を設定
app.use('/api/*', cors({
  origin: 'https://example.com',
  allowMethods: ['GET', 'POST', 'PUT', 'DELETE'],
}))

// JSON レスポンスを見やすく整形
app.use('/api/*', prettyJSON())

// gzip 圧縮
app.use('*', compress())

// ETag でキャッシュ制御
app.use('*', etag())

// Bearer トークン認証
app.use('/admin/*', bearerAuth({ token: 'my-secret-token' }))

// Basic 認証
app.use('/basic/*', basicAuth({ username: 'user', password: 'pass' }))

// JWT 認証
app.use('/protected/*', jwt({ secret: 'my-jwt-secret' }))

// セキュリティヘッダーを自動付与
app.use('*', secureHeaders())

// リクエスト ID を付与
app.use('*', requestId())

// CSRF 対策
app.use('/form/*', csrf())

// リクエストボディサイズ制限（100KB）
app.use('*', bodyLimit({ maxSize: 100 * 1024 }))

// タイムアウト設定（5秒）
app.use('/api/*', timeout(5000))
```

---

## ルーティング

### ルートをグループ化する

```ts
const api = new Hono()
api.get('/users', ...)
app.route('/api', api)
```

!!! note "ルートグループ"
    `app.route()` でサブルーターをマウントします。
    ファイルを分割して大規模アプリケーションを管理しやすくなります。

#### 使用例

```ts
import { Hono } from 'hono'

const app = new Hono()

// ユーザー関連ルート
const users = new Hono()
users.get('/', (c) => c.json([]))
users.post('/', async (c) => c.json({ created: true }, 201))
users.get('/:id', (c) => c.json({ id: c.req.param('id') }))
users.put('/:id', async (c) => c.json({ updated: true }))
users.delete('/:id', (c) => c.json({ deleted: true }))

// 記事関連ルート
const posts = new Hono()
posts.get('/', (c) => c.json([]))
posts.get('/:id', (c) => c.json({ id: c.req.param('id') }))

// ルートにマウント
// => /users, /users/:id など
app.route('/users', users)
// => /posts, /posts/:id など
app.route('/posts', posts)

export default app
```

---

### ワイルドカードルートを定義する

```ts
app.get('/files/*', (c) => { /* ... */ })
```

!!! note "ワイルドカード"
    `*` は任意のパスセグメントにマッチします。
    `c.req.param('*')` でマッチした部分を取得できます。

#### 使用例

```ts
app.get('/files/*', (c) => {
  const path = c.req.param('*')  // 例: 'images/photo.jpg'
  return c.text(`File: ${path}`)
})

// 正規表現ルート（オプション）
app.get('/posts/:id{[0-9]+}', (c) => {
  const id = c.req.param('id')  // 数字のみマッチ
  return c.json({ id: Number(id) })
})
```

---

### エラーハンドラーを設定する

```ts
app.onError((err, c) => {
  return c.json({ error: err.message }, 500)
})

app.notFound((c) => {
  return c.json({ error: 'Not Found' }, 404)
})
```

!!! note "エラーハンドラー"
    `app.onError()` でグローバルエラーハンドラーを登録します。
    `app.notFound()` でどのルートにもマッチしなかった場合の処理を定義できます。

#### 使用例

```ts
// グローバルエラーハンドラー
app.onError((err, c) => {
  console.error(err)
  return c.json({
    error: err.message,
    // 開発環境のみスタックトレースを返す
    stack: process.env.NODE_ENV === 'development' ? err.stack : undefined,
  }, 500)
})

// 404 ハンドラー
app.notFound((c) => {
  return c.json({
    error: `Route ${c.req.method} ${c.req.path} not found`,
  }, 404)
})
```

---

## コンテキスト操作

### 変数をコンテキストに保存する

```ts
c.set('key', value)
const value = c.get('key')
```

!!! note "コンテキスト変数"
    ミドルウェアからハンドラーへ値を受け渡すのに使います。
    TypeScript で型安全にするには `Variables` 型を定義します。

#### 使用例

```ts
type Variables = {
  user: { id: string; name: string }
}

const app = new Hono<{ Variables: Variables }>()

// ミドルウェアで値をセット
app.use('*', async (c, next) => {
  // 認証チェック後にユーザー情報をセット
  c.set('user', { id: '1', name: 'Alice' })
  await next()
})

// ハンドラーで値を取得
app.get('/profile', (c) => {
  const user = c.get('user')  // 型: { id: string; name: string }
  return c.json(user)
})
```

---

### 環境変数・バインディングを取得する（Cloudflare）

```ts
const { DB, KV, MY_VAR } = c.env
```

!!! note "環境バインディング"
    Cloudflare Workers の `c.env` から Bindings にアクセスします。
    `wrangler.toml` で定義したリソース（D1・KV・R2 など）を参照できます。

#### 使用例

```ts
type Bindings = {
  DB: D1Database
  KV: KVNamespace
  R2: R2Bucket
  MY_SECRET: string
}

const app = new Hono<{ Bindings: Bindings }>()

app.get('/data', async (c) => {
  // D1 データベースにクエリ
  const result = await c.env.DB.prepare('SELECT * FROM users').all()

  // KV ストレージから取得
  const value = await c.env.KV.get('my-key')

  // R2 バケットからオブジェクト取得
  const obj = await c.env.R2.get('file.txt')

  // 環境変数を参照
  const secret = c.env.MY_SECRET

  return c.json({ result, value, secret })
})
```

---

## バリデーション

### Zod Validator を使う

```ts
import { zValidator } from '@hono/zod-validator'
import { z } from 'zod'
```

!!! note "Zod バリデーション"
    `@hono/zod-validator` は別途インストールが必要です。
    スキーマに合わない場合は自動で 400 エラーを返します。

#### 使用例

```bash
# パッケージをインストール
bun add @hono/zod-validator zod
```

```ts
import { zValidator } from '@hono/zod-validator'
import { z } from 'zod'

// JSON ボディのバリデーション
const userSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
  age: z.number().min(0).optional(),
})

app.post('/users',
  zValidator('json', userSchema),
  (c) => {
    // バリデーション済みの値を取得
    const { name, email, age } = c.req.valid('json')
    return c.json({ name, email, age }, 201)
  }
)

// クエリパラメータのバリデーション
const searchSchema = z.object({
  q: z.string().min(1),
  page: z.coerce.number().default(1),
  limit: z.coerce.number().max(100).default(20),
})

app.get('/search',
  zValidator('query', searchSchema),
  (c) => {
    const { q, page, limit } = c.req.valid('query')
    return c.json({ q, page, limit })
  }
)

// パスパラメータのバリデーション
const paramSchema = z.object({
  id: z.string().uuid(),
})

app.get('/users/:id',
  zValidator('param', paramSchema),
  (c) => {
    const { id } = c.req.valid('param')
    return c.json({ id })
  }
)
```

---

## 上級者向けコマンド

### Hono RPC（型安全なクライアント）を使う

```ts
import { hc } from 'hono/client'
```

!!! note "Hono RPC"
    サーバーの型定義をクライアントと共有し、完全な型安全を実現します。
    `AppType` をエクスポートしてクライアントで `hc<AppType>()` に渡します。

#### 使用例

```ts
// server.ts
import { Hono } from 'hono'

const app = new Hono()
  .get('/users', (c) => c.json([{ id: 1, name: 'Alice' }]))
  .post('/users', async (c) => {
    const body = await c.req.json<{ name: string }>()
    return c.json({ id: 2, ...body }, 201)
  })
  .get('/users/:id', (c) => c.json({ id: c.req.param('id') }))

// AppType をエクスポート（クライアント側で使用）
export type AppType = typeof app
export default app
```

```ts
// client.ts（フロントエンドや別サービス）
import { hc } from 'hono/client'
import type { AppType } from './server'

const client = hc<AppType>('http://localhost:8787')

// 型補完と型チェックが効く
const res = await client.users.$get()
const users = await res.json()
// 型: { id: number; name: string }[]

// POST も型安全
const created = await client.users.$post({
  json: { name: 'Bob' },
})
const user = await created.json()

// パスパラメータ付き
const single = await client.users[':id'].$get({ param: { id: '1' } })
```

---

### ストリーミングレスポンスを返す

```ts
import { streamText, stream, streamSSE } from 'hono/streaming'
```

!!! note "ストリーミング"
    大きなデータや AI レスポンスの逐次配信に使います。
    `streamText` はテキスト、`stream` はバイナリ、`streamSSE` は SSE 向けです。

#### 使用例

```ts
import { Hono } from 'hono'
import { streamText, stream, streamSSE } from 'hono/streaming'

const app = new Hono()

// テキストストリーム
app.get('/stream', (c) => {
  return streamText(c, async (stream) => {
    for (let i = 0; i < 5; i++) {
      await stream.writeln(`Line ${i + 1}`)
      await stream.sleep(500)  // 0.5秒ごとに送信
    }
  })
})

// バイナリストリーム
app.get('/binary', (c) => {
  return stream(c, async (stream) => {
    const data = new Uint8Array([1, 2, 3, 4, 5])
    await stream.write(data)
  })
})

// SSE（Server-Sent Events）でリアルタイム配信
app.get('/sse', (c) => {
  return streamSSE(c, async (stream) => {
    let id = 0
    while (true) {
      await stream.writeSSE({
        data: JSON.stringify({ count: id++ }),
        event: 'update',
        id: String(id),
      })
      await stream.sleep(1000)  // 1秒ごとにイベント送信
    }
  })
})
```

---

### JSX でレンダリングする

```ts
import { jsxRenderer } from 'hono/jsx-renderer'
```

!!! note "JSX サポート"
    Hono は JSX を標準サポートしています。
    `tsconfig.json` に `"jsx": "react-jsx"` と `"jsxImportSource": "hono/jsx"` を設定します。

#### 使用例

```json
// tsconfig.json に追加
{
  "compilerOptions": {
    "jsx": "react-jsx",
    "jsxImportSource": "hono/jsx"
  }
}
```

```tsx
// src/index.tsx
import { Hono } from 'hono'
import { jsxRenderer } from 'hono/jsx-renderer'

const app = new Hono()

// 共通レイアウトを定義
app.use(
  '*',
  jsxRenderer(({ children, title }) => (
    <html lang="ja">
      <head>
        <meta charset="UTF-8" />
        <title>{title ?? 'My App'}</title>
      </head>
      <body>{children}</body>
    </html>
  ))
)

// c.render() でレイアウトを適用してレンダリング
app.get('/', (c) => {
  return c.render(<h1>Hello JSX!</h1>, { title: 'Top Page' })
})

app.get('/users/:id', (c) => {
  const id = c.req.param('id')
  return c.render(<div>User: {id}</div>)
})
```

---

### ファクトリ関数でカスタムミドルウェアを作る

```ts
import { createMiddleware } from 'hono/factory'
```

!!! note "カスタムミドルウェア"
    `createMiddleware` を使うと型推論が正しく効いたミドルウェアを作れます。
    `Variables` の型も正確に伝播します。

#### 使用例

```ts
import { createMiddleware } from 'hono/factory'

// 型安全なカスタム認証ミドルウェア
const authMiddleware = createMiddleware<{
  Variables: { userId: string; role: string }
}>(async (c, next) => {
  const token = c.req.header('Authorization')?.replace('Bearer ', '')
  if (!token) return c.json({ error: 'Unauthorized' }, 401)

  // トークンを検証してコンテキストにセット
  c.set('userId', 'user-123')
  c.set('role', 'admin')
  await next()
})

app.use('/protected/*', authMiddleware)

app.get('/protected/profile', (c) => {
  const userId = c.get('userId')  // 型: string
  const role = c.get('role')      // 型: string
  return c.json({ userId, role })
})
```

---

### テストを書く

```ts
import { testClient } from 'hono/testing'
```

!!! note "ユニットテスト"
    `app.request()` または `testClient` で HTTP リクエストを直接発行してテストできます。
    Bun の組み込みテストランナーや Vitest と組み合わせて使います。

#### 使用例

```ts
// app.test.ts
import { describe, it, expect } from 'bun:test'
import app from './app'

// GET テスト
describe('GET /', () => {
  it('200 を返す', async () => {
    const res = await app.request('/')
    expect(res.status).toBe(200)
    expect(await res.text()).toBe('Hello Hono!')
  })
})

// パスパラメータのテスト
describe('GET /users/:id', () => {
  it('ユーザー情報を JSON で返す', async () => {
    const res = await app.request('/users/42')
    expect(res.status).toBe(200)
    const json = await res.json()
    expect(json.id).toBe('42')
  })
})

// POST テスト
describe('POST /users', () => {
  it('新しいユーザーを作成する', async () => {
    const res = await app.request('/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name: 'Alice', email: 'alice@example.com' }),
    })
    expect(res.status).toBe(201)
    const body = await res.json()
    expect(body.name).toBe('Alice')
  })
})

// 認証ミドルウェアのテスト
describe('GET /admin', () => {
  it('トークンなしで 401 を返す', async () => {
    const res = await app.request('/admin')
    expect(res.status).toBe(401)
  })

  it('正しいトークンで 200 を返す', async () => {
    const res = await app.request('/admin', {
      headers: { Authorization: 'Bearer valid-token' },
    })
    expect(res.status).toBe(200)
  })
})
```

---

## 知ってると便利なコマンド

### Cookie を操作する

```ts
import { getCookie, setCookie, deleteCookie } from 'hono/cookie'
```

!!! note "Cookie"
    `getCookie` / `setCookie` / `deleteCookie` で Cookie の読み書きを行います。
    `getSignedCookie` / `setSignedCookie` で署名付き Cookie も扱えます。

#### 使用例

```ts
import { getCookie, setCookie, deleteCookie, getSignedCookie, setSignedCookie } from 'hono/cookie'

app.get('/cookie', (c) => {
  // 取得
  const session = getCookie(c, 'session')

  // セット（オプション付き）
  setCookie(c, 'session', 'abc123', {
    maxAge: 3600,       // 1時間
    httpOnly: true,     // JavaScript からアクセス不可
    secure: true,       // HTTPS のみ
    sameSite: 'Strict', // クロスサイト送信を禁止
    path: '/',
  })

  // 削除
  deleteCookie(c, 'session')

  return c.json({ session })
})

// 署名付き Cookie（改ざん検知）
app.get('/signed', async (c) => {
  const secret = 'my-secret'
  const value = await getSignedCookie(c, secret, 'user_id')
  await setSignedCookie(c, 'user_id', '42', secret, { httpOnly: true })
  return c.json({ value })
})
```

---

### HTML をエスケープする

```ts
import { html, raw } from 'hono/html'
```

!!! note "HTML エスケープ"
    `html` タグ関数でテンプレートリテラルを安全にエスケープします。
    `raw()` は信頼できる値のみに使用してください（XSS に注意）。

#### 使用例

```ts
import { html, raw } from 'hono/html'

app.get('/safe', (c) => {
  const userInput = '<script>alert("xss")</script>'
  // html タグ関数で自動エスケープ
  return c.html(html`<p>${userInput}</p>`)
  // => <p>&lt;script&gt;alert("xss")&lt;/script&gt;</p>
})

app.get('/raw', (c) => {
  const trustedHtml = '<strong>強調テキスト</strong>'
  // raw() でエスケープを回避（信頼できる値のみ）
  return c.html(html`<p>${raw(trustedHtml)}</p>`)
})
```

---

### ルート一覧を確認する

```ts
console.log(app.routes)
```

!!! note "ルート確認"
    登録済みの全ルートをオブジェクト配列として確認できます。
    デバッグやドキュメント生成に便利です。

#### 使用例

```ts
const app = new Hono()
app.get('/users', (c) => c.json([]))
app.post('/users', async (c) => c.json({}, 201))
app.get('/posts', (c) => c.json([]))

// 登録済みルートを出力
console.log(app.routes)
// [
//   { method: 'GET',  path: '/users', handler: [Function] },
//   { method: 'POST', path: '/users', handler: [Function] },
//   { method: 'GET',  path: '/posts', handler: [Function] },
// ]
```

---

### サードパーティミドルウェアをインストールする

```bash
bun add @hono/<パッケージ名>
```

!!! note "サードパーティミドルウェア"
    Hono の公式サードパーティパッケージは `@hono/` スコープで提供されています。

#### 使用例

```bash
# Zod バリデーション
bun add @hono/zod-validator zod

# Swagger UI（API ドキュメント）
bun add @hono/swagger-ui

# Sentry エラートラッキング
bun add @hono/sentry

# Firebase 認証
bun add @hono/firebase-auth

# Clerk 認証
bun add @hono/clerk-auth

# GraphQL サーバー
bun add @hono/graphql-server

# Node.js アダプタ
bun add @hono/node-server

# Cloudflare Pages アダプタ
bun add @hono/cloudflare-pages

# Remix 統合
bun add @hono/remix-adapter

# Vite Plugin（SSR）
bun add @hono/vite-dev-server
```

```ts
import { swaggerUI } from '@hono/swagger-ui'
import { sentry } from '@hono/sentry'

// Swagger UI を /ui で公開
app.get('/ui', swaggerUI({ url: '/doc' }))

// Sentry によるエラー監視
app.use('*', sentry({ dsn: 'YOUR_SENTRY_DSN' }))
```

---

## オプション

### `new Hono()` のオプション

| オプション | 型 | デフォルト | 説明 |
| --- | --- | --- | --- |
| `strict` | `boolean` | `true` | パスの末尾スラッシュを厳密チェックする |
| `router` | `Router` | `SmartRouter` | 使用するルーターの実装を指定 |
| `getPath` | `function` | — | リクエストからパスを取得するカスタム関数 |

#### 使用例

```ts
import { Hono } from 'hono'
import { RegExpRouter } from 'hono/router/reg-exp-router'

// 末尾スラッシュを区別しない（/users と /users/ を同一視）
const app = new Hono({ strict: false })

// 高速な正規表現ルーターを使用
const app = new Hono({ router: new RegExpRouter() })

// ホスト名ベースのルーティング
const app = new Hono({
  getPath: (req) =>
    req.headers.get('host') + new URL(req.url).pathname,
})
```

---

### `cors()` のオプション

| オプション | 型 | デフォルト | 説明 |
| --- | --- | --- | --- |
| `origin` | `string \| string[] \| function` | `'*'` | 許可するオリジン |
| `allowMethods` | `string[]` | 主要メソッド全て | 許可する HTTP メソッド |
| `allowHeaders` | `string[]` | `[]` | 許可するリクエストヘッダー |
| `exposeHeaders` | `string[]` | `[]` | 公開するレスポンスヘッダー |
| `maxAge` | `number` | — | プリフライトキャッシュ秒数 |
| `credentials` | `boolean` | `false` | Cookie 送信（`credentials: 'include'`）を許可 |

---

### `setCookie()` のオプション

| オプション | 型 | 説明 |
| --- | --- | --- |
| `maxAge` | `number` | 有効期限（秒） |
| `expires` | `Date` | 有効期限（Date オブジェクト） |
| `path` | `string` | Cookie のパス（デフォルト: `'/'`） |
| `domain` | `string` | Cookie のドメイン |
| `secure` | `boolean` | HTTPS 接続のみ送信 |
| `httpOnly` | `boolean` | JavaScript からのアクセスを禁止 |
| `sameSite` | `'Strict' \| 'Lax' \| 'None'` | クロスサイト送信の制御 |
| `partitioned` | `boolean` | パーティション分割 Cookie（CHIPS） |

---

### `jwt()` のオプション

| オプション | 型 | 説明 |
| --- | --- | --- |
| `secret` | `string` | JWT 署名・検証に使う秘密鍵 |
| `alg` | `string` | 署名アルゴリズム（デフォルト: `'HS256'`） |
| `cookie` | `string` | Cookie からトークンを取得する場合のキー名 |

---

### `cache()` のオプション

| オプション | 型 | 説明 |
| --- | --- | --- |
| `cacheName` | `string` | Cache Storage のキャッシュ名 |
| `wait` | `boolean` | キャッシュ書き込みを待機するか（デフォルト: `false`） |
| `cacheControl` | `string` | `Cache-Control` ヘッダーの値 |
| `vary` | `string[]` | `Vary` ヘッダーに追加するヘッダー名 |
| `keyGenerator` | `function` | キャッシュキーを生成するカスタム関数 |

---

## 設定

### `wrangler.toml`（Cloudflare Workers）

```toml
# ワーカー名
name = "my-hono-app"

# エントリーポイント
main = "src/index.ts"

# 互換日
compatibility_date = "2024-01-01"

# 互換フラグ
compatibility_flags = ["nodejs_compat"]

# 環境変数（平文）
[vars]
MY_VAR = "hello"
APP_ENV = "production"

# KV Namespace
[[kv_namespaces]]
binding = "KV"
id = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# D1 データベース
[[d1_databases]]
binding = "DB"
database_name = "my-db"
database_id = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# R2 バケット
[[r2_buckets]]
binding = "R2"
bucket_name = "my-bucket"

# シークレット（wrangler secret put で設定）
# SECRET_KEY = ...

# ルート設定
[routes]
pattern = "example.com/api/*"
zone_name = "example.com"

# 開発環境の設定
[env.development]
name = "my-hono-app-dev"

[env.development.vars]
MY_VAR = "hello-dev"
APP_ENV = "development"
```

---

### `tsconfig.json`（TypeScript 設定）

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ES2022",
    "moduleResolution": "bundler",
    "strict": true,
    "skipLibCheck": true,
    "types": ["@cloudflare/workers-types"],
    "jsx": "react-jsx",
    "jsxImportSource": "hono/jsx"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

### Node.js アダプタの設定

```ts
// src/index.ts
import { serve } from '@hono/node-server'
import { Hono } from 'hono'

const app = new Hono()

app.get('/', (c) => c.text('Hello Node.js + Hono!'))

// ポート 3000 でサーバー起動
serve({
  fetch: app.fetch,
  port: 3000,
}, (info) => {
  console.log(`サーバー起動: http://localhost:${info.port}`)
})

export default app
```

!!! note "Node.js アダプタ"
    `@hono/node-server` は別途インストールが必要です。

```bash
bun add @hono/node-server
```

---

### Cloudflare Pages の設定

```ts
// functions/[[path]].ts
import { handle } from '@hono/cloudflare-pages'
import { Hono } from 'hono'

const app = new Hono()
app.get('/api/hello', (c) => c.json({ message: 'Hello from Pages!' }))

export const onRequest = handle(app)
```

---

## 頻出コマンド早引き

```bash
# プロジェクトを作成（bun）
bun create hono@latest my-app

# プロジェクトを作成（npm）
npm create hono@latest my-app

# Hono をインストール
bun add hono

# Hono を最新版にアップデート
bun add hono@latest

# 開発サーバー起動（Cloudflare Workers）
npx wrangler dev

# 開発サーバー起動（Bun ホットリロード）
bun run --hot src/index.ts

# デプロイ（Cloudflare Workers）
npx wrangler deploy

# テスト実行（Bun）
bun test

# Zod バリデーターを追加
bun add @hono/zod-validator zod

# Node.js アダプタを追加
bun add @hono/node-server

# Swagger UI を追加
bun add @hono/swagger-ui

# wrangler ログイン（初回）
npx wrangler login

# wrangler のバージョン確認
npx wrangler --version
```

```ts
// よく使うパターン早引き
import { Hono } from 'hono'
import { logger } from 'hono/logger'
import { cors } from 'hono/cors'
import { jwt } from 'hono/jwt'

const app = new Hono()

// ミドルウェア
app.use('*', logger())
app.use('/api/*', cors({ origin: 'https://example.com' }))
app.use('/protected/*', jwt({ secret: 'my-secret' }))

// ルート定義
app.get('/', (c) => c.text('Hello Hono!'))
app.get('/json', (c) => c.json({ ok: true }))
app.get('/users/:id', (c) => c.json({ id: c.req.param('id') }))
app.get('/search', (c) => c.json({ q: c.req.query('q') }))
app.post('/users', async (c) => c.json(await c.req.json(), 201))
app.put('/users/:id', async (c) => c.json({ ...await c.req.json() }))
app.delete('/users/:id', (c) => c.json({ deleted: c.req.param('id') }))

// エラーハンドラー
app.notFound((c) => c.json({ error: 'Not Found' }, 404))
app.onError((e, c) => c.json({ error: e.message }, 500))

export default app
```

---

!!! info "公式ドキュメント"
    - [Hono 公式サイト](https://hono.dev)
    - [Hono ドキュメント（英語）](https://hono.dev/docs)
    - [Hono GitHub](https://github.com/honojs/hono)
    - [サードパーティミドルウェア一覧](https://hono.dev/docs/middleware/third-party)
    - [create-hono（プロジェクト生成）](https://github.com/honojs/create-hono)
    - [Wrangler ドキュメント](https://developers.cloudflare.com/workers/wrangler/)
    - [Cloudflare Workers ドキュメント](https://developers.cloudflare.com/workers/)
