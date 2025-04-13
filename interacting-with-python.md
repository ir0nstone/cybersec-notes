---
description: Using web3.py
---

# Interacting with Python

## Installation

We will use the [`web3.py`](https://web3py.readthedocs.io/en/stable/index.html) package to programatically interact with the Ethereum testnet.

```
$ pip install web3
```

Once installed, we can connect to the RPC using

```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider('http://127.0.0.1:8545'))
```

In order to interact with [contracts](https://web3py.readthedocs.io/en/stable/web3.contract.html#contracts) specifically, we need to be able to provide the [ABI](https://en.wikipedia.org/wiki/Application_binary_interface) (Application Binary Interface) of the contract. An ABI decribes the protocol for interacting with a system, in this case, for interacting with the contract. Effectively, an ABI tells `web3.py` exactly how to interact with the remote contract.

Luckily, we can use Python to generate the ABI from the source code itself. To do so, we need to install [`py-solc-x`](https://solcx.readthedocs.io/en/latest/):

```
$ pip install py-solc-x
```

`py-solc-x` handles versions of `solc`, the Solidity compiler. Different versions _could_ generate different ABIs, so keep an eye on it, but generally it won't matter too much. Before we can use it to generate a contract's ABI, we have to install the version of `solc` that we want:

```python
from solcx import install_solc

# install the latest version
install_solc("0.8.29")
```

## Interacting with Contracts in Python

Now we can use the source code of the Solidity contract to generate an ABI for it. After that, everything is pretty self-explanatory!

```python
from web3 import Web3
from solcx import set_solc_version, compile_source

set_solc_version('0.8.29')

with open('src/Counter.sol') as f:
    compiled_sol = compile_source(f.read(), output_values=['abi'])

contract_id, contract_interface = compiled_sol.popitem()
abi = contract_interface['abi']

w3 = Web3(Web3.HTTPProvider('http://127.0.0.1:8545'))
counter = w3.eth.contract(
    address='0x5FbDB2315678afecb367f032d93F642f64180aa3',
    abi=abi
)

# read state
print(counter.functions.getCounter().call())

# update state
print(counter.functions.increment().transact())
```

Running this code multiple times shows the counter incrementing!

## Accounts on Local Nodes

Typically transactions - such as the last line, with `.transact()` - need to be signed using a private key. Here, we have not specified the private key, yet it still works. Why is that?

Our `anvil` testnet spins up a node locally, with several **unlocked accounts**_._ Running the program locally, `w3` will take the first unlocked account and use it. We can print the unlocked accounts out:

```python
print(w3.eth.accounts)
# ['0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266', '0x70997970C51812dc3A010C7d01b50e0d17dc79C8', '0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC', '0x90F79bf6EB2c4f870365E785982E1f101E93b906', '0x15d34AAf54267DB7D7c367839AAf71A00a2C6A65', '0x9965507D1a55bcC2695C58ba16FB37d819B0A4dc', '0x976EA74026E726554dB657fA54763abd0C3a0aa9', '0x14dC79964da2C08b23698B3D3cc7Ca32193d9955', '0x23618e81E3f5cdF7f54C3d65f7FBc0aBf5B21E8f', '0xa0Ee7A142d267C1f36714E4a8F75612F20a79720']
```

Comparing this to the output of `anvil`, we see that they are the same addresses in the same order. To override this functionality, we can set the variable `w3.eth.default_account`, which defaults to `None` (and when it's `None`, the default account used is `w3.eth.accounts[0]`).

```python
w3.eth.default_account = '0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC'
```

Alternatively, if it's a one-off, you can set the `from` field in the `transact` method:

```python
counter.functions.increment().transact({
    'from': '0x3C44CdDdB6a900fa2b585dd299e03d12FA4293BC'
})
```
