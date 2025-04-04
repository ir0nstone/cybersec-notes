# An Introduction to Blockchain

One category that's been popping up a lot more in recents CTFs is _blockchain_. This typically takes the form of something called ethereal _Smart Contracts_ and their exploitation, generally on an ethereum testnet.

This section is going to provide you with an overview of cryptocurrencies, blockchain and their various uses. We will also introduce the concept of a _Smart Contract_, which is the main component of blockchain CTF challenges.

{% hint style="warning" %}
To warn you right now: I know _very_ little about this field, and anything that you read here has a surprisingly decent chance of being incorrect. I am also learning! If you come across something that you know or think is incorrect, I recommend you get in touch with me, either via Twitter or Discord!
{% endhint %}

## What is a Cryptocurrency

A **cryptocurrency** (often called "crypto" by people that don't understand it, though [I will never](https://www.cryptoisnotcryptocurrency.com)) is a _decentralised_ digital currency - it is not reliant on a central authority, such as a government or bank, in order to function. It relies on a technology called **blockchain**, which is a list of records (called blocks) that are "chained" together using cryptographic hashes to validate their integrity.

Among other things, they provide a higher level of **anonymity** due to their decentralised nature. While all transactions are in the open (making them not as anonymous as one would like to think), the only identifier in a transaction is a **public key** that identifies the location that the cryptocurrency was sent to and from.

There is a lot of sentiment around about cryptocurrencies, fuelled in no small part by the [proliferation of scams](https://www.nbcnews.com/tech/security/crypto-scams-stole-56-billion-americans-last-year-mostly-older-people-rcna170410) in recent years. You might know a fair bit about cryptocurrency, and not be a fan (I certainly am not). Luckily, this doesn't affect your ability to test its security!

I will say that the _maths_ behind it and the technology itself is very cool. If you look into it more, you might find the same - a basic understanding of cryptocurrencies can really help, and [this 3blue1brown video](https://www.youtube.com/watch?v=bBC-nXj3Ng4) is an outstanding starting point. It describes Bitcoin, but most cryptocurrencies work on similar principles.

## What is it worth?

One overarching narrative among the cryptocurrency haters is that "they have no real worth" or "their worth is purely speculative" - that is, their worth can fluctuate wildly as their worth is not tied to real-world assets.

Of all arguments against cryptocurrency (and there are a lot of those), this one is not particularly strong. For comparison, what is the dollar pegged to? What about the pound? Both used to be tied to the [worth of gold](https://en.wikipedia.org/wiki/Gold_standard), but this was abandoned by the former in 1971 and the latter as early as 1931 (there were various issues with these, such as the [undesirable consequence of rising gold production](https://www.parliament.uk/business/publications/research/olympic-britain/the-economy/small-change/)). So what is their worth now? The worth of USD or GBP is nothing more than your _belief_ that it is worth something, and the similar belief of all other people in society: its _inherent value_ is non-existent. These currencies are called [fiat currencies](https://www.britannica.com/money/fiat-money), and their worth is essentially backed by the reliability and stability of the governments that supply them, which is why the US dollar is the de facto international standard - the US is considered _trustworthy_.

The difference between cryptocurrencies and fiat currencies, therefore, is that the latter has simply been around for long enough to have a value, while the former has not. If adoption is high enough, specific cryptocurrencies may become equivalent to fiat currencies. In fact, Bitcoin has a [**hard limit**](https://www.kraken.com/en-gb/learn/how-many-bitcoin-are-there-bitcoin-supply-explained) of 21 million bitcoin in existence, providing it with an inherent scarcity that might make it even more suitable for a currency. Other cryptocurrencies, like Ethereum, have no hard cap.

## Who uses cryptocurrencies?

The classic example of cryptocurrency users is scammers and criminals, due to the increased anonymity. However, a lot of _money-related_ institutions also invest a lot in them (think hedge funds, traders, etc) - anything that doesn't really care for its purpose, but cares to make as much money as possible and has access to sufficient funds to make it happen.

Cryptocurrencies are also popular in developing countries with weak economies and high inflation, such as [Argentina](https://vitalik.eth.limo/general/2024/01/31/end.html). The rationale behind this is that if the currency will depreciate so rapidly, exchaning it to a cryptocurrency will enable you to exchange it back and the value will - _in theory_ - have roughly kept up with inflation (because cryptocurrencies are also traded with more stable currencies such as the dollar).

## What sucks about it?

&#x20;A lot! From massive [energy usage](https://www.nytimes.com/interactive/2021/09/03/climate/bitcoin-carbon-footprint-electricity.html) to its [cult-like followers](https://www.ft.com/content/9e787670-6aa7-4479-934f-f4a9fedf4829), there's no shortage of reasons to dislike it. Unsurprisingly, anything with the potential to make or break your entire bank account is met with a decent level of scepticism.



