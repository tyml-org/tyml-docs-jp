+++
title = "JWT認証"
weight = 5
+++

## authed
`authed`キーワードと`@claim`引数を記述するとJWT認証を利用するコードを生成できるようになります。

```tyml
type Token {
    token: string
}

type Claim {
    iss: string
    sub: string
    iat: int
    exp: int
}

interface API {
    /// あらかじめ登録してトークンを返すメソッドも必要になります
    function register(user_name: string) -> Token

    /// ログインした状態で名前を取得する
    authed function get_user_name(@claim: Claim) -> string
}
```

## 生成されるコード
JWT認証を利用する場合は`JwtValidator`が作成され、JWTのトークンの検証用実装を強制されます。

* Rustの場合

```rs
/// Implement this for your server struct.
///
/// ## Example
/// ```
/// impl JwtValidator for YourServerStruct {
///     fn validate<T: serde::de::DeserializeOwned>(token: &str) -> Result<T, ()> {
///         let claim = jsonwebtoken::decode(
///             token,
///             &DecodingKey::from_secret("* your secret key *".as_bytes()),
///             &Validation::default(),
///         )
///         .map_err(|_| ())?
///         .claims;
///
///         Ok(claim)
///     }
/// }
/// ```
pub trait JwtValidator {
    fn validate<T: serde::de::DeserializeOwned>(token: &str) -> Result<T, ()>;
}
```

生成後に実装をすることでJWTを利用できます。
(Rustの実装には`jsonwebtoken`と`async-trait`と`tokio(features = ["full"])`クレートが必要です)
```rs
use api::{
    serve,
    types::{Claim, JwtValidator, API, Token},
};
use async_trait::async_trait;
use jsonwebtoken::{DecodingKey, EncodingKey, Header, Validation};

struct Server {}

#[async_trait]
impl API for Server {
    async fn register(&self, user_name: String) -> String {
        let token = jsonwebtoken::encode(
            &Header::default(),
            &Claim {
                iss: "localhost".to_string(),
                iat: 0,
                sub: user_name,
                exp: i64::MAX,
            },
            &EncodingKey::from_secret(SECRET.as_bytes()),
        )
        .unwrap();

        Token { token }
    }
    async fn get_user_name(&self, claim: Claim) -> String {
        println!("Login : {}", &claim.sub);

        claim.sub
    }
}

static SECRET: &'static str = "RUST IS GOOD!";

impl JwtValidator for Server {
    fn validate<T: serde::de::DeserializeOwned>(token: &str) -> Result<T, ()> {
        let claim = jsonwebtoken::decode(
            token,
            &DecodingKey::from_secret(SECRET.as_bytes()),
            &Validation::default(),
        )
        .map_err(|_| ())?
        .claims;

        Ok(claim)
    }
}

#[tokio::main]
async fn main() {
    let server = Server {};
    serve(server, "localhost:3000").await.unwrap();
}
```
