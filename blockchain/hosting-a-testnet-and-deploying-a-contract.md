---
description: Using Foundry to start a local testnet and deploy a Solidity contract.
---

# Hosting a Testnet and Deploying a Contract

## Foundry

We use [Foundry](https://github.com/foundry-rs/foundry) for this.

## Developing a Contract

```
$ forge init
```

in the desired folder.

## Starting a Testnet

```
anvil
```

This will give you an RPC URL, plus 10 accounts and their private keys.

## Deploying a Contract

```
forge create --rpc-url 127.0.0.1:8545 src/Counter.sol:Counter --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --broadcast
```

* The RPC URL should be the one provided by `anvil`
* `src/Counter.sol` is the file path to the smart contract's Solidity source code
* `Counter` is the name of the smart contract
* `--private-key` is one of the private keys displayed by `anvil`
* `--broadcast` broadcasts the deployment of the transaction

```
Deployer: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
Deployed to: 0x5FbDB2315678afecb367f032d93F642f64180aa3
Transaction hash: 0x9c43e48790455815acecf9e5ae2a835daac756fe3f8164eb1a294bf7a768e00e
```

Without the `--broadcast`, foundry will do a "dry run" - simulate the contract's execution without actually deploying it. Broadcasting it will return an address!

## Interacting with the Contract

We can increment the counter as follows:

```
$ cast send 0x5FbDB2315678afecb367f032d93F642f64180aa3 "increment()" --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --rpc-url 127.0.0.1:8545

blockHash            0x85443db3aa3b5761016586e3cb720b28fda0819f179a9ed4f7b45b12fe849695
blockNumber          2
contractAddress      
cumulativeGasUsed    43517
effectiveGasPrice    876013326
from                 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
gasUsed              43517
logs                 []
logsBloom            0x00[...]
root                 
status               1 (success)
transactionHash      0x35d8a6b48a3cef67a9301b6c4d818b294e6ad1eac7e219870d9afd49e3984265
transactionIndex     0
type                 2
blobGasPrice         1
blobGasUsed          
authorizationList    
to                   0x5FbDB2315678afecb367f032d93F642f64180aa3
```

`cast send` is used for transactions that **write to the blockchain**, and therefore **require signing**.&#x20;

`getCounter()` is a `view` function, so does not require a transaction to call. We use `cast call` instead, which does not requires a private key:

```
$ cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "getCounter()" --rpc-url 127.0.0.1:8545
0x0000000000000000000000000000000000000000000000000000000000000001
```

And we see it returns the value `1`! If we `cast send` another increment call and then `cast call` to read the counter again, we see it's incremented again:

```
$ cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "getCounter()" --rpc-url 127.0.0.1:8545 
0x0000000000000000000000000000000000000000000000000000000000000002
```
