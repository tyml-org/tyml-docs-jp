+++
title = "デフォルト値"
weight = 3
+++

## 既定値
メソットの引数や戻り値にはデフォルト値を設定できます
```tyml
type Type {
    id: int
    name: string
}

interface API {
    function test(id: int = 100, name: string = "test") -> Type {
        return { id = 100, name = "test" }
    }
}
```
これは[`TYML for VSCode`](https://marketplace.visualstudio.com/items?itemName=bea4dev.tyml-lsp-vscode)向けの機能です。

拡張機能をインストールしたVSCodeでファイルを開くと以下のように`▶ Run server`や`▶ Run client`が表示されるはずです。

![tyml_vscode_run](https://tyml-org.github.io/tyml-docs-jp/chapter3/tyml_vscode_run.png)

これらをクリックすることでモックサーバーやクライアントとして使用でき、アプリケーションの簡易テストをワンクリックで実行できます。

## 記法
デフォルト値は実際に使用されるjson値に対応しますが、文法はjsonと少し異なります。

### プリミティブ
プリミティブ値はjsonと共通です
```json
// int
100
// float
100.0
// string
"text"
// bool
true
false
// null
null
```

### オブジェクト
オブジェクトはjsonと少し異なります。
名前のリテラルは`"`で囲う必要はなく、`:`は`=`になります。
```tyml
{ id = 100, name = "text" }
```

### 配列
配列はjsonと同じです
```json
[0, 100, null]
```
