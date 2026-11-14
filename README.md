![Trí Nguyễn · @thekivashyn. Founder of Gióng, CTO at GreenZone.ai. A thin crescent above a dark horizon.](./assets/nightfall.svg)

I’m Trí, a developer from Vietnam. I’m the founder of [Gióng](https://giong.run), an async runtime for Rust, and the CTO of [GreenZone.ai](https://www.thegreenzone.ai), an AI company in Australia.

### Gióng

[![giong 0.5.0. One syscall per tick, not per operation. 1.75M requests/s on 4 cores of a bare-metal server, about 0.03 system calls per request, all 147 h2spec cases passed, 9 crates on crates.io.](./assets/giong.svg)](https://giong.run)

In Vietnamese legend, Thánh Gióng lay silent for three years, then rose a giant the day his country called. The runtime carries his name because it does the same: it costs next to nothing while idle and uses every core the moment load arrives.

Gióng is a thread-per-core runtime for Linux, built on io_uring, with an epoll driver wherever io_uring is blocked. It brings its own HTTP/1.1 and HTTP/2 server and client, TLS, PostgreSQL, MySQL and Redis clients and a web framework, so a whole service runs on it with no Tokio underneath.

- **1.75M requests/s on 4 cores** of a bare-metal server, against 1.69M for a tuned Monoio and 1.59M for raw Tokio.
- **About 0.03 system calls per request.** A server built on epoll needs 2.
- **HTTP/2 in the box.** It passes all 147 `h2spec` cases and answers 2.3 to 4.3 times as many requests as axum on one core.
- **Made for the agents that write your code.** Every error has a stable code, the cause and the fix.

```rust
use giong::net::TcpListener;
use giong_http::{Request, Response, Server};

async fn handler(req: Request) -> Response {
    match (req.method(), req.path()) {
        ("GET", "/health") => Response::text("ok"),
        ("POST", "/echo") => Response::new(200).body(req.body().to_vec()),
        _ => Response::new(404),
    }
}

fn main() -> std::io::Result<()> {
    giong::Builder::new()
        .shutdown_on_signals(true)
        .build()?
        .run(|_worker| async {
            // One listener per core on the same port; the kernel spreads the connections.
            let listener = TcpListener::bind("0.0.0.0:8080").expect("bind");
            Server::new().serve(listener, handler).await;
        })
}
```

Gióng is at 0.5 and on its way to 1.0. The numbers are from version 0.5.0, and each one has its method and raw data [in the repository](https://github.com/giong-run/Giong#benchmarks), next to the cases where Gióng is not ahead.

[giong.run ↗](https://giong.run) · [Source ↗](https://github.com/giong-run/Giong) · [crates.io ↗](https://crates.io/crates/giong) · [Roadmap ↗](https://giong.run/roadmap/)

### Apps and small worlds

One of my browser games has a tram that crosses town while the players take their turns. The tram has nothing to do with the dice. I just wanted the place to feel lived in.

I build mobile apps, the APIs behind them, and the tools people use to run them. On a map, a pin takes one tap to open. Behind that tap there’s a database, an API, and a moderation screen where someone can correct the place you’re looking at. I like working on all three.

### What I want to leave here

I want to share more of the things I wish I’d found when I started: a small tool you can take apart, an example you can actually run, or the search result I needed three hours into debugging.

[Explore my public work ↗](https://github.com/thekivashyn?tab=repositories&q=&type=public&language=&sort=)

Built something with my code? Tag me on X [@tringuyenvnn](https://x.com/tringuyenvnn). I’d love to see what you made.
