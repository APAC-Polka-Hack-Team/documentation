# Summary

Broadly, we’ve identified four different areas to work on in order to produce a minimum proof-of-concept. If you're interested in learning more or contributing to any of these areas, there will be a lead contact (discord handle) listed for each section.

# Links

Currently our project is working off a fork of Phala's phala-blockchain and js-sdk repos. By convention, the APAC-Polka-Hack-Team branch serves as our "master" branch for our code work. For quick reference:

This is where all the smart contract examples are stored on the phala-blockchain repo. The darkpool smart contract file specifically is darkpool.rs:

https://github.com/APAC-Polka-Hack-Team/phala-blockchain/tree/APAC-Polka-Hack-Team/crates/phactory/src/contracts

This is where the front-end logic is stored. The darkpool frontend file specifically is darkpool.tsx:

https://github.com/APAC-Polka-Hack-Team/js-sdk/tree/APAC-Polka-Hack-Team/packages/example/pages

# 1. Order Matching Logic

Written in Rust using Phala’s smart contract format, this includes the definitions for the order data structures, basic logic for adding, updating, and canceling orders, as well as matching buy and sell orders. The code logic should follow the format described in the README under section “Darkpool DEX: Business Logic”.

Future enhancements would include features such as adding in user-specified time limits on live orders to expire automatically, user-specified maximum/minimum price limits they are willing to be matched on, user-specified toggles for how much/little privacy they want on their specific order, and potentially queueing logic such as pushing large orders back to the end of the queue after X amount of their order has been filled to prevent large whale orders from clogging the queue.

**Lead contact: @tsuigeo (Geo)**



# 2. Reference Mid Pricing

Written in Rust, taking advantage of Phala’s feature of smart contracts running off-chain to access HTTP services natively, we can retrieve the best bid/offer prices at various CEXes via their API. This can be done via Phala’s so-called **AsyncSideTask**. More information on this can be found here: https://wiki.phala.network/en-us/docs/developer/your-first-confidential-contract/#advanced-feature-access-http-service-in-confidential-contracts

After retrieving the best bid/offer, the smart contract should calculate a reference **“mid price”** which theoretically represents the current true fair value of the market. Every time a new order is inserted into the trading engine, the AsyncSideTask should be called to update the mid price to the latest current price, and use it as the executed price if any orders are matched at this time.

Future enhancements would include aggregating prices from multiple CEXes/DEXes to make the pricing more robust, adding clever throttling or filtering to the mid price to account for potential manipulation by high-frequency traders attempting to influence the mid pricing, and caching the last good mid price as a smart contract storage variable (e.g. good for 200 milliseconds) for scalability purposes.

**Lead contact: @ftameemi (Faisal)**



# 3. Transfer Of Coins Post-Trade

Written in Rust, this section of the code handles the physical transfer of coins between users after a trade has been executed. Since the first iteration of markets on the darkpool exchange will likely be the native tokens of various parachains on the Polkadot network (For example, buying DOT and selling ACA) that are on different chains altogether, the smart contract will likely need to use Polkadot’s XCMP, a crosschain messaging format, to handle this. More information on XCMP here:

https://github.com/paritytech/xcm-format

https://wiki.polkadot.network/docs/learn-crosschain

https://polkadot.network/blog/xcm-the-cross-consensus-message-format/

https://polkadot.network/blog/xcm-part-two-versioning-and-compatibility/

https://polkadot.network/blog/xcm-part-three-execution-and-error-management/

Since XCMP is relatively new and not well-documented yet, it may be difficult to fully move forward on this at the moment. For the purposes of the proof-of-concept, it should be acceptable instead to write Phala smart contracts for various artificial ERC20 tokens to stand in as wrapped tokens living inside the Phala chain. For demonstration purposes then, token transfers will be much more simple this way. A good template for writing ERC20 smart contracts in rust can be found on the substrate tutorials page:

https://docs.substrate.io/tutorials/v3/ink-workshop/pt3/

**Lead contact: @claudio**




# 4. Frontend UI

The first iteration of the frontend UI will likely utilize Phala’s js-sdk package to interact with the smart contract in the backend. More information on Phala js-sdk here:

https://github.com/Phala-Network/js-sdk/tree/main/packages/sdk

Phala has several examples of very basic frontend to communicate with the smart contract written in Typescript / React, located here:

https://github.com/APAC-Polka-Hack-Team/js-sdk/tree/APAC-Polka-Hack-Team/packages/example/pages

As the project progresses, the frontend UI will become more and more important. A full web app will need to be designed and fleshed out to maximize user experience, so full-stack web app development and website design skills will be very useful here.

Lead contact: ?? None yet. Please let us know in the discord channel if you want to take the lead on this!!
