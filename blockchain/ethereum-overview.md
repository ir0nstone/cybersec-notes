---
description: The one we need to understand
---

# Ethereum Overview

## Overview

Ethereum is currently the second-biggest cryptocurrency (by market cap, after Bitcoin) but serves an altogether different purpose. While Bitcoin is an alternative digitial currency, Ethereum takes the technology of blockchains a step further to build a decentralised _computing platform_. Yes, code is run _on the Ethereum blockchain_.

There are a host of applications that have already been developed for the blockchain, from games to trading bots.  Ethereum-based apps are written using **smart contracts**, which are analogous to paper contracts (like legal documents) but have a major benefit: they are executed _automatically_. Two individuals who know nothing about one another can trade in complete security by virtue of these smart contracts, because nobody - even the developer - can intercept execution.

For example, say that a company (for some bizarre reason) offers you a simple deal: for every dollar you send them, they will send you two. Bizarre business decisions aside, how can you guarantee that they will? In the real world you would insist upon contracts being drawn up and signed by both parties, and you would ensure that they exchange happened in a country or district where you felt that the law would be successfully upheld if they broke it. In the world of Ethereum smart contracts, none of that is necessary. By observing the code of the smart contract, you can see something very simple: **for every one ether the contract receives, it will send you two**. Even the most treacherous of dealmakers would be unable to break such a contract, because once it is published it is out of their hands (and yes, obviously there are other checks that would need to be undertaken, such as that the wallet has sufficient funds to return the money).

## Proof of Stake

While Bitcoin still works on the old [Proof of Work](https://www.coinbase.com/en-gb/learn/crypto-basics/how-do-cryptocurrency-miners-work) approach - which involves miners - Ethereum recently transitioned to a new [Proof of Stake](https://www.coinbase.com/en-gb/learn/crypto-basics/what-is-proof-of-work-or-proof-of-stake) consensus mechanism. Under Proof of Stake, the Ethereum network chooses a network of participants actively invested in the network, and each participant (or **validator**) contributes ETH to the **staking pool**. This is called **staking**. The network selects a "winner" based on the amount of ether staked, and how long it has been staked for. The idea is that those who have more ether for longer periods of time are **more invested in the long-term success of Ethereum**, and therefore deemed "more trustworthy" to validate a transaction.

Once the winner validates the block, other validators check their work. Once enough validators agree, the blockchain is updated with the block and all participating validators are rewarded in ETH proportional to their stake.

A good overview of the above can be found [here](https://www.coinbase.com/en-gb/learn/crypto-basics/what-is-ethereum).

## The Ethereum Virtual Machine

As mentioned previously, code is run on the Ethereum blockchain. It is the [**Ethereum Virtual Machine**](https://ethereum.org/en/developers/docs/evm/) that enables this,&#x20;
