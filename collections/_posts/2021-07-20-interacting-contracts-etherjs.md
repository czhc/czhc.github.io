---
title: "[Guide] Interacting with smart contracts using ethers.js"
description: How to interact with deployed smart contracts which are unverified on etherscan
categories: [guides, smart-contracts, solidity, ethereum]
tags: [ethers, hardhat, etherscan, smart-contracts, solidity]
last_modified_at: "2021-07-20"
published: true
---

### Interacting with Smart Contracts on Etherscan

Etherscan is **the** block explorer to view onchain data and transactions for Ethereum. It also provides several handy tools including **verified contracts** which allows you to interact with smart contracts without using a dApp or UI.

![Etherscan Contracts Tab](/assets/img/posts/2021-07-20-interacting-contracts-etherjs/etherscan-contracts.png)

For example, you can directly use the [**Uniswap V2 Router**](etherscan.com/address/0x7a250d5630b4cf539739df2c5dacb4c659f2488d#writeContract) to:

* Add or remove liquidity for token pairs using `addLiquidity`
* Swap ETH for tokens using `swapExactETHForTokens`
* Swap tokens for tokens using `swapTokensForExactTokens`

![Swap tokens for tokens](/assets/img/posts/2021-07-20-interacting-contracts-etherjs/swap-tokens.png)


without needing to use the web UI at [app.uniswap.org](https://app.uniswap.org/#/swap).
This can be particularly useful for when the website is unavailable for any number of reasons e.g. when the website is down.


Unfortunately: this feature is only available for **Verified contracts**, which requires the deployer to take a few more steps to upload the source code and dependencies to Etherscan for verification.



### Interacting with unverified contracts


I have a modified `Greeter` contract that allows you to set and get its `greeting` attribute. It is modified to only set this variable if you tip it i.e. `setGreeting()` must receive a non-zero `msg.value`.

I've pre-deployed the `Greeter` at: [https://rinkeby.etherscan.io/address/0x04E97E65487aBb3bb8BFcFCeed3755e18Ce2c5E3#code](https://rinkeby.etherscan.io/address/0x04E97E65487aBb3bb8BFcFCeed3755e18Ce2c5E3#code)


How can you interact with the `Greeter` `greet()` or `setGreeting()`, without having the contract verified on Etherscan?



### Using ethers and contract ABI to interact with deployed contracts


First, you will need to compile the contract ABI. ABIs or Application Binary Interfaces are compiled artifacts from the contracts written in high-level programming languages e.g. Solidity. They can be `.json` files, and they describe information about the contracts e.g. functions, payability, return values and state mutability. You can read more about them [here](https://www.sitepoint.com/compiling-smart-contracts-abi/?ref=czhc.dev)


To compile the contract ABIs from source, you can clone the source-code directly from [smockit-poc](https://github.com/czhc/smockit-poc).


```bash
# clone repo
git clone https://github.com/czhc/smockit-poc


#install dependencies with yarn
npm install


# compile
npx hardhat compile
```


You can now interact with the contract using a combination of the locally-compiled ABIs, and the remotely-deployed contract address `0x7a250d5630b4cf539739df2c5dacb4c659f2488d`

Launch the console using

```sh
npx hardhat console --network rinkeby
```

Inside the console (Javascript):

```js
// 1. Load the Greeter ABI
const Greeter = await ethers.getContractFactory("Greeter");


// 2. Attach the remote contract and its state, to the local ABI
const greeter = await Greeter.attach('0x04E97E65487aBb3bb8BFcFCeed3755e18Ce2c5E3') //deployed contract address


// 3. You can now use the local ABI (functions etc.) to interact with the remote greeter state
// Check what is the "current" greeting stored in the contract. This may differ based on when you're testing this

await greeter.greet()
// => "Hello world"
```


What if you want to change the `greeting` of the Greeter? You will need to call the `setGreeting()` function. Remember that this function requires you to tip / pay a non-zero transaction value to the `Greeter`. You will need to provide a wallet/signer for the function.



```js
// 4. Load your wallet using a private key. The private key

wallet = await new ethers.Wallet('0x000myprivatekey...');


// 5. Attach your wallet with the hardhat provider

signer = await wallet.connect(ethers.provider);


// 6. You can now call `setGreeting`, signed with your wallet to include a transaction fee of 100 wei or 0.0000000000000001 ETH

await greeter.connect(signer).setGreeting('Hello! My name is Bob', { value: 100 });


// 7. Check that your changes have been reflected, after the transaction has completed

await greeter.greet()

// => "Hello! My name is Bob"

```



### Conclusion

That's the quick workaround to interact with or test a pre-deployed contract, using its ABI from source and deployed address.
This guide also covered how to use the web3 library[`ethers.js`](http://ethers.io/) (included in `hardhat-ethers`) to interact with smart contracts; keep in mind you are able to do the same using other web3 libraries e.g [`web3.js`](https://web3js.readthedocs.io/) - but I personally prefer the `ethers` SDK.

It is a good way to quickly test a 3rd party contract which has not been verified, or does not provide UIs to access some methods such as claiming rewards, migrating pools etc. - from personal experience. In some cases, it may be useful to invoke emergency withdraws too.

However, keep in mind that directly interacting with functions may have unforeseen consequences.


