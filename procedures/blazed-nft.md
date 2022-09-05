# Blazed NFT Procedures

## Getting Started
Getting started using Blazed Cash (B$), Blazed Land Token (BLT), and Blazed Bond Token (BBT) is a simple and painless process. To begin, the user must have a/an valid/active citizenship for the Blazed Nation. Gaining citizenship is free and easy, simply sign up for an account at [Blazed One](https://blz.one/), and you will immediately be granted citizenship.

## Contract
To deploy an NFT to the Blazed Blockchain, a contract must be created according to the [BBS-1](https://github.com/blazed-labs/blazed-labs/blob/main/standards/BBS-1.md) standard. This contract must be hosted somewhere, so a site operator must deploy your contract to a room in their business for it to be publicly available. Users can also deploy the contract to a room in an apartment or house, but be advised that private property deployments default to "private" visibility. The property owner can either change their property's visibility (at a risk to personal security) or they may utilize access control lists (ACLs) to declaratively permit access (much less riskier to personal safety). To access certain features, contracts may need to be certified with permits. Permits can be purchased from [Blazed City](https://blazed.city/).

The contract can be written in the following programming/scripting languages: PHP, Node.js, Ruby, Python, Go, Java, C#/.NET, C++/C. We will attempt to create and maintain libraries for all supported languages, but developers are also encouraged to create their own libraries if they wish (as long as it is a supported language). To deploy a contract, do the following:

```js
    const cfg = {
        uri: process.env.URI,
        name: process.env.NAME,
        genesis: block,
        period: [0, 0],
    };
    const newBlock = new Block(cfg);
```
where:
* **name**: name of chain.
* **uri**: URI of chain's default gateway
* **limit**: maximum number of transactions allowed per block (0 if infinite/manual minting)
* **totalNFT**: total number of NFTs in circulation (decimal precision represented by trailing 0's)
* **genesis**: genesis block object

## Minting
Once deployed, an NFT needs a genesis block to initialize it. Once initialized, the generated NFTs can be transferred between wallets, held in wallets as equity, used as leverage in business agreements, and farmed/mined. To mint a new block, call the following function (on the target contract object):

```js
    contract.mintBlock();
```

## Transacting

## Farming

## Mining

## 