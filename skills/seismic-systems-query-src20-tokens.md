---
generated: '2026-07-21'
method: generated
name: Query SRC20 tokens via the Factory REST API
description: List the SRC20 tokens deployed through the Seismic SRC20 Factory and read a single token's public metadata by address.
api: openapi/seismic-systems-src20-factory-openapi.yml
operations: [listTokens, getToken]
source: >-
  Grounded in openapi/seismic-systems-src20-factory-openapi.yml (generated from
  https://docs.seismic.systems/getting-started/src20-factory/api.md). Both
  operationIds are verified in that spec.
---

# Query SRC20 tokens via the Factory REST API

The SRC20 Factory REST API is a read-only service that returns **public** metadata for
tokens deployed through the factory. It uses a public provider and holds no key, so it
can read only public on-chain state — never shielded balances or allowances.

## Base URL
- `http://localhost:3001` (the factory service as documented in the quickstart). See
  `conventions/seismic-systems-conventions.yml`.

## Auth
- None. The API requires no credentials. See `authentication/seismic-systems-authentication.yml`.

## Steps
1. **List deployed tokens** — `listTokens` (`GET /api/tokens`). Returns `{ count, tokens[] }`;
   each token carries `address`, `name`, `symbol`, `decimals`, `owner`, `total_supply`.
2. **Read one token** — `getToken` (`GET /api/token/{address}`) with the token contract
   address. Returns the same metadata object for that token.

## Notes & errors
- Addresses are returned as **lowercase hex** (not EIP-55 checksummed) — normalize before
  comparing to checksummed addresses.
- A malformed address returns HTTP `400` with `{ "error": "Invalid address: ..." }`; an
  unknown address returns `404`. See `errors/seismic-systems-problem-types.yml`.
- To read shielded values (individual balances, encrypted transfers) you must use a client
  SDK with a wallet and signed reads (`seismic-viem`, `seismic-web3`, or `seismic-alloy`),
  not this REST API.
