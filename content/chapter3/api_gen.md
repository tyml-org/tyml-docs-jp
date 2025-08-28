+++
title = "コード生成"
weight = 4
+++

## インストール
TYMLの各種CLIツールは
```bash
cargo install tyml_core tyml_api_generator tyml_mock
```
でインストールできます。(Rustのcargoが必要です)

ここではAPI生成ツールについて説明します。

## コマンド

```bash
tyml-api-gen --help
```
このコマンドを実行すると以下のように出るはずです。

```
TYML: type checker for markup language

Usage: tyml-api-gen <COMMAND>

Commands:
  server
  client
  help    Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```
実行結果のとおりに、`tyml-gapi-gen`には２つのサブコマンドがあります。

### サーバー
```bash
tyml-api-gen server <KIND> <TYML> <DIR> [NAME]
```
サーバーコマンドは3つの必須引数と、一つのオプション引数があります。
それぞれの引数の取れる値は以下のとおりです。
```
 - KIND: 生成するサーバーの種類
    指定できる値: rust-axum

 - TYML: 生成に使用するTYMLファイル

 - DIR: 生成先のディレクトリ、パッケージやクレートのディレクトリになる

 - NAME: パッケージやクレートの名前、デフォルトは"api"
```

 * 例
```bash
tyml-api-gen server rust-axum api.tyml ./api-example-server/api
```

### クライアント
```bash
tyml-api-gen client <KIND> <TYML> <DIR> [NAME]
```
クライアントコマンドは3つの必須引数と、一つのオプション引数があります。
それぞれの引数の取れる値は以下のとおりです。
```
 - KIND: 生成するクライアントの種類
    指定できる値: typescript

 - TYML: 生成に使用するTYMLファイル

 - DIR: 生成先のディレクトリ、パッケージやクレートのディレクトリになる

 - NAME: パッケージやクレートの名前、デフォルトは"api"
```

 * 例
```bash
tyml-api-gen client typescript api.tyml ./api-example-client/api
```
