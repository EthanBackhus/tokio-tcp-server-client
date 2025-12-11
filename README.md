# Tokio TCP Server–Client

A simple asynchronous Redis-like server written in Rust using Tokio and the mini-redis protocol.

## Features

- Tokio runtime for async execution
- Mini-Redis client for sandboxing and testing
- Server-side std library mutex to share state data across tasks
- Client-side message passing to concurrently send Redis commands using channels

## Client Architecture

- **mpsc channel**: Multiple tasks send `GET` and `SET` requests to a single manager task
- **oneshot channels**: Each request includes a one-time response channel for returning results
- **manager task**: Owns the Redis connection and processes all commands sequentially
- **concurrent tasks**: Each task can issue commands without directly touching the Redis connection

This design cleanly separates communication, concurrency, and networking by multiplexing all client requests over a single Redis connection.

```bash
# Clone the repository
git clone https://github.com/EthanBackhus/tokio-tcp-server-client.git
cd tokio-tcp-server-client

# Build the project
cargo build --release

# Run the server
cargo run --bin server

# In another terminal window, run the client separately
cargo run --bin client
```