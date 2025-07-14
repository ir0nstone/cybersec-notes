# Smart Contracts and Solidity

TODO - an explanation of what smart contrats are

## Contract Source

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

contract Counter {
    uint256 private counter;

    constructor() {
        counter = 0;
    }

    function increment() external {
        counter += 1;
    }

    function getCounter() external view returns (uint256) {
        return counter;
    }
}
```
