+++
title = "配列"
weight = 3
+++

## 配列型
TYMLでは配列型もサポートされます
```tyml
/// string型の配列
settings: [string]
```

or型の配列も許可されます
```tyml
settings: [string | int]
```

- TOMLの場合
```toml
# !tyml example.tyml
settings = ["string1", "string2", 100]
```