# EVM-based Chains

The source data is located in `_data/chains`. Each chain has its own JSON file, with the filename following the [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) format and the `.json` extension.

## Example

```json
{
  "name": "Ethereum Mainnet",
  "chain": "ETH",
  "rpc": [
    "https://mainnet.infura.io/v3/${INFURA_API_KEY}",
    "https://api.mycryptoapi.com/eth"
  ],
  "faucets": [],
  "nativeCurrency": {
    "name": "Ether",
    "symbol": "ETH",
    "decimals": 18
  },
  "features": [{ "name": "EIP155" }, { "name": "EIP1559" }],
  "infoURL": "https://ethereum.org",
  "shortName": "eth",
  "chainId": 1,
  "networkId": 1,
  "icon": "ethereum",
  "explorers": [{
    "name": "etherscan",
    "url": "https://etherscan.io",
    "icon": "etherscan",
    "standard": "EIP3091"
  }]
}
```

When an icon is used in either the network or an explorer, there must be a corresponding JSON file in `_data/icons` with the same name.
For example, in the example above, there must be both `ethereum.json` and `etherscan.json`.  
The icon JSON files look like this:

```json
[
  {
    "url": "ipfs://QmdwQDr6vmBtXmK2TmknkEuZNoaDqTasFdZdu3DRw8b2wt",
    "width": 1000,
    "height": 1628,
    "format": "png"
  }
]
```

**Requirements:**
* The URL **must** be publicly resolvable through IPFS.  
* `width` and `height` must be positive integers.  
* `format` must be either `"png"`, `"jpg"`, or `"svg"`.  
* File size must be less than **250 KB**.

---

## Linking to a Parent Chain (L2 / Shard)

If the chain is an L2 or shard of another chain, you can link it to its parent as follows:

```json
{
  ...
  "parent": {
    "type": "L2",
    "chain": "eip155-1",
    "bridges": [{ "url": "https://bridge.arbitrum.io" }]
  }
}
```

The `bridges` field is optional.

---

## Chain Status

You can add a `status` field to indicate lifecycle state:
* `active` (default)
* `deprecated` — for chains no longer in use
* `incubating` — for experimental or early-stage chains

Chains should **never** be deleted to avoid potential replay attack risks.

---

## Aggregated Files

Aggregated JSON files with all chains are automatically assembled and available at:

* https://chainid.network/chains.json  
* https://chainid.network/chains_mini.json (lighter version with fewer fields)

---

## Constraints

* `shortName` and `name` **must** be unique — see [EIP-3770](https://eips.ethereum.org/EIPS/eip-3770) for details.  
* Parent chains referenced must exist in the repository.  
* If using an IPFS CID for an icon, it must be retrievable using `ipfs get` (not only via a gateway).  
* Additional validation rules are enforced in the CI pipeline.

---

## Collision Management

No two chains can share the same `chainId`.  
Allowing duplicates would enable replay attacks.

The first pull request that registers a new `chainId` owns it.  
Subsequent PRs attempting to reassign the same ID will be closed.

Reassignments are only possible when a chain is deprecated (e.g., short-lived testnets).  
In such cases, a `reusedChainID` red flag will be shown to warn clients.

---

## Getting Your PR Merged

### Before Submitting

Run local validation checks:

```bash
./gradlew run
```

Example output:
```
BUILD SUCCESSFUL in 7s
9 actionable tasks: 9 executed
```

Then format JSON files using Prettier according to the repository’s style rules:

```bash
npx prettier --write _data/*/*.json
```

---

### After Submitting

* Ensure CI passes successfully (“green”).  
* If you fix CI issues, re-request a review — maintainers cannot track all updates manually.

---

## Usages

### Tools
* [MESC](https://paradigmxyz.github.io/mesc)

### Explorers
* [Otterscan](https://otterscan.io)

### Wallets
* [WallETH](https://walleth.org)
* [TREZOR](https://trezor.io)
* [Minerva Wallet](https://minerva.digital)

### EIPs
* EIP-155
* EIP-3014
* EIP-3770
* EIP-4527

### Listing Sites
* [chainid.network](https://chainid.network) / [chainlist.wtf](https://chainlist.wtf)
* [chainlist.org](https://chainlist.org)
* [Chainlink Docs](https://docs.chain.link/)
* [dRPC Chainlist](https://drpc.org/chainlist)
* [eth-chains](https://github.com/taylorjdawson/eth-chains)
* [EVM-BOX](https://github.com/izayl/evm-box)
* [evmchain.info](https://evmchain.info)
* [evmchainlist.org](https://evmchainlist.org)
* [networks.vercel.app](https://networks.vercel.app)
* [Wagmi Chain Configs](https://spenhouet.com/chains)
* [chainlist.simplr.sh](https://chainlist.simplr.sh)

### Other
* [FaucETH](https://github.com/komputing/FaucETH)
* [Sourcify Playground](https://playground.sourcify.dev)
* [Smart Contract UI](https://xtools-at.github.io/smartcontract-ui)

*Your project — contact us to add it here!*
