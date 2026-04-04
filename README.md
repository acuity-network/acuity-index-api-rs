# acuity-index-api-rs

High-level Rust client for the `acuity-index` WebSocket API.

The indexer serves JSON-over-WebSocket on `ws://127.0.0.1:8172` by default.

## Quick Start

Add the crate to your `Cargo.toml`, connect to the indexer, and issue typed requests with `IndexerClient`.

Full API reference: [`API.md`](./API.md)

```rust
use acuity_index_api_rs::{CustomKey, CustomValue, IndexerClient, Key};

#[tokio::main(flavor = "current_thread")]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = IndexerClient::connect("ws://127.0.0.1:8172").await?;

    let spans = client.status().await?;
    println!("indexed spans: {spans:?}");

    let events = client
        .get_events(
            Key::Custom(CustomKey {
                name: "ref_index".into(),
                value: CustomValue::U32(42),
            }),
            Some(100),
            None,
        )
        .await?;

    println!("{} matching events", events.events.len());
    Ok(())
}
```

## Example

```rust
use acuity_index_api_rs::{CustomKey, CustomValue, IndexerClient, Key};

#[tokio::main(flavor = "current_thread")]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = IndexerClient::connect("ws://127.0.0.1:8172").await?;

    let spans = client.status().await?;
    println!("indexed spans: {spans:?}");

    let events = client
        .get_events(
            Key::Custom(CustomKey {
                name: "ref_index".into(),
                value: CustomValue::U32(42),
            }),
            Some(100),
            None,
        )
        .await?;

    println!("{} matching events", events.events.len());
    Ok(())
}
```
