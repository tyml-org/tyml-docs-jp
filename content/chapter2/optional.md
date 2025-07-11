+++
title = "Optional型?"
weight = 4
+++

## Optional型
値を入力しなくても良い、という場合を表す方法もあります。

型の右側に`?`をつけるだけです。
```tyml
/// 入力する場合はint型である必要があるが、入力しなくてもOK!
setting: int?
```

## `?`の記述位置
型の右側に`?`を記述すると説明しましたが、厳密には、型名と配列型の右側に記述することができます。

```tyml
/// int? もしくは [int] 型です
/// int? | [int]? と記述した場合と同一です
setting: int? | [int]
```

- 配列の内部で使う場合
```tyml
/// これは int? の配列です
/// JSONでは、[null, 100] のような値が許可されます
/// TOMLのようなnullの無い言語では、空配列でしか表現できません
setting: [int?]
```

- 許可されない記法
```tyml
/// この記法は許可されません！
setting: int |?
```