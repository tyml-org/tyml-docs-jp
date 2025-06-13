+++
title = "or型"
weight = 2
+++

## or型
TYMLでは、どちらかの型を許可したいというケースにも対応できるように設計されています
```tyml
/// int型もしくはstring型の値を許可する
setting: int | string
```

- TOMLの場合
```toml
# !tyml example.tyml
setting = 100
```
```toml
# !tyml example.tyml
setting = "string"
```
どちらも有効です