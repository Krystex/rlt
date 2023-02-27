# Localtunnel

[![localtunnel](https://img.shields.io/crates/v/localtunnel.svg)](https://crates.io/crates/localtunnel)
[![localtunnel-cli](https://img.shields.io/crates/v/localtunnel-cli.svg)](https://crates.io/crates/localtunnel-cli)

Localtunnel exposes your localhost endpoint to the world, user cases are:
- API testing
- multiple devices access to single data store
- peer to peer connection, workaround for NAT hole punching.

## Client Usage

Use in CLI:

```shell
cargo install localtunnel

localtunnel client --host https://localtunnel.me --subdomain kaichao --port 3000
```

Use as a Rust library:

```shell
cargo add localtunnel-client
```

```Rust
use localtunnel_client::{open_tunnel, broadcast, ClientConfig};

let (notify_shutdown, _) = broadcast::channel(1);

let config = ClientConfig {
    server: Some("https://localtunnel.me".to_string()),
    subdomain: Some("demo".to_string()),
    local_host: Some("localhost".to_string()),
    local_port: 3000,
    shutdown_signal: notify_shutdown.clone(),
    max_conn: 10,
    credential: None,
};
let result = open_tunnel(config).await?;

// Shutdown the background tasks by sending a signal.
let _ = notify_shutdown.send(());
```

## Server Usage

Use in CLI:

```shell
localtunnel server --domain localtunnel.me --port 3000 --proxy-port 3001 --secure
```

Use as a Rust library,

```shell
cargo install localtunnel-server
```

```Rust
use localtunnel_server::{start, ServerConfig};

let config = ServerConfig {
    domain: "localtunnel.me".to_string(),
    api_port: 3000,
    secure: true,
    max_sockets: 10,
    proxy_port: 3001,
    require_auth: false,
};

start(config).await?
```

## Resources

- [localtunnel](https://github.com/localtunnel/localtunnel)