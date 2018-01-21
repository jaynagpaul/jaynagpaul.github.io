---
layout: post
title: How To Create Your Own Cryptocurrency
description: Step by step instructions on how to create a cryptocureency,
image: assets/images/ethereum.jpg
tags: cryptocurrency tutorial
comments: true
---
## Why?
Honestly, I don't even know. 9 out of 10 cryptocurrencies created today provide zero value to the public and won't even exist by the end of 2018. But for some reason you still want to create your own coin, read ahead!

## What Will We Create?
At the end of this blog post we will have a working cryptocurrency which can be traded/purchased on exchanges and transferred between owners of the coin. We shall dub this coin "JayCoin" or $JAY, and it's full implementation shall be under 100 lines of code.

## Getting Started / Setup
To get started on creating JayCoin we're going to need some Ethereum*, so head over to your preferred exchange to buy some. "Why Ethereum?" you may be wondering,but for JayCoin to exist and be under 100 lines we're actually going to build it on the Ethereum blockchain. This is pretty common knowledge, and I'm willing to put money on the fact that many of the coins you've heard of/own are implemented on Ethereum.

*If you're looking to just test out, you can get some free Rinkeby ETH at https://faucet.rinkeby.io and deploy it onto the Rinkeby test network.

After this you'll need to download Tokens by ConsenSys. If you have git installed you can install it by typing `$ git clone https://github.com/Consensys/Tokens`. If you don't have git installed, just head over [here](https://github.com/Consensys/Tokens) to download and unzip the repository.

## Remix
Now head over to [Remix](https://remix.ethereum.org), a web based Solidity IDE. For this to work, you'll need to have [Metamask](https://metamask.io/) installed, or be using a decentralized web browser such as Mist or Cipher. The wallet should also be filled with the Ethereum you obtained during setup. If you're unsure how to do this you can go to your browser's homepage to learn more. **Make sure you are connected to the proper network, real money = mainnet fake money = rinkeby.**

When you arrive at [Remix](https://remix.ethereum.org) it may seem a little overwhelming, don't worry. 

The first thing you want to do is delete the default contract. 

![Delete Contract](assets/images/delete-contract.png)

Next item on the list is to import the previously downloaded contracts into Remix.

![Import Contracts](assets/images/import-contracts.png)

## Deploying
We've successfully imported our contracts to Remix, now it's time to deploy them. Click on the file which says EIP20.sol, and navigate to over to "Run". Click on the text-box right next to Create and enter in "300, "JayCoin", 0, "JAY"". 
The 300 means that only 300 JayCoins will ever be created. 
JayCoin is the name of the coin. 
0 is the number of decimals we should be allowed to use (In this case we can't send half of a JayCoin). 
Jay is the ticker code for the coin.

![Deploy Contract](assets/images/deploy-contract.png)

Click on create, approve the purchase, and sit tight till it confirms. When it's done you should see a box pop up with your token.

## Buying/Selling On An Exchange
You can buy or sell your JayCoin on the Rinkeby Testnet [here](https://etherdelta.com/) by clicking "PPT" at the top and switching it to other and inserting our coin details.

![Etherdelta](assets/images/etherdelta.png)

If you enjoyed this article, make sure to check out [Ledger](https://joinledger.com), a chat group dedicated to cryptocurrency with a focus on development.