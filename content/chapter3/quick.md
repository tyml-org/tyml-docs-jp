+++
title = "クイックスタート"
weight = 1
+++

## 1. VSCode拡張機能をダウンロード
VSCodeマーケットプレイスから[`TYML for VSCode`](https://marketplace.visualstudio.com/items?itemName=bea4dev.tyml-lsp-vscode)をダウンロードします。

![tyml_vscode](https://tyml-org.github.io/tyml-docs-jp/chapter3/tyml_vscode.png)

## 2. APIを定義する
適当な場所にファイル`api.tyml`を作成してください。
```tyml
interface API {
    function hello() -> string {
        return "Hello, world!"
    }
}
```

## 3. TYMLのCLIツールをインストール
以下のコマンドでインストールできます(Rustのcargoが必要です)。
```bash
cargo install tyml_core tyml_api_generator
```

## 4. コードの生成と実装
今回の例では、サーバー側がRust、クライアント側がTypeScriptを想定します。

### サーバー側(Rust)
まずテスト用のクレートを作成します。
```bash
cargo new api-example-server
```
次に2.で定義した`api.tyml`を使って型を生成します。
```bash
tyml-api-gen server rust-axum api.tyml ./api-example-server/api
```
`Success!`と表示されれば成功です。

`Cargo.toml`に先程作成した`api`と`async-trait`と`tokio`を追加します
```toml
[dependencies]
api = { path = "./api/" }
async-trait = "0.1"
tokio = { version = "1", features = ["full"] }
```
最後に`main.rs`を編集してAPIを実装します。
```rs
use api::{serve, types::API};
use async_trait::async_trait;

struct Server {}

#[async_trait]
impl API for Server {
    async fn hello(&self) -> String {
        "Hello, world!".to_string()
    }
}

#[tokio::main]
async fn main() {
    let server = Server {};

    serve(server, "localhost:3000").await.unwrap();
}
```

### クライアント側
TypeScriptの型を作成します。
```bash
tyml-api-gen client typescript api.tyml ./api-example-client/api
```
次に`main.ts`を`./api-example-client`内に作成してAPIを使用するコードを実装します。
```ts
import { API } from "./api/types.ts";

async function main() {
    const api = new API("http://localhost:3000")

    /// メソッドのように呼び出せる
    const result = await api.hello()

    console.log(result)
}

main()
```

## 5. 実行
サーバーとクライアントの *それぞれのディレクトリ* で以下のコマンドを実行します。

### サーバー側
```bash
cargo run
```

### クライアント側
```bash
npx tsx main.ts
```

### 実行結果
クライアント側を実行したときに`Hello, world!`が表示されれば成功です
