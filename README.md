

<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 1; ALERTS: 7.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>
<a href="#gdcalert5">alert5</a>
<a href="#gdcalert6">alert6</a>
<a href="#gdcalert7">alert7</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>


# Terra Classic Community DEX


## Executive Summary

Main motivations:



* Generate revenue at the protocol level and create value for the token holders
* Refill the Oracle/Community pools and burn the LUNC supply
* Develop a strategy to make the platform self sustainable
* Innovate to (re)establish Terra Classic as a top DEFi protocol

Leverage the existing DEFi infrastructure of the protocol to implement a DEX and generate revenues from transaction fees by capturing a share of the trading volume in pairs such as LUNC/USTC. This revenue will belong to the protocol and the amount used to refill the pools and/or burn the LUNC supply will be decided with _Param Change Proposals_ via governance.

The oracle pool contains currently ~280B LUNC tokens ~1B USTC (around 65M$ in total). The idea behind this project is to borrow some of these coins internally to put them at work so the protocol can earn a yield under the form of transaction fees. **These coins will remain the property of the protocol at all times. They never leave the protocol. **


# Liquidity core module

This is a UNISWAP v3 DEX **implemented as a cosmos core module** listing LUNC/USTC. Users can already swap LUNC/USTC in Terra Station but the swap is routed to an external dApp and is **not part of the protocol**. This will be implemented as a core module which means all the transaction fees from swapping will be sent to the oracle, the community pool and/or to burn the LUNC supply. The split between pools and burn will be determined by three parameters (% oracle pool, % community pool and % burn) which will be set via governance with _Param Change Proposal_.

**This is not the Terra market swap, it is not the swap used to peg USTC to USD**. It simply allows you to swap your LUNC or USTC in Terra Station. **This is not a dApp** **and the protocol is the liquidity provider. **In the example below we’re using a 1% transaction fee as an example but this will be set via governance with _Param Change Proposal_.

![dex](https://github.com/lehajam/terra-classic-dex/blob/main/dex.png)

Everytime people trade against the community DEX, they pay a transaction fee. The liquidity providers receive this fee. In this case, the protocol is the liquidity provider which** **means** all the fees will be paid to the protocol**. The funds used to provide liquidity will be **borrowed** from the oracle pool, **they will remain in the protocol and generate more money for it. The funds are NOT taken out of the protocol.**

The risk associated with providing liquidity is called impermanent loss (IL) and will be mitigated by implementing the UNISWAP V3 concentrated liquidity feature. Indeed we have access to market prices within core modules thanks to the Oracle so we (the protocol) can provide liquidity dynamically with a tight range around the spot price and rebalance at a given interval (every X blocks). This also means we’ll get more fees by providing less liquidity.

![uniswap_v3](https://github.com/lehajam/terra-classic-dex/blob/main/uniswap_v3.png)

**<span style="text-decoration:underline;">Revenue estimation</span>**

The tool: [https://science.flipsidecrypto.xyz/uniswapv3/](https://science.flipsidecrypto.xyz/uniswapv3/)

The assumptions:



* SHIB/WETH as a proxy for the pair LUNC/USTC
    * tdlr: simulation with SHIB/WETH as if it was LUNC/USTC
* Provide liquidity at -6%/+10% of the spot price
    * tdlr: we earn fees as long as the price is in the range -6%/+10% of current price
    * We have access to oracle prices so we can add/remove liquidity anytime to remain in this range
* Daily trading volume of 10,000,000$
    * tdlr: trading volume for LUNC/USTC
* Swap fee of 1%
    * tdlr: how much does the protocol earn per swap
* Liquidity position of 10,000,000$
    * tdlr: **borrow** 10M$ from the oracle pool to deposit as liquidity for people to trade against

Simulation results:



* 283% APY
* ~40k$ / per day
* This does **not** take into account compounding eg. we don’t reinvest what we earn to earn more

These are indicative numbers only and will be estimated more accurately using our in house open source tools: [https://cpy-amm.readthedocs.io/en/latest/readme.html](https://cpy-amm.readthedocs.io/en/latest/readme.html)


# Terra Indices

More volume means more trading fees, so we need more pairs than LUNC/USTC. Obviously we don’t want to become the new Osmosis on the other hand we need to increase the protocol revenue. For that we are planning to support LUNA and ATOM so we can list the pairs LUNC/LUNA, LUNC/ATOM, LUNA/USTC, ATOM/USTC and LUNA/ATOM.

To get access to the LUNA and ATOM tokens we can either submit proposals to the respective protocols and ask them to provide liquidity in exchange for a share of the transaction fees or swap tokens from the oracle pool. This method (the swap) is preferred as we don’t have to share transaction fees and it helps diversify our risks. **Please note that the newly obtained tokens (LUNA and ATOM) will still remain the property of the protocol at all times. **

To further increase the trading volume while innovating, we will offer DEFi powered **Terra Indices**: A one click swap to exchange your LUNC, or USTC for an index: Your traditional index product allowing to diversify your risk and gain exposure to multiple coins (LUNC and LUNA for example) in one click, powered by DEFi.


![lp_pool](https://github.com/lehajam/terra-classic-dex/blob/main/lp_pool.png)


To be clear this is not designed to push LUNC holders towards LUNA. It is designed to maximize the trading volume and provide arbitrage opportunities (see below) from few tokens which make sense to trade for our current holders: LUNC and USTC holders have been airdropped LUNA, some LUNA holders have or may want to have exposure to Terra Classic. **It burns LUNC and gives utility to USTC. **

We will start with one index (LUNC, LUNA) but will support more in the future with new listings added via governance. Indices must always include LUNC. These indices will always be equally weighted to favor Terra Classic eg. for the first one we’ll have (50% LUNC, 50% LUNA).

When minting a new index, a fee will be captured and used to burn LUNC. This is set with a _Param Change Proposal_. **It is a nice way to reintroduce the burn tax in a place where it won’t be noticed.** An index can be minted and paid for using USTC only but can be redeemed against the constituents eg. LUNC + LUNA. We’ll also add pools with Terra Indices so we can arb internally the index against its constituents.

![terra_index](https://github.com/lehajam/terra-classic-dex/blob/main/terra_index.png)

Let’s be 100% clear here, **NO LUNC TOKENS WILL BE MINTED, **when you buy an index, LUNC and LUNA will be taken from the circulating supply by swapping against the DEX and put aside because they’re now part of the index. A new token representing the ownership of this index will be minted.

In fact, buying an index will not only reduce the **total** **supply** by burning, but also reduce the **circulating** **supply** by locking the tokens away.


# Flash Loans for Flash Swaps

Arbitrage (source: wikipedia):

_“In simple terms, it is the possibility of a risk-free profit after transaction costs. For example, an arbitrage opportunity is present when there is the possibility to instantaneously buy something for a low price and sell it for a higher price.”_

Flash loans (source: moonpay):

_“Flash loans are uncollateralized loans without borrowing limits in which a user borrows funds and returns them in the same transaction. If the user can’t repay the loan before the transaction is completed, a smart contract cancels the transaction and returns the money to the lender.”_

![flash_loan](https://github.com/lehajam/terra-classic-dex/blob/main/flash_loan.png)

Tokens from the oracle pool are currently sitting idle without earning any yield. Lending them **internally** via flash loans to allow the protocol to flash swap and arb its own pools will help increase the trading volume of the DEX while generating a **risk free yield** on the tokens from the oracle pool.

At a given interval (every X blocks), **the protocol** can take a flash loan from the oracle pool in order to perform a triangular arbitrage between the pools. This will boost the profit made from the DEX as well as generate a risk free yield on the tokens in the oracle pool in the form of transaction fees and arbitrage profit.

![triangular_arb](https://github.com/lehajam/terra-classic-dex/blob/main/triangular_arb.png)

We also want to be able to use flash loans to arb between the index and its constituents, for example, you buy the index and redeem it when the index price is lower than the constituents or you mint the index and redeem it when the index price is higher than the components.

![index_arb](https://github.com/lehajam/terra-classic-dex/blob/main/index_arb.png)

# Funding and development costs

**<span style="text-decoration:underline;">Development costs</span>**

_Funding will be requested using Community-Spend proposals and paid in LUNC_

_USD amounts are only used for illustration purpose, we used a reference rate of LUNC/USD = 0.0001515_

Two senior developers over two months and one senior quantitative analyst dedicated to the market making and arb strategies. This includes full testing (testing of the code, back testing and simulations of the quant strategies).



* Liquidity core module: **330,000,000 LUNC** (50,000 USD)
* Flash swaps: **198,000,000 LUNC** (30,000 USD)
* Terra Indices: **132,000,000 LUNC** (20,000 USD)
* Liquidity strategy and arbitrage : **165,000,000 LUNC **(25,000 USD)
* Total cost: **825,000,000 LUNC** (125,000 USD)

This will be requested as three separate _Community Spend Proposal_ as follow:



* Liquidity core module: **330,000,000 LUNC**
* Flash Swap and Terra Indices: **330,000,000 LUNC**
* Liquidity strategy, arbitrage and rebalancing: **165,000,000 LUNC**

All proposals could be requested at the same time but we believe it is better to proceed with the liquidity core modules first, show some results / deliverables and then ask for more. This does not include any security audit.

**<span style="text-decoration:underline;">Performance indexed bonus</span>**

The performance indexed bonus will be requested after full delivery when the solution is up and running and **only if the total revenue generated from the community DEX (transaction fees plus profit from internal arbitrage) is greater than​​ 1,600,000,000 LUNC before the 1st June 2023**.

If this happens we will submit a spending proposal against the community pool asking for **825,000,000 LUNC **(125,000 USD).



* Performance indexed bonus: **825,000,000 LUNC**
* Conditions:
    * Total transaction fees generated by the community DEX and profits from internal arbitrage **are greater than 1,600,000,000 LUNC** **before** **the 1st June 2023**
    * Submitted as a _Community Spend Proposal_ **after** the target has been reached
    * _Community Spend Proposal_ must be submitted **before** the 1st June 2023

**<span style="text-decoration:underline;">Deliverables</span>**



* One cosmos module (eg. a new liquidity keeper) for the liquidity core module including a Swap function and Admin functions to add/remove liquidity from/to the Oracle pool
* Integrated market maker triggered by the BeginBlock/EndBlock ABCI message at a given interval
* Integrated arbitrage bot triggered by the BeginBlock/EndBlock ABCI message at a given interval
* We haven’t decided yet whether or not Terra Indices will be implemented as a dedicated module or part of the liquidity module. This will be clarified before we ask for funding for Flash Swap and Terra Indices
* Wiring of the liquidity core module swap functionality in Terra Station
* New Price feeds in the Oracle for the market making strategies
* Full testing, simulation and back testing for risk management and P&L estimation of the market making and arbitrage strategies. This is called _Model Validation_, it is an internal project of its own since we need to develop the tools allowing us to test, visualize and optimize what we are doing. For that we will use python, that’s the standard in the industry, this will allow us to publish testing reports containing in depth analysis. This is part of an open source project so other teams can use it if they wish to: [https://cpy-amm.readthedocs.io/en/latest/readme.html](https://cpy-amm.readthedocs.io/en/latest/readme.html)

**<span style="text-decoration:underline;">About our team</span>**

We are a team of three professionals with extensive experience in developing cutting-edge trading systems and algo trading strategies (market making, statistical arbitrage and execution) in both the Financial and Crypto industry.

Together, our team has more than 30 years' experience in financial services and technology. We have worked at top investment banks and hedge funds, as well as at Big Tech companies and startups in the blockchain industry. Our expertise includes programming (Golang, Python, Rust), Netops/DevOps (CCIE, CKA/CKAD, AWS SA), machine learning (tensorflow), digital assets and blockchain (DEFi, Cosmos, Bitcoin, Ethereum).

We’re passionate about crypto, decentralized finance and how, combined with on-chain governance, it empowers people and allows everyone to build and participate in the next generation of finance.


# FAQ

**_What is a DEX ? _**

A decentralized exchange (DEX) is a peer-to-peer (P2P) marketplace that connects cryptocurrency buyers and sellers. For example, The swap mechanism designed to maintain the PEG of USTC is based on a DEX.

UNISWAP v2 DEX: [https://cpy-amm.readthedocs.io/en/latest/examples/uniswap_v2.html](https://cpy-amm.readthedocs.io/en/latest/examples/uniswap_v2.html)

Terra Market Module: [https://cpy-amm.readthedocs.io/en/latest/examples/terra_market_module.html](https://cpy-amm.readthedocs.io/en/latest/examples/terra_market_module.html)

**_Why are you getting paid to do a DEX ? Aren’t you going to sell tokens ?_**

This is a core module, transaction fees are paid to the oracle/community pool, and used to burn the LUNC supply. **This is not a dApp. We won’t be selling any tokens.** We’re getting paid to develop the module and the tools to test it.

This DEX will belong to the community and benefit it by paying stacking rewards, burning the supply down and funding the revitalization effort.

**_Are you competing with other teams to become the main core developers ?_**

**Absolutely not. **This is the community’s passport to financial freedom. It is designed to generate revenue at the protocol level so the blockchain can become sustainable and community assets can be used to pay for the costs of maintaining it and to attract builders.

It will encourage all the actors contributing to the development of the chain to be paid via governance and therefore to utilize it: **It empowers the community.**

**_Are you going to deplete the Oracle pool ? _**

**No**. We are simply **borrowing **tokens from the Oracle Pool to generate revenue in order to refill it.

The protocol is composed of multiple modules and multiple on-chain "bank accounts". We’re proposing to add a new module which will borrow funds internally against a yield coming from transaction fees. This is **not** a smart contract or a dApp. It does **not** belong to us. We **don’t** and we **can’t** interact with the funds.

**_How much will the liquidity module borrow ? _**

This is a parameter which will be decided by governance (param change proposal). We obviously recommend to start with a small amount and to increase gradually. It takes 7 days to change this parameter.

We will provide simulations and data analysis to help choose the right amount as well as tools for people to do their own analysis if they wish to.

**_Are there any risks of losing the money borrowed from the Oracle pool ? _**

The higher the correlation between the assets in a trading pair, the lower the IL risk. By choosing to provide  liquidity in pairs such as LUNC/USTC, LUNC/LUNA or LUNA/USTC we are naturally hedged against this risk. Furthermore, we will implement a UNISWAP v3 like concentrated liquidity feature so we can manage the risk associated with providing liquidity.

**_Are you proposing to merge LUNC and LUNA ?_**

**Absolutely not.** We are proposing to make an index composed of these two tokens because it makes sense financially for the Terra Classic protocol to do so. This will increase our trading volume, it will increase the amount of LUNC burned and will help refill the Oracle / Community pools.

We are also planning to have an index with LUNC and ATOM. **It does not mean we are planning to merge LUNC and ATOM.**

**_Why start with (LUNC, LUNA) as a first index ?_**

First of all we don’t decide which indices will be available. **The community will decide via governance.** We think it makes sense to start with a "cross Terra blockchain" index so that investors will be able to diversify their exposure to Terra Classic or Luna v2 on-chain. Hence we hope it will attract more trading volume. Remember, from our point of view, all we want is to maximize the trading fees and the LUNC burn.

Besides, everytime you mint/redeem an index you burn LUNC, and indices must always include LUNC which means the circulating supply will also be reduced. Finally, to mint an index you can only pay with USTC which we hope will create a nice buying pressure.

In this way, we are effectively favoring Terra Classic.

**_What if people don’t pay back the flash loans ?_**

**To be clear flash loans will only be available internally for the protocol. They won’t be available to users. **Think of it as you transferring money between your two wallets, a wallet you use to hold and a wallet you use to trade; You see an arb opportunity, then:



1. You transfer LUNC from your holding wallet to your trading wallet
2. You take the arb and make a profit
3. You transfer back LUNC + profit from your trading account to your holding wallet

**The three steps above cannot happen if the arb results in a loss. They’re automatically canceled. **That’s the beauty of flash swaps, everything happens in the same transaction, the protocol either earns money or everything is canceled and it doesn't lose anything. **The money remains within the protocol at any time and the arbitrage is risk free.**

**_Why won’t the protocol lose the transaction fee when it will flash swap to arb ?_**

This will be implemented internally in a way that we don’t have to pay the transaction fee. Nice advantage of working with a core module and virtual pools rather than with a dApp and smart contracts.

**_How can we trust you  ?_**

You don’t have to. This proposal and the open source tools available show that we have already spent time reverse engineering the platform to understand how it works and how we can use it to make it sustainable and support the revitalization effort. We hope to see some healthy discussion around the ideas presented and we’ll be available to help inform and provide further explanations so you can make the best decision.

Furthermore, funding is split in different tranches and our remuneration is tied to the performance of the delivered solution which means that not only we’re incentivized to deliver, but we’re incentivized to deliver the solution which maximizes your profit.
