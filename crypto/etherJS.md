# Ethers.JS

compact library for interacting with the Ethereum Blockchain and its ecosystem. 

##### Features
- keep private keys in your client, safe and sound
- import and export JSON wallets
- import and export BIP mnemonic phrases (12 word backup phrases)
- meta-classes create Javascript objects from any contract ABI
- connect to etherium nodes over  JSON-RPC, INFURA, Etherscan, ect.
- ENS names are first-class citizens; can be used anywhere an Ethereum addresses can be used
- Tiny; on 284kb uncompressed

```
npm install --save ethers
```

##### Terminology

| Provider | A Provider (in ethers) is a class which provides an abstraction for a connection to the Ethereum Network. It provides read-only access to the Blockchain and its status.|
|-|-|
| Signer | A Signer is a class which (usually) in some way directly or indirectly has access to a private key, which can sign messages and transactions to authorize the network to charge your account ether to perform operations.|
|Contract | A Contract is an abstraction which represents a connection to a specific contract on the Ethereum Network, so that applications can use it like a normal JavaScript object.|




Metamask is a browser extension that provides:
- A connection to the Ethereum network (a Provider)
- Holds your private key and can sign things (a Signer)

##### Contracts

A contract is an abstraction of program code which lives on the Ethereum blockchain.

A Contract is an abstraction of program code which lives on the Ethereum blockchain.

The Contract object makes it easier to use an on-chain Contract as a normal JavaScript object, with the methods mapped to encoding and decoding data for you.

If you are familiar with Databases, this is similar to an Object Relational Mapper (ORM).

In order to communicate with the Contract on-chain, this class needs to know what methods are available and how to encode and decode the data, which is what the Application Binary Interface (ABI) provides.

This class is a meta-class, which means its methods are constructed at runtime, and when you pass in the ABI to the constructor it uses it to determine which methods to add.

While an on-chain Contract may have many methods available, you can safely ignore any methods you don't need or use, providing a smaller subset of the ABI to the contract.

An ABI often comes from the Solidity or Vyper compiler, but you can use the Human-Readable ABI in code, which the following examples use.

### Ethereum Basics
---- 

#### Events
##### Logs and filtering
Logs and filtering are used quite often in blockchain applications, since they allow for efficient queries of indexed data and provide lower-cost data storage when data is not required to be accessed on-chain.

These can be used in conjunction with the Provider Events API and with the Contract Events API
Contract Events API also provides higher-level methods to compute and query this data, which should be preferred over the lower-level filter

##### Filters
when a contract creates a log, 4 pieces of data are hashed and are included in a bloom filter, which allows for efficient filtering. Filters can subsequently have 4 topic-sets, where each topic set refers to a condition that matches the indexed log topic in that position (i.e each condition is AND-ed together)

if a topic-set is null, a log topic in that position is not filtered at all and any value matches.

if a topic-set is a single topic, a log topic in that position must match any one of the topics (i.e. the topic in this position are OR-ed).
###### examples


| ho |-|
|hi|hi|
|-|-|
|hi|hi|
|-|-|


||||||
<table class="table full"><tbody><tr><td align="center" width="25%"><b>Topic-Sets</b></td><td align="center" colspan="3" width="75%"><b>Matching Logs</b></td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ A ]</td><td align="left" colspan="3" rowspan="2" width="75%">topic[0] = A</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ A, null ]</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ null, B ]</td><td align="left" colspan="3" rowspan="3" width="75%">topic[1] = B</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ null, [ B ] ]</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ null, [ B ], null ]</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ A, B ]</td><td align="left" colspan="3" rowspan="3" width="75%">(topic[0] = A) <b>AND</b> (topic[1] = B)</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ A, [ B ] ]</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ A, [ B ], null ]</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ [ A, B ] ]</td><td align="left" colspan="3" rowspan="2" width="75%">(topic[0] = A) <b>OR</b> (topic[0] = B)</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ [ A, B ], null ]</td><td class="fix">&nbsp;</td></tr><tr><td align="center" width="25%">[ [ A, B ], [ C, D ] ]</td><td align="left" colspan="3" width="75%"><b>[</b> (topic[0] = A) <b>OR</b> (topic[0] = B) <b>]</b> <b>AND</b> <b>[</b> (topic[1] = C) <b>OR</b> (topic[1] = D) <b>]</b></td><td class="fix">&nbsp;</td></tr></tbody></table>


##### Solidity Topics
come back to this as its non-essential

#### Gas
##### Gas Price
the gas price is used somewhat like a bid, indicating an amount you are willing to pay (per unit of execution) to have your transaction processed.

##### Gas Limit
come back to this

#### Security
##### Key derivation Functions
This is not specific to Ethereum but is useful technique to understand and has some implications on User Experience.

Encrypting and decrypting an ethereum wallet is slow and can take quite ome time. it is important to understand that this is intentional and provides much stronger security.

The algorithm usually used for this process is Scrypt, which is a memory and CPU intensive algorithm which computes a key (fixed-length pseudo-random series of bytes) for a given password.

Why make the algorithm slow and intensive: The goal is to use as much CPU and memory as possible during this algorithm, so that a single computer can only compute a very small number of results for some fixed amount of time. To scale up an attack, the attacker requires additional computers, increasing cost to brute-force-attack to guess the password.

example: user knows password, it takes them ten seconds to unlock their password. Attacker doesn't know password, it costs them 10 million seconds or 115 days to guess a million passwords of someones.

the trade off is simple, complexity and time consuming for security 
There is no way the algorithm can be faster for a legitimate user without also being faster for an attacker.

##### Mitigating the User Experience
Rather than reducing the security (see below), a better practice is to make the user feel better about waiting. The Ethers encryption and decryption API allows the developer to incorporate a progress bar, by passing in a progress callback which will be periodically called with a number between 0 and 1 indication percent completion.

In general a progress bar makes the experience feel faster, as well as more comfortable since there is a clear indication how much (relative) time is remaining. Additionally, using language like "decrypting..." in a progress bar makes a user feel like there time is not being needlessly wasted.

##### Provider
A Provider is an abstraction of a connection to the Ethereum network, providing a concise, consistent interface to standard Ethereum node functionality. It is a read-only abstraction to access the blockchain data.
`ethers.getDefaultProvider( [ network , [ options ] ] ) ⇒ Provider`

##### Networks
There are several official common Ethereum networks as well as custom networks and other compatible projects.

##### Signers
A Signer in ethers is an abstraction of an Ethereum Account, which can be used to sign messages and transactions and send signed transactions to the Ethereum Network to execute state changing operations.

##### Contract Interaction
A Contract object is an abstraction of a contract (EVM bytecode) deployed on the Ethereum network. It allows for a simple way to serialize calls and transactions to an on-chain contract and deserialize their results and emitted logs.