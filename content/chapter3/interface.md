+++
title = "基本定義"
weight = 2
+++

## interface

`interface`キーワードを使用するとREST-APIの型を定義できるようになります

```tyml
interface API {

}
```

また、複数の定義にも対応しています。

```tyml
interface API1 {}

interface API2 {}
```

それぞれがコード生成時に各言語のinterface相当にトランスパイルされます。

## function
interfaceの中では`function`キーワードを用いてメソッドを定義できます。

引数や戻り値の型を指定でき、引数はクエリパラメータに対応します。
```tyml
interface API {
    /// helloという名前のメソッド
    /// string型の値を返す
    function hello() -> string

    /// 引数を取ることもできる
    /// ','で区切って複数指定可能
    function arguments(id: int, name: string)
}
```

## @body
`@body`キーワードを使用することでHTTPリクエストのbodyの型を指定できます
```tyml
type BodyType {
    id: int
    name: string
}

interface API {
    function body_test(@body: BodyType)
}
```
