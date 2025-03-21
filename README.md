# Gravex SDK

[![npm][npm-image]][npm-url]

[npm-image]: https://img.shields.io/badge/npm%3A%40gravex%2Fgravex--sdk-green?style=flat&labelColor=333
[npm-url]: https://www.npmjs.com/package/@gravexio/gravex-sdk

An SDK for building applications on top of Gravex.

## Usage Guide

### Installation

```
$ yarn add @gravex/gravex-sdk
```

## SDK local test

```
$ yarn dev {directory}

e.g. yarn dev test/init.ts
```

## Features

### Initialization

```javascript
import { Gravex } from "@gravex/gravex-sdk";
const gravex = await Gravex.load({
  connection,
  owner, // key pair or publicKey, if you run a node process, provide keyPair
  signAllTransactions, // optional - provide sign functions provided by @solana/wallet-adapter-react
  tokenAccounts, // optional, if dapp handle it by self can provide to sdk
  tokenAccountRowInfos, // optional, if dapp handle it by self can provide to sdk
  disableLoadToken: false, // default is false, if you don't need token info, set to true
});
```

#### how to transform token account data

```javascript
import { parseTokenAccountResp } from "@gravex/gravex-sdk";

const solAccountResp = await connection.getAccountInfo(owner.publicKey);
const tokenAccountResp = await connection.getTokenAccountsByOwner(owner.publicKey, { programId: TOKEN_PROGRAM_ID });
const token2022Req = await connection.getTokenAccountsByOwner(owner.publicKey, { programId: TOKEN_2022_PROGRAM_ID });
const tokenAccountData = parseTokenAccountResp({
  owner: owner.publicKey,
  solAccountResp,
  tokenAccountResp: {
    context: tokenAccountResp.context,
    value: [...tokenAccountResp.value, ...token2022Req.value],
  },
});
```

#### data after initialization

```
# token
gravex.token.tokenList
gravex.token.tokenMap
gravex.token.mintGroup


# token account
gravex.account.tokenAccounts
gravex.account.tokenAccountRawInfos
```

#### Api methods (https://gravex-io/gravex-sdk/blob/master/src/api/api.ts)

- fetch gravex default mint list (mainnet only)

```javascript
const data = await gravex.api.getTokenList();
```

- fetch mints recognizable by gravex

```javascript
const data = await gravex.api.getTokenInfo([
  "So11111111111111111111111111111111111111112",
  "4k3Dyjzvzp8eMZWUXbBCjEvwSkkk59S5iCNLY3QrkX6R",
]);
```

- fetch pool list (mainnet only)
  available fetch params defined here: https://github.com/gravex-io/gravex-sdk/blob/master/src/api/type.ts#L249

```javascript
const data = await gravex.api.getPoolList({});
```

- fetch poolInfo by id (mainnet only)

```javascript
// ids: join pool ids by comma(,)
const data = await gravex.api.fetchPoolById({
  ids: "AVs9TA4nWDzfPJE9gGVNJMVhcQy3V9PGazuz33BfG2RA,8sLbNZoA1cfnvMJLPfp98ZLAnFSYCFApfJKMbiXNLwxj",
});
```

- fetch pool list by mints (mainnet only)

```javascript
const data = await gravex.api.fetchPoolByMints({
  mint1: "So11111111111111111111111111111111111111112",
  mint2: "4k3Dyjzvzp8eMZWUXbBCjEvwSkkk59S5iCNLY3QrkX6R", // optional,
  // extra params: https://github.com/gravex-io/gravex-sdk/blob/master/src/api/type.ts#L249
});
```
