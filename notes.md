# Learning Notes

## Tokio
`cargo expand` tells us this:
```
fn main() -> Result<(), std::io::Error> {
    let body = async {
        HttpServer::new(|| {
                App::new()
                    .route("/", web::get().to(greet))
                    .route("/{name}", web::get().to(greet))
            })
            .bind("127.0.0.1:8000")?
            .run()
            .await
    };
    #[allow(clippy::expect_used, clippy::diverging_sub_expression)]
    {
        return tokio::runtime::Builder::new_multi_thread()
            .enable_all()
            .build()
            .expect("Failed building the Runtime")
            .block_on(body);
    }
}
```

In other words, the job of #[tokio::main] is to give us the illusion of being able to define an asynchronous main while,
under the hood, it just takes our main asynchronous body and writes the necessary boilerplate to make it run on top of tokio’s runtime.
