---
layout: post
title:  Bitcoin apps
description: Unlike younger counterparts like Ethereum, Bitcoin wasn't designed with programmability in mind. The absence of smart contracts in Bitcoin's architecture fundamentally alters the approach to app development. In this landscape, simplicity is king, and limitations are plentiful. However, it's within these constraints that creativity and ingenuity are truly tested...
date:   2024-01-01 01:01:01 +0000
tags:   [bitcoin,nostr]
---
### Introduction

In the ever-evolving landscape of cryptocurrency, Bitcoin stands as the pioneering digital currency. Its breakthrough has spawned a myriad of cryptocurrencies and smart contract/blockchain platforms, yet developers venturing into the realm of Bitcoin apps face distinct challenges.

Unlike younger counterparts like Ethereum, Bitcoin wasn't designed with programmability in mind. The absence of smart contracts in Bitcoin's architecture fundamentally alters the approach to app development. In this landscape, simplicity is king, and limitations are plentiful. However, it's within these constraints that creativity and ingenuity are truly tested.

As a decentralized digital currency, Bitcoin operates with a very simplified set of computing capabilities, enabled by the Bitcoin Script programming language. This language is not Turing complete, meaning it doesn't have the ability to perform all possible computation tasks. In practical terms, Bitcoin Script is designed for processing simple transactions and cannot execute complex operations or loops, which are possible in Turing-complete languages like those used for Ethereum's smart contracts. This limitation is by design, prioritizing security and predictability in Bitcoin transactions.

Moreover, Bitcoin lacks any form of cloud services and cloud infrastructure, as it is focused on the single task of securing Bitcoin transfers. Fortunately, there are already existing supporting technologies that complement its capabilities. In this book, we will explore several of these technologies. As an example, we will present NOSTR, a decentralized relay protocol for openly publishing and consuming data. NOSTR stands out for its simplicity and its ability to facilitate decentralized social interactions and information exchange, complementing Bitcoin's infrastructure by enabling a wider range of applications beyond basic transactions.

Another bitcoin-centric technology that makes a significant difference is the Lightning Network. Bitcoin has a limitation of 1 MB per block approximately every 10 minutes, which hampers its usability for micropayments due to capacity and speed constraints. The Lightning Network, a layer-2 technology built on top of the Bitcoin network, addresses this limitation. It enables transactions at a throughput comparable to conventional payment systems like Visa or Mastercard, thus making micropayments feasible on Bitcoin's infrastructure. This innovation significantly expands the potential applications of Bitcoin, allowing it to handle a much higher volume of transactions and smaller payment amounts efficiently.

## Bitcoin vs Crypto
If you are a crypto developer from Solana or ETH used to work with Solidity etc this can be a strange for you that someone is tryign to write a book about programming apps on Bitcoin. Bitcoin is like a old fashioned stuff that do not have much novelity. It lacks Smart contracts and moreove do not allow for zk rolups therefore seems  not to scale with the demand. Moreover Bitcoin maxis seems loking lika a church with anonymous mistical Messaiah (satoshi), Bible (Bitcoin Standard) and school of thought (Austrian Economics). Crypto space areound ETH or solana seems to be on the a vibrant space where any new idea is welcomed and projects are coloroful and look like an el Dorado. Crypto space crated web3 movement that was about to surpas web2 with decentralisation, calling for distributed orgranisations (DAOs) managed by anonymous stakeholders etc with its Techonomy etc.

Bitcoin seems to be mundane. There is no fun there. Even BRC20 - that was quite suprising sideeffect of taproot softfork and allowed for Bitcoin Oridinals and Inscriptions is seen from the Bitcoin community as a bug that should be fixed. Bitcoin network has a single purpose - move Bitconis. Movement of bitcoins is limited to 1MB/10m transfer rate (aprox 1.7 kB/s). Avarage 4000 transactions per block gives us 6.7 transactions per second (2-3 before segwit, 7-10 on segwit) - which is very slow  designed from very begining and it do not look like it is impossible to change what so called block-size wars already proven. Slow, mundane network that is almost imposibble to innovate - but on the other side - solid rock foundation of having 21 milion Bitcoins only as a limit of geometric serie reducing Bitcoin inflation every time new 210k block are mined.

|System | transactions/s| number of nodes (2023)|architecturee| cost a node|
|---|---|---|---|---|
|Bitcoin Segwit| 7 |17300| P2P POW| $250 |
|Bitcoion on Lightning Network| >1000000 (theoretical)| 16500|P2P| $250|
|Ethereum| 20 | 7460 | P2P POS| $4000 (x4 for archive)|
|Solana| 2500 | 2040 | Hybrid P2P + Cloud|$18738|
|Pay pal|193| - | Centralized|-
|Visa|24000| - |Centralized|-
|Mastercard|5000| - | Centralized|-


## Bitcoin and Decentralised Apps Development

The main promise of Crypto space can be undestand as a decentralisation. Maintaing blockchain makes sense only if you want to get rid of trusted cenralised organasations and manage a way allows participants objective veryfication of the state of the system without the need trusting anyone. We will be speaking here about decentralised apps. Centralised apps are already covered in many other books describing how to build the scalable centralised architecture e.g. in the cloud.

Web 3 is all about decentralisation. Bitcoin however due to its limitations requires impacts any decentralised app architecture by enforcing specific separation of responsibility. Bitcoin secures the monetary side of the system and all the rest needs to be done offchain. Offchain computing touches the bitcoin network only to stick to the hard bitcoin assets. Naturally one of the touching point are bitcoin transactions that in their complex form of HTLC are used as an enabling technology for Lightnig Network. Lightning Network allows creating invoices and making payment in the onion-routed way. There are two missing pieces, however that would enable building the app: 1. Communication infrasdtracture, 2. Database, 3. Oracles
Here we will see how Nostr is solving both 1 and 2 to some extend these issues to some extend by enabling kind of P2P, general purpose infrastructure.

