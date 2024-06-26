<div align="center">
  <h1><code>selene-helius-sdk</code></h1>
  <a href="https://docs.rs/selene-helius-sdk/">
    <img src="https://docs.rs/selene-helius-sdk/badge.svg">
  </a>
  <a href="https://github.com/dougEfresh/selene-helius-sdk/actions">
    <img src="https://github.com/dougEfresh/selene-helius-sdk/workflows/Continuous%20integration/badge.svg">
  </a>
  <a href="https://deps.rs/repo/github/dougEfresh/selene-helius-sdk">
    <img src="https://deps.rs/repo/github/dougEfresh/selene-helius-sdk/status.svg" >
  </a>
  <a href="https://codecov.io/gh/dougEfresh/selene-helius-sdk" > 
   <img src="https://codecov.io/gh/dougEfresh/selene-helius-sdk/graph/badge.svg?token=OI06VXUKKJ"/> 
 </a>  
  <a href="https://crates.io/crates/selene-helius-sdk">
    <img src="https://img.shields.io/crates/v/selene-helius-sdk.svg">
  </a>
</div>

# Selene Helius SDK

Async library for [helius](https://docs.helius.dev/) API & RPC

```rust
use color_eyre::Result;
use selene_helius_sdk::api::das::GetAssetsByOwnerParams;
use selene_helius_sdk::HeliusBuilder;

#[tokio::main]
async fn main() -> Result<()> {
  let api_key = std::env::var("HELIUS_API_KEY").expect("env HELIUS_API_KEY is not defined!");
  let helius = HeliusBuilder::new(&api_key).build()?;
  let result = helius
    .get_assets_by_owner(&GetAssetsByOwnerParams {
      owner_address: "86xCnPeV69n6t3DnyGvkKobf9FdN2H9oiVDdaMpo2MMY".to_string(),
      ..Default::default()
    })
    .await?;

  println!("total: {}", result.total);
  for asset in result.items {
    println!("{}", asset.id);
  }

  Ok(())
}
```

---

## Usage

The package needs to be configured with your account's API key, which is available in
the [Helius Dashboard](https://dev.helius.xyz/dashboard/app).

API reference documentation is available at [docs.helius.dev](https://docs.helius.dev).

---

## Supported APIs

### DAS API Status

| Endpoint                                                                                                                         | Status  |
|----------------------------------------------------------------------------------------------------------------------------------|---------|
| [getAsset](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-asset)                             | &check; |
| [getAssetBatch](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-asset)                        | &check; |
| [getAssetProof](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-asset-proof)                  | &check; |
| [getAssetProofBatch](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-asset-proof)             | &check; |
| [getAssetsByOwner](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-assets-by-owner)           | &check; |
| [getAssetsByAuthority](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-assets-by-authority)   | &check; |
| [getAssetsByCreator](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-assets-by-creator)       | &check; |
| [getAssetsByGroup](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-assets-by-group)           | &cross; |
| [searchAssets](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/search-assets)                     | &check; |
| [getSignaturesForAsset](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-signatures-for-asset) | &cross; |
| [getTokenAccounts](https://docs.helius.dev/compression-and-das-api/digital-asset-standard-das-api/get-token-accounts)            | &check; |
| [getPriorityFeeEstimate](https://docs.helius.dev/solana-rpc-nodes/alpha-priority-fee-api#priority-fee-estimate)                  | &check; |

### Enriched Transactions

| Endpoint                                                                                            | Status  |
|-----------------------------------------------------------------------------------------------------|---------|
| [transactions](https://docs.helius.dev/solana-apis/enhanced-transactions-api/parse-transaction-s)   | &check; |
| [history](https://docs.helius.dev/solana-apis/enhanced-transactions-api/parsed-transaction-history) | &cross; |

### Webhooks API Status

| Endpoint                                                                                           | Status  |
|----------------------------------------------------------------------------------------------------|---------|
| [create-webhook](https://docs.helius.dev/webhooks-and-websockets/api-reference/create-webhook)     | &check; |
| [get-all-webhooks](https://docs.helius.dev/webhooks-and-websockets/api-reference/get-all-webhooks) | &check; | 
| [get-webhook](https://docs.helius.dev/webhooks-and-websockets/api-reference/get-webhook)           | &check; | 
| [edit-webhook](https://docs.helius.dev/webhooks-and-websockets/api-reference/edit-webhook)         | &check; | 
| [delete-webhook](https://docs.helius.dev/webhooks-and-websockets/api-reference/delete-webhook)     | &check; |
| [appendAddressesToWebhook](https://docs.helius.dev/webhooks-and-websockets)                        | &check; |

### Mint API

| Endpoint                                                                                          | Status  |
|---------------------------------------------------------------------------------------------------|---------|
| [mintCompressedNft](https://docs.helius.dev/webhooks-and-websockets/api-reference/create-webhook) | &cross; |
| [delegateCollectionAuthority](https://docs.helius.dev/compression-and-das-api/mint-api)           | &cross; |
| [revokeCollectionAuthority()](https://docs.helius.dev/compression-and-das-api/mint-api)           | &cross; |
| [getMintlist](https://docs.helius.dev/compression-and-das-api/mint-api)                           | &cross; |  

## Examples

See [examples](./examples) directory for various ways to use the library

* Create a webhook

```shell
HELIUS_API_KEY=<mykey> cargo run --example get_assets
```

## Development

To run tests you need to export or create a `.env` file with the HELIUS_API_KEY

```shell
HELIUS_API_KEY=mykey cargo test

```

## Bot

There's an example telegram [bot](https://github.com/dougEfresh/selene-helius-bot) which can
create [webooks](https://docs.helius.dev/webhooks-and-websockets/api-reference/create-webhook) and send solana activity
to your telegram channel

---

## Credits

Inspired by sync library for
helius, [https://github.com/bgreni/helius-rust-sdk](https://github.com/bgreni/helius-rust-sdk)
