# dungeon-token-registry

Remote token list for **DungeonWallet**. Editing this repo adds or updates the
IBC tokens shown in the wallet **without shipping a new app build or store review**.

The wallet fetches [`tokens.json`](tokens.json) on launch, merges it over its
bundled defaults (by `denom`), and caches it offline.

## Add / update / hide a token

1. Edit `tokens.json` — add an entry to `ibcTokens[]`, or change an existing one.
   - To **hide** a token without deleting it, set `"isEnabled": false`.
2. Commit and push to `main`.
3. The wallet picks it up on the next launch. (GitHub's raw CDN caches for a few
   minutes, so the very first launch after a push may still show the previous
   list and update on the following one.)

## Entry shape

Each `ibcTokens[]` entry must match the `IBCToken` type in
`DungeonWallet/src/types/token.ts`. Required fields:

| field | type | notes |
|-------|------|-------|
| `id` | string | unique, e.g. `dungeon-1:phmn` |
| `chainId` | string | e.g. `dungeon-1` |
| `denom` | string | the on-chain denom, usually `ibc/<HASH>` |
| `symbol` | string | e.g. `PHMN` |
| `decimals` | number | e.g. `6` |
| `kind` | `"ibc"` \| `"native"` | |
| `isEnabled` | boolean | `false` hides it |

Optional: `displayName`, `baseDenom`, `baseChainId`, `path`, `sourceChannelId`,
`counterpartyChainId`, `logoUrl`, `coinGeckoId`.

Malformed entries are dropped by the wallet's validator — a bad edit can't crash
the app, it just won't show that token.
