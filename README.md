# **ARIS**

## Introduction

ARIS, an acronym for AR + Inscription, is a protocol based on the Arweave network and is part of the Inscription project. The protocol is designated as PRC-20 and represents a new technological advancement stemming from the Ordinals technology. Its distinctive feature lies in utilizing Arweave as the data storage layer, facilitating the creation of immutable Arweave-native digital artifacts.

Arweave functions as a storage-oriented blockchain with limitless storage capabilities. Whether dealing with 1 GB or 10 GB of data, storage can be accomplished through a single transaction on Arweave. Thanks to Arweave's specialized expertise in blockchain storage, the cost of storing 1 GB is kept below $10 (view fees at: https://ar-fees.arweave.dev/), significantly lower than Bitcoin's expenses. Arweave's technology empowers inscriptions with lower costs and higher availability.

Traditional inscriptions are constrained by the limitations of blockchain storage, resulting in very limited practical use cases. Arweave's inscription system is poised to truly unlock application spaces, extending beyond DNS, NFTs, content creation, and encompassing social and gaming applications.

For insights into the principles of Ordinals, refer to: https://docs.ordinals.com/introduction.html

Learn more about Arweave at: https://arweave.org/

## Inscription

Similar to Bitcoin, Arweave does not offer smart contract functionality, and it does not support Script and OP. The PRC-20 protocol involves inscribing the metadata and transaction data of Inscription onto the Arweave network. Consistency proof of the ledger is ensured through the use of standard indexers.

PRC-20 processes transactions in JSON format and is comprised of three fundamental transaction types: **`deploy`**, **`mint`**, and **`transfer`**.

**Deploy**

To deploy an inscription, developers are required to submit the corresponding JSON format document to Arweave, and the receiving address must be the black hole address **`AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA`**.

All characters should be in lowercase, and the length of **`tick`** should be 4 characters. To mitigate dust attacks, the deploy operation must transfer 1 AR to the black hole address.

```Json
{ 
  "p": "prc-20",
  "op": "deploy",
  "tick": "aris",
  "max": "66000000",
  "lim": "100",
  "burn": "0.01"
}
```

**Mint**

Complete a mint transaction by submitting a JSON format document. The receiving address must be the black hole address **`AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA`**.

Ensure that **`tick`** in mint matches the **`tick`** in deploy. The **`amt`** must less than or equal to the **`lim`** in deploy. Burning a specified quantity of AR, as indicated in the deploy transaction, is necessary for the mint operation.

```Json
{ 
  "p": "prc-20",
  "op": "mint",
  "tick": "aris",
  "amt": "100"
}
```

**Transfer**

Transfer the owned inscription from one user to another.

```Json
{ 
  "p": "prc-20",
  "op": "transfer",
  "tick": "aris",
  "amt": "100",
  "to": "AAA..AA",
}
```

## How ARIS Work

- First, inscribe and deploy the inscription. This sets rules for the PRC-20. If the same code (case insensitive) has already been deployed, a second deployment will be invalid.
- Anyone can inscribe and mint inscriptions, with limitations set in the deploying inscription, until the minted balance reaches the "max" set in the deploying inscription.
- The last minted inscription will mint the remaining tokens. For example, if only 100 tokens remain under the code, but the minted inscription wants to mint 500, it will mint the remaining 100 tokens.
- When a wallet mints PRC-20 tokens (inscribing the minting inscription to its address), both its total balance and available balance will increase.
- Wallets can inscribe transfer inscriptions, not exceeding their available balance.
- When this transfer inscription is transferred, the original wallet's total balance will decrease, while the recipient's total balance and available balance will increase.
- If you transfer the transfer inscription to your own wallet, your available balance will not change.

### Index Rules

Here are the detailed indexing rules we follow:

- Inscriptions must be valid JSON (not JSON5).
- JSON must have "p", "op", "tick" fields, where "p"="prc-20", "op" is in ["deploy", "mint", "transfer"]
    - If op is deploy, JSON must have a "max" and "burn" field. "lim" is optional. If "lim" is not set, it will be equal to "max". the minimum mint "burn" fee is 0.001 ar
    - If op is mint or transfer, JSON must have an "amt" field.
- All required JSON fields must be strings. Numerical values like max, lim, amt, etc., are not accepted. Additional fields not discussed here can be of any type.
- Empty strings in numerical fields are invalid.
- 0 in numerical fields is invalid.
- The maximum value for any numerical field is uint64_max.
- "tick" must be 4 bytes wide (accepts UTF-8). "tick" is case-insensitive, and we use lowercase letters to track stocks (convert tick to lowercase before processing).
- If the amt of a mint or deploy exceeds lim, it will be ignored.
- If the amt of a transfer deployment exceeds the available balance of that wallet, it will be ignored.

## Bridge

To deliver an unparalleled user experience, ARIS will leverage the renowned cross-chain payment protocol everPay within the Arweave ecosystem (check everPay: [https://everpay.io](https://everpay.io/)) as its settlement tool. EverPay supports 6 public chains, and using this protocol can reduce the barriers to Arweave inscription, facilitating the payment of storage fees by crossing assets such as ETH and USDT from the Ethereum mainnet to the Arweave ecosystem.

You can use the following link to cross assets like ETH and USDT to the Arweave ecosystem: [https://app.everpay.io/deposit/](https://app.everpay.io/deposit/ethereum-eth-0x0000000000000000000000000000000000000000).

After cross-chain transfer, ETH and USDT can be exchanged for AR using Arweave's native DEX, accessible at: https://app.permaswap.network/.

If you are an Arweave native user, it is also necessary to deposit AR into everPay for the payment of the inscription's burn fee.

## Guide

Coming soon!
