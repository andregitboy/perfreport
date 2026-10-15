# Stage 1: a temporary machine with Rust, used only to build
FROM rust:1.82-slim AS builder
WORKDIR /app
COPY Cargo.toml ./
COPY src ./src
RUN cargo build --release

# Stage 2: a small machine that carries only the finished program
FROM debian:bookworm-slim
COPY --from=builder /app/target/release/perfreport /usr/local/bin/perfreport
ENTRYPOINT ["/usr/local/bin/perfreport"]