# Binance Chain as A Blockchain

The purpose of the new blockchain and DEX is to create an alternative marketplace for issuing and exchanging digital assets amongst everyone in the world in a decentralized manner. 

## Consensus Details


As Blockchain, essentially Binance Chain is a p2p network, with multiple clients on it to reach consensus on data state. Binance Chain uses [Tendermint](https://github.com/tendermint/tendermint) consensus mechnism, which is BFT based, and build a dedicated `application layer` upon it. Its simplified architecture is as below:

```
+------------+-----------+
| RPC API    | Web API   |
+------------------------+---------+
| Asset Management | Match Engine  |
+----------------------------------+
| Account Management | Governance  |---------> crypto and blockchain governance
+----------------------------------+
| State Caching and Persisence     +-+
+----------------------------------+ |
| Consensus Protocol               | |
+----------------------------------+ |-----> revised Tendermint
| P2P Protocol                     | |
+----------------------------------+ |
| Networking    |  Database        +-+
+----------------------------------+

```

Please refer to [Tendermint spec](https://github.com/tendermint/tendermint/blob/master/docs/spec/consensus/consensus.md)


## Node Roles

### What is a `Validator`?

Validators are a group of IT infrastructure who take the responsibility to maintain the Binance Chain/DEX data and validate all the transactions. They would join the consensus procedure and vote to produce blocks. The fees would be collected and distributed among all validators. You can consider Validator as "miner" in Bitcoin and Ethereum and similar concepts exist in dPoS blockchain as EOS or dBFT in NEO. The initial validators are selected from trusted members of the Binance community, and will eventually expand to more members as the Binance blockchain and ecosystem matures, this responsibility will be distributed. The decentralized governance procedure would be introduced and executed. More qualified organization/individual can become Validator.


### What is a `Witness Node`?
`Witness Node` are the majority nodes of Binance Chain. Although they do not join consensus process and produce blocks, they do:

- witnes consensus process
- serve as data replicas and propagate the chain states around
- receive transactions and broadcast them to all other nodes including `Validator`.

### What is `Accelerate Node`?
Please check [here](faq.md#what_is_the_accelerated_node).

## Blocking

Binance Chain uses a similar block structure as Tendermint proposes, with a size limit as 10 megabytes. It is expected a block would be produced on a-few-of-seconds level among validators, and can include from 0 upto several thousands of transactions.

## Blockchain State
Blockchain state stores the below information:

- account and balances
- fees
- token information
- trading pairs
- tick size and lot size
- governance information

please note the transactions are not stored as chain state, because they are stored on blocks, while trades are not stored as state either, because they can be reproduced via balances and transactions.

## Cryptographic Design

### Account and Address
For normal users, all the keys and addresses can be generated via Binance [Web Wallet](wallet.binance.org). 

#### Keys
Binance Chain uses the same elliptic curve cryptography as the current [Bitcoin implementation](https://github.com/btcsuite/btcd/tree/master/btcec), i.e. secp256k1. Its private key is 32 bytes while public key is 33 bytes.

#### Address

`Address = RIPEMD160(SHA256(public key))`

The address is presented via [bech32](https://github.com/bitcoin/bips/blob/master/bip-0173.mediawiki) format with a checksum and human-read prefix. For Binance Chain address, the prefix is `bnb` for production network, and `tbnb` for testnet.

#### Signature

Binance Chain uses ECDSA signature on curve Secp256k1 against `SHA256` hash of the byte array of encoded transaction, in the same way as the current Bitcoin, according to RFC 6979 and BIP 62.