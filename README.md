# Rust Actix

Based upon the official [Getting Started Guide](https://actix.rs/docs/getting-started)

## Hello, world!

Start by creating a new binary-based Cargo project and changing into the new directory:

```bash
cargo new hello-world
cd hello-world
```

Add actix-web as a dependency of your project by adding the following to your Cargo.toml file.

```rust
[dependencies]
actix-web = "4.4.0"
```

Replace the content of `src/main.rs` with the following content.

```rust
use actix_web::{get, post, web, App, HttpResponse, HttpServer, Responder};

#[get("/")]
async fn hello() -> impl Responder {
    HttpResponse::Ok().body("Hello world!")
}

#[post("/echo")]
async fn echo(req_body: String) -> impl Responder {
    HttpResponse::Ok().body(req_body)
}

async fn manual_hello() -> impl Responder {
    HttpResponse::Ok().body("Hey there!")
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(hello)
            .service(echo)
            .route("/hey", web::get().to(manual_hello))
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}
```

That's it! Compile and run the program with cargo run. The `#[actix_web::main]` macro executes the async main function within the actix runtime. Now you can go to `http://127.0.0.1:8080/` or any of the other routes you defined to see the results.