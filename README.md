```markdown
# multievm-tx

A small Node.js utility to automate sending transactions across multiple EVM wallets and multiple recipients.

Features:
- Multiple private keys (comma-separated in .env)
- Multiple recipients in recipients.json (or simple CSV)
- Per-wallet nonce handling (uses pending nonces)
- Retries, delay, and concurrency
- Dry-run mode, configurable gas and waits for confirmations

WARNING: This script requires private keys. Keep .env private and secure. Test on a testnet or with small amounts first.

## Setup

1. Copy the example env and edit values:

   cp .env.example .env

2. Install:

   npm install

3. Edit `recipients.json` (example below) or create a CSV.

4. Run:

   node scripts/multi-tx.js
   or
   npm start

## .env example

Set RPC_URL to the node you'd like to use, and PRIVATE_KEYS to a comma-separated list of keys.

```
RPC_URL=https://rpc.ankr.com/eth_goerli
PRIVATE_KEYS=0xYOUR_PRIVATE_KEY_1,0xYOUR_PRIVATE_KEY_2
RECIPIENTS_FILE=recipients.json
VALUE_DEFAULT=0.001
GAS_LIMIT=21000
GAS_PRICE_GWEI=
MAX_RETRIES=3
DELAY_MS_BETWEEN_TX=500
CONCURRENCY=2
WAIT_CONFIRMATIONS=0
DRY_RUN=false
```

## recipients.json example

The JSON file should be an array of objects:

```json
[
  { "address": "0xRecipient1...", "value": "0.01" },
  { "address": "0xRecipient2...", "value": "0.02" },
  { "address": "0xRecipient3...", "value": "0.005" }
]
```

You can also include `data` if you want to send a contract call payload:
{ "address": "0xContract...", "value": "0", "data": "0x..." }

## Notes and safety

- The script currently uses ETH transfer format. For ERC-20 token transfers or contract interactions you will need to populate `data` (ABI-encoded) and adjust `gasLimit` accordingly.
- This script uses provider.getTransactionCount(..., "pending") to set nonces; in high-throughput scenarios you may want a more robust nonce manager (e.g., a shared DB).
- Always dry-run on testnet first (`DRY_RUN=true`).
- Keep your private keys secure and do not commit .env.

## Extending

I can:
- Add ERC-20 transfer support (automatically encode transfer)
- Add multi-chain support (per-recipient RPC and chainId)
- Add CSV support with headers
- Add better nonce manager and throttling

Tell me which of the above you want and I’ll adapt the script accordingly (I can add features and prepare a PR).
```