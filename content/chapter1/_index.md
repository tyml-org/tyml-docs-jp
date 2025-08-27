+++
title = "はじめに"
weight = 1
sort_by = "weight"
insert_anchor_links = "right"
+++

## [TYML](https://tyml-org.github.io/tyml-lang.org/)とは
TYMLは任意の設定用言語に対して型検査やエディタ支援を行うことを目的としたスキーマ言語です。
現在は以下の設定用言語に対応しています。
- `ini`
- `toml`
- `json`

## 簡単な仕様紹介
TYMLは競合の`JsonSchema`よりも直感的かつ厳密な仕様を両立できるように設計されています。

また、`Open-API`のようにREST-APIのスキーマを定義することもできます。

ここでは簡単に、TYMLがどのような文法で記述されるのかを紹介します。

```tyml
// "setting"は設定値の名前で"int"はその型を表します
setting: int

/// "/"を３つ繋げるとドキュメントとして認識されます
/// (もちろんLSP上でも認識され、設定記述中も表示させることが可能です)
///
/// "[Setting]"は型"Setting"の配列を意味します
settings: [Setting]

/// 構造型"Setting"を定義することができます
type Setting {
    ip: string
    port: int
    mode: Mode
}

/// enumを定義すると、指定された文字列に限定することができます
enum Mode {
    "Debug"
    "Release"
}

/// "?"を型につけることで、この値を省略することが許可されます
/// いわゆるNullable型やOptional型と呼ばれるものです
nullable: int?

/// "|"で型を区切ることで両端の型のうちいずれかがの値であれば受理されるようになります
or_type: int | string


/// REST-APIを定義することもできます！
/// もちろん型生成ツール(tyml-api-generator)を使用してコードを生成できます
/// ここに書いたドキュメントは生成時にも反映されます
interface API {
    function register(id: int = 100, name: string = "test") -> string {
        return "* Secret Token *"
    }

    authed function get_user(@claim: Claim) -> User {
        return { id = 100, name = "test" }
    }

    #[kind = "get"]
    function hello() -> string {
        return "Hello, world!"
    }
}
```

さらに設定用言語で以下のように記述することで型チェックやエディタ支援を有効化できます
```toml
# !tyml example.tyml
# コメントで"!tyml"と記述することで、スキーマファイルを指定することができます
setting = 100

[[settings]]
ip = "192.168.1.1"
port = 25565
mode = "Debug"

or_type = 200
```
