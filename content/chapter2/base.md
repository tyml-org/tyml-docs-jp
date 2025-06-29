+++
title = "基本形"
weight = 1
+++

## 設定名と型
基本的には`設定名: 型`の形式で記述します
```tyml
setting: int
```

## 基本型
TYMLには利用可能な基本型がいくつか存在します。
- `int`    : 64bit符号付き整数型
- `uint`   : 64bit符号なし整数型
- `bool`   : 真偽値、`true`もしくは`false`
- `float`  : 実数型
- `string` : 文字列型

## 構造型
構造を持たせる場合には二通りの書き方があります

- インライン表記
```tyml
setting: {
    ip: string
    port: int
}
```

- 型定義
```tyml
type Setting {
    ip: string
    port: int
}

setting: Setting
```

また、構造体の内部の表記にも二通りの書き方があります

- 改行区切り
```tyml
type Setting {
    ip: string
    port: int
}
```

- コンマ区切り
```tyml
type Setting { ip: string, port: int }
```

もちろん２つの記法を混ぜて記述することも有効です
```tyml
type Setting {
    ip: string,
    port: int, // 最後のコンマも問題なし
}
```

原則として、インデントはスペース4つを推奨しますが、タブ文字も構文上は有効です

### 任意の要素を持たせたい場合
```tyml
// settingの場合はstring、それ以外の要素はint型に限定される
setting: string
// *を記述しない場合は、任意の要素を記述することは許可されません
*: int
```
```toml
# !tyml example.tyml
setting = "string"
# *を定義したことにより任意の要素を記述できるようになる
user_element = 100
```

## enum型
enum型を宣言することで、文字列の値を制限することも可能です
```tyml
enum Mode {
    "Debug"
    "Release"
}

// modeは"Debug"もしくは"Release"しか受け付けなくなる
mode: Mode
```

要素の区切り方は構造体と同じく、コンマもしくは改行で区切ることができます
```tyml
enum Mode {
    "Debug",
    "Mode",
}
```