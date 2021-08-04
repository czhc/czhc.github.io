---
title: EIP-1559 Expert Panel
description: AMA panel notes and additional reading materials
categories: [notes]
tags: [ethereum, eip-1559, mev, bankless]
last_modified_at: "2021-08-04"
published: true
---
Quick notes from the AMA.
Will be edited with additional points from Appendix materials

**Panel**

* [Tim Beiko](twitter.com/TimBeiko) (moderator)
* [Hasu](twitter.com/hasufl)
* [Micah Zoltu](twitter.com/micahzoltu)
* [Barnabe Monnot](twitter.com/barnabemonnot)
* [Lightclients](twitter.com/lightclients)

### Bankless EIP-1559 Expert Panel

#### KPIs for EIP 1559:

* Gas used metrics - 1559 introduces doubly-sized blocks to act as damper for demand shocks. are blocks getting too full? how often does 1559 revert to auction on the tip? can we quantify how often 1559 is pushed in this operation mode?
* Series of base fees - base fees are multiplicative of gas used in the previous block.
* how users send txns using wallets, and wallets will depend on oracles to set max fee and priority fee. Find different patterns in how the parameters are set. Backtest how oracles have done

#### Why is 1559 a better UX for new users?
* predicting the current/future gas price is hard.
* wallets depend on oracles to recommend a suitable gas price: gas too high, user is overcharged or gas too low, user txn is not included.
* when demand of block space is stable over a period of time, recommended gas price can be an average over last period. but this more difficult when gas prices are increasing/anomalous in the last block. gas prices / block space demand is hard to predict. oracles may end up underestimating the current recommended gas price. users will end up resubmitting multiple txns for 1 single swap. in which all txns may end up being processed or failed.
* additional risk that new users will be helpless in navigating txn submissions, UX is not intuitive.
* ideal situation with 1559: user txns will be included in the next block with min gas, worst case scenario is still a delay but min gas, and this _new UX applicable to a majority of users_.

#### How should devs think about 1559?
* most changes have been abstracted/taken into account by wallet teams, integrating new RPC endpoints etc.
* **Gas price opcode** will return _effective gas price_ of the txn - including base fee that will be deducted, and will be non-zero.
* contracts that depend on gas price opcode (exception) will not be able to do so. e.g. flashbots generally require that bundles are included with gas price = 0, helps to avoid gas costs if txns are uncled.
* Additional use-cases for new gas opcodes:
    1.  added base fee opcode can be used to price bounty that will be executed on-chain e.g. relayers placing limit-orders on-chain.
    2. market primitives e.g. predictions on base fees (e.g. GAS tokens) can be built.
    3. fraud-proof scaling solutions e.g. rollups, state channels currently provide a static amount of time for fraud proof submissions e.g. optimism ~2 weeks. With base fees, on-chain activity can be analyzed to dynamically adjust for fraud-proof submission periods.
* Post 1559, **legacy gas fees are still supported as max fee per gas or effective fee** depending on before/after txn is mined

#### Why does 1559 work in the interest of miners?
* Understanding how much of fees will be potentially burned with 1559. Mapping total miner revenue to how much is from MEV and how much is from transfers.
* a list of transactions included in a block belongs to multiple markets, namely **Priority markets**: _txns require a specific slot_ to be submitted e.g. arbitrage swaps are preferably ordered first / slot[0] by miners VS **regular markets.**
* priority market fees cannot be _rationally_ burned, otherwise incentivizes priority txns to move offchain.
* All of the MEV extracted on ethereum will not be affected by 1559. an estimated of 30% of fees paid (highly-variable estimate) today are from regular txns which are not specific about ordering.
* Additionally, miners (ETH-denominated) are incentivized to support best UX on ethereum to maximize value of ETH, dependent on increasing economic activity on ethereum.
* @barnabe: _1159 addresses pricing inclusion in a block, agnostic to the position of a txn in a block. - does not address the MEV problem, censorship attacks._

#### Implementing 1559 in other blockchains?
* demand to transact in bitcoin is not high enough in a longer timeframe to pay for security infrastructure - timely when demand picks up. bitcoin does not have significant MEV problems, most txns are standard.
* @hasu: recommend chains that have block subsidies to implement 1559. some blocks already implement this.
* fee-smoothing mechanisms e.g. paying fees into a fund to reimburse activity spikes may be more fitting for bitcoin.

#### What happens when users send old transaction type after 1559 goes live?
* bts converts txn into 1559 type transaction and sets legacy gas price = max fee and max priority fee.
* in 1559, gas fees are refunded as `max fee - (base fee + prority fee)`. legacy txns converted to max fee = max priority fee, resulting in high priority and fees paid to miner. legacy txns will not get a refund as a result.

#### How is the UX for _pending txns_ after 1559?

* with 1559, it is **still possible to have stuck txns** in the event of **_huge_ increasing demand**, compared to legacy where any increase in demand will cause stuck txns. 1559 should only make this less likely.


### Appendix

* Bankless [EIP-1559](https://www.youtube.com/watch?v=ydAHh-BVGms) Expert Panel

* VB: [Blockchain Resource Pricing](https://ethresear.ch/uploads/default/original/2X/1/197884012ada193318b67c4b777441e4a1830f49.pdf)

* VB: First and second-price [auctions](https://ethresear.ch/t/first-and-second-price-auctions-and-improved-transaction-fee-markets/2410) and improved transaction-fee markets

* [Deribit](https://insights.deribit.com/market-research/analysis-of-eip-1559/): Analysis of EIP-1559

* Flashbots: [MEV and EIP-1559](https://hackmd.io/@flashbots/MEV-1559)

* EF: The Problem of [Censorship](https://blog.ethereum.org/2015/06/06/the-problem-of-censorship/)

