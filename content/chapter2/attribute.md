+++
title = "篩型"
weight = 5
+++

## 篩型(ふるいがた)
TYMLでは型に追加の制約を課すことができます
```tyml
port: int @value 100..<200
```

## `@value`アトリビュート
数値型である`int`・`uint`・`float`のいずれかで使用することができます
```tyml
setting: int @value 0..<100
```
`0..<100`のように範囲を指定することができ、記述方法は以下のとおりです

* `start..<end`: start以上end未満
* `start..=end`: start以上end以下
* `start..`: start以上
* `..<end`: end未満
* `..=end`: end以下

## `@length`アトリビュート
文字列型`string`のUnicode文字数を制限することができます
```tyml
setting: string @length 0..<100
```
数値の範囲指定方法は`@value`と同一です

## `@u8size`アトリビュート
文字列型`string`のUTF-8バイト数を制限することができます
```tyml
setting: string @u8size 0..<100
```
数値の範囲指定方法は`@value`と同一です

## `@regex`アトリビュート
文字列型`string`の文字列形式を正規表現で制限することができます
```tyml
// 'で囲うとエスケープされません
setting: string @regex '^\d+$'
```

## 複数の条件
`and`や`or`を使うことで型の条件を複数記述できます
```tyml
// andのほうが優先度が高いです
// ()で囲うと優先度を変更できます
setting: string (@length 0..<10 or @length 100..<200) and @regex '^\d+$'
```