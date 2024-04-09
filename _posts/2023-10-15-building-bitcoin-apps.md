---
layout: post
title:  Building Bitcoin Mobile Apps
description: Unlike younger counterparts like Ethereum, Bitcoin wasn't designed with programmability in mind. The absence of smart contracts in Bitcoin's architecture fundamentally alters the approach to app development. In this landscape, simplicity reigns supreme, and limitations abound. Yet, it is within these constraints that creativity and ingenuity are truly tested, and mobile apps find their place...
date:   2023-10-15 11:02:01 +11:00
tags:   [bitcoin,lightning network,nostr,apps]
---

### Introduction

In the ever-evolving landscape of cryptocurrency, Bitcoin stands as the pioneering digital currency. Its breakthrough has spawned a myriad of cryptocurrencies and smart contract/blockchain platforms. Yet, developers venturing into the realm of Bitcoin apps face distinct challenges.

Unlike younger counterparts like Ethereum, Bitcoin wasn't designed with programmability in mind. The absence of smart contracts in Bitcoin's architecture fundamentally alters the approach to app development. In this landscape, simplicity is king, and limitations are plentiful. However, it's within these constraints that creativity and ingenuity are truly tested.

As a decentralized digital currency, Bitcoin operates with a very simplified set of computing capabilities, enabled by the Bitcoin Script programming language. This language is not Turing complete, meaning it doesn't have the ability to perform all possible computation tasks. In practical terms, Bitcoin Script is designed for processing simple transactions and cannot execute complex operations or loops, which are possible in Turing-complete languages like those used for Ethereum's smart contracts. This limitation is by design, prioritizing security and predictability in Bitcoin transactions.

Moreover, Bitcoin lacks any form of cloud services and infrastructure, as it is focused on the single task of securing Bitcoin transfers. Fortunately, there are existing supporting technologies that complement its capabilities. In this post, we will explore several of these technologies. As an example, we present NOSTR, a decentralized relay protocol for openly publishing and consuming data. NOSTR stands out for its simplicity and its ability to facilitate decentralized social interactions and information exchange, complementing Bitcoin's infrastructure by enabling a wider range of applications beyond basic transactions.

Another Bitcoin-centric technology that makes a significant difference is the Lightning Network. Bitcoin has a limitation of 1 MB per block approximately every 10 minutes, which hampers its usability for micropayments due to capacity and speed constraints. The Lightning Network, a layer-2 technology built on top of the Bitcoin network, addresses this limitation. It enables transactions at a throughput comparable to conventional payment systems like Visa or Mastercard, thus making micropayments feasible on Bitcoin's infrastructure. This innovation significantly expands the potential applications of Bitcoin, allowing it to handle a much higher volume of transactions and smaller payment amounts efficiently.

## Bitcoin vs. Crypto

If you are a crypto developer from Solana or Ethereum used to working with Solidity, etc., it might seem strange to write about programming apps on Bitcoin. Bitcoin is perceived as old-fashioned, lacking in novelty. It lacks smart contracts and does not allow for zk-rollups, therefore seeming not to scale with demand. Moreover, Bitcoin maximalists seem like a church with an anonymous mystical Messiah (Satoshi), a Bible (The Bitcoin Standard), and a school of thought (Austrian Economics). The crypto space around Ethereum or Solana appears to be a vibrant space where any new idea is welcomed, and projects are colorful, looking like an El Dorado. The crypto space created the web3 movement, aiming to surpass web2 with decentralization, advocating for distributed organizations (DAOs) managed by anonymous stakeholders, etc., with its techonomy.

Bitcoin seems to be mundane. There is no fun there. Even BRC20 - a quite surprising side effect of the Taproot soft fork, which allowed for Bitcoin Ordinals and Inscriptions - is seen by the Bitcoin community as a bug that should be fixed. The Bitcoin network has a single purpose: to move Bitcoins. Movement of bitcoins is limited to a 1MB/10m transfer rate (approx 1.7 kB/s). An average of 4000 transactions per block gives us 6.7 transactions per second (2-3 before SegWit, 7-10 with SegWit) - which is very slow, designed from the beginning, and it does not seem like it is possible to change, as the so-called block-size wars have already proven. Slow, mundane network that is almost impossible to innovate 

- but on the other side - a solid rock foundation of having only 21 million Bitcoins as a limit of a geometric series reducing Bitcoin inflation every time new 210k blocks are mined.

**I am sure this is the kind of foundation you, as a Solidity/Solana developer, might be missing, and it's something you can only find with Bitcoin:**
1. 21 million.
2. It was used as monopoly money for so long that it became fully distributed and not only decentralized.
3. Purely game-theoretic from the economic point of view.

Moreover;

|System | transactions per second| number of (named) nodes (2023)|architecture| cost of single node (USD)|
|---|---|---|---|---|
|Bitcoin Segwit| 7 |17300| P2P POW| $250 |
|Bitcoion on Lightning Network| >1000000 (theoretical)| 16500|P2P| $250|
|Ethereum| 20 | 7460 | P2P POS| $4000 (x4 for archive)|
|Solana| 2500 | 2040 | Hybrid P2P + Cloud|$18738|
|Pay pal|193| - | Centralized|-|
|Visa|24000| - |Centralized|-|
|Mastercard|5000| - | Centralized|-|

## Bitcoin and Decentralized Apps Development

The essence of the crypto space can be understood as decentralization. Maintaining a blockchain makes sense only if the goal is to eliminate reliance on trusted, centralized organizations, enabling a system where participants can objectively verify the state of the system without needing to trust anyone. Here, we will discuss decentralized apps (dApps). Centralized apps have been extensively covered elsewhere, focusing on how to build scalable, centralized architectures, such as those in cloud computing.


Web 3.0 is synonymous with decentralization. However, due to its inherent limitations, Bitcoin introduces unique challenges to decentralized app architecture by enforcing a specific separation of responsibilities. Bitcoin secures the monetary aspect of the system, requiring all other functionalities to be handled off-chain. Off-chain computing interacts with the Bitcoin network only in the context of securing Bitcoin assets. A primary interaction point involves Bitcoin transactions, which, in their complex form as Hash Time-Locked Contracts (HTLCs), serve as the enabling technology for the Lightning Network. The Lightning Network facilitates the creation of invoices and payments in an onion-routed manner, significantly enhancing Bitcoin's scalability and utility for microtransactions. However, there are a few missing components necessary for comprehensive app development: Communication infrastructure, Database, 
Oracles, Regulatory Compliance and Authentication/Authorisation.

<div class="gallery-box">
    <div class="mermaid col-12">
flowchart TB
    MA((Mobile App)) --> PS(Payment System)
    MA --> CI(Communication)
    MA --> DB[(Database)]
    MA --> OR(Oracles)
    MA --> CM(Regulatory \nCompliance)
    MA --> AU(Authentication \n & Authorisation)
    </div>
</div>

Moreover; certain design principles come into play:

1. **Symmetry**: Each app instance operates the same way as the others, which mirrors the peer-to-peer principle of Bitcoin.

2. **Permissionlessness**: Anyone with an internet connection can join in. No one is gatekept, reflecting the permisionless nature of Bitcoin.

3. **Privacy**:  Communication among nodes or clients is encrypted, enhancing user trust by protecting sensitive transactional data.

4. **Anonymity**: The identity of people behind transactions is secret.

5. **Sustainability**: The application's features should promote honest participation, with built-in mechanisms that inherently disadvantage dishonest actors. This "implicit punishment principle", taken from Bitcoin, ensures the long-term health of the app ecosystem.

6. **Compliance Consistency**: Staying within legal boundaries is critical, even in decentralized spaces. The app should be developed to comply with laws and regulations over time.


## Communication Infrastructure in Mobile Applications

Mobile applications today are crafted with a variety of communication infrastructures to meet diverse user needs. These range from facilitating personal and professional interactions to enabling transactions and fostering community engagement. In the context of Bitcoin Apps and similar decentralized applications, there's a distinct absence of a central communication platform, prompting a reliance on Peer-to-Peer (P2P) communication methods. This scenario unfolds in two primary ways: through mobile mesh networks and using a relay map.

### Mobile Mesh Networks

A mobile mesh network leverages a network of mobile devices interconnected through wireless communication technologies like Wi-Fi, Bluetooth, or cellular data. Each device within this network acts as a node, relaying data to and from other nodes. This structure creates a decentralized and self-organizing network, where data can traverse across various devices to reach its destination. Technologies such as WebRTC or Peer from Holepunch.io exemplify the application of mobile mesh networks in enabling direct, device-to-device communication without the need for a centralized server.

**Example of a Mobile Mesh Network:**


<div class="gallery-box">
    <div class="mermaid col-12">
graph LR
    U1(User1) --- U2(User2)
    U2 --- U3(User3)
    U3 --- U4(User4)
    U4 --- U5(User5)
    U5 --- U6(User6)
    U6 --- U7(User7)
    U7 --- U1(User1)
    U2 --- U4
    U4 --- U6
    U7 --- U3
    U5 --- U1
    </div>
</div>

- User1 connects directly to User2, User3 to User4, and so on, forming a mesh where each user can directly or indirectly communicate with any other user in the network.
- Data can route through multiple devices, enabling communication even in challenging environments where direct connections might not be possible.

### Relay Map

The concept of a relay map introduces an implicit network built atop redundant connections between users and multiple relays. In this model, relays function as nodes that users connect to but are not directly interconnected with each other. Users can publish events to or subscribe to events from one or more relays. When an event is published to a specific relay, it gets distributed to all other users connected to that relay, facilitating a distributed network topology as more users and relays join the network. The Nostr protocol is a prime example of implementing P2P communication through encrypted messages using a relay map approach.

Moreover, relays play a critical role beyond just data transmission; they act as micro-servers, enabling the implementation of essential features such as push notifications. Push notifications are a pivotal component in the realm of mobile app communication, especially considering that mobile apps can frequently operate in a background mode. This mode significantly restricts many of the app's advanced functionalities, including its ability to maintain internet connectivity. By leveraging push notifications, developers can implement a wake-up mechanism that activates the app from its background state to the foreground, ensuring that users remain informed and engaged with timely and relevant updates. This functionality is vital for maintaining user interaction and ensuring that the app remains responsive to real-time events, even when it is not actively in use.

**Example of a Relay Map:**
<div class="gallery-box">
    <div class="mermaid col-12">
graph BT
    NSTR1(Relay1) --- U1(User1)
    NSTR1(Relay1) --- U2(User2)
    NSTR1(Relay1) --- U3(User3)
    NSTR1(Relay1) --- U4(User4)
    NSTR1(Relay1) --- U5(User5)
    NSTR2(Relay2) --- U3(User3)
    NSTR2(Relay2) --- U4(User4)
    NSTR2(Relay2) --- U5(User5)
    NSTR3(Relay3) --- U4(User4)
    NSTR3(Relay3) --- U5(User5)
    NSTR3(Relay3) --- U6(User6)
    NSTR3(Relay3) --- U7(User7)
    </div>
</div>

- Users connect to one or more relays (e.g., User3 is connected to Relay1 and Relay2).
- Events published by a user to their connected relay(s) are distributed to all other users connected to those same relays.
- This creates a scalable and flexible network topology, enabling efficient distribution of information without requiring direct user-to-user connections.

Both mobile mesh networks and relay maps are pivotal in the development of decentralized communication protocols, offering robust and resilient alternatives to traditional, centralized communication infrastructures. These technologies not only enhance privacy and security but also ensure that communication remains uninterrupted even in the absence of central servers, making them ideal for applications that prioritize decentralization and user autonomy.

## Databases for Mobile Apps

Every mobile app necessitates a repository for its data. Surprisingly, upon reflection, it becomes evident that the majority of this data can, and arguably should, be owned directly by the user. It's entirely feasible to design and develop fully functional apps that store all pertinent information locally on the user's device. Such an approach significantly curtails the access central entities have to personal information, thereby safeguarding user privacy. With the advent of data encryption via the user's private key, it's possible to back up data in a redundant manner across multiple, cost-effective cloud providers. Importantly, these providers would not have direct access to the content of the data. This concept is in harmony with the philosophy of Bitcoin, promoting a decentralized and user-empowered approach to data storage and privacy.

## Oracles for Mobile Apps

Mobile applications often depend on external services to furnish real-time, reliable information reflective of the global state. These services, known as oracles, are crucial for applications that require up-to-date information beyond the scope of their internal data or need computations that are too resource-intensive to be efficiently handled on a mobile device. Oracles play a pivotal role in ensuring that mobile apps have access to accurate, maintained information and can include a wide variety of data sources, such as:

| **Oracle Type**                 | **Description**                                                                                                                                                                                                                           | **Application Examples**                            |
|---------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| **Heavy Computing Calculations**| Tasks requiring significant computational resources, like route optimizations or complex financial modeling.                                                                                                         | Logistics and travel apps, Health apps, Fintech apps|
| **Current Maps**                | Real-time updates on navigation including traffic conditions, construction sites, and newly opened routes.                                                                                                                               | Navigation apps                                     |
| **Weather Forecast**            | Accurate, up-to-the-minute weather data for a variety of applications.                                                                                                                                                                   | Travel planning apps, Agricultural apps             |
| **Market Indicators**           | Real-time financial data for monitoring stock prices, cryptocurrency values, and other investment indicators.                                                                                                                            | Stock trading apps, Cryptocurrency wallets          |
| **Sport Scores and Events**     | Live updates on scores, schedules, and player statistics.                                                                                                                                                                                | Sports betting apps, Fan engagement platforms       |
| **Flight Data**                 | Real-time information on flight schedules, delays, and cancellations.                                                                                                                                                                    | Travel and logistics applications                   |
| **Public Transport Timetables** | Updated schedules and delays for various public transport services.                                                                                                                                                                      | Public transport apps                               |
| **Environmental Data**          | Information related to air quality, pollution levels, and the UV index.                                                                                                                                                                  | Health and fitness apps                             |
| **Currency Exchange Rates**     | Up-to-date exchange rates for converting prices and transactions between different currencies.                                                                                                                                           | Travel apps, E-commerce apps, Finance apps          |


## Regulatory Compliance

Even when developing applications that offer users sovereignty, privacy, and anonymous features, it's crucial to recognize that these applications still operate within a specific legislative framework. This framework is governed by regulatory bodies at both local government and state levels. To ensure safety and legality for both your platform and its users, compliance with these regulations is essential. An illustrative example can be seen in the context of Bitcoin transactions. In many jurisdictions, if Bitcoin is permitted as a form of payment, it often comes with stringent requirements for Know Your Customer (KYC) protocols and the tracing of transactions for purposes such as taxation. Generally, there are at least two critical regulatory requirements that your applications might need to address: KYC and ODR [Wikipedia - Legality of Bitcoin by country or territory](https://en.wikipedia.org/wiki/Legality_of_cryptocurrency_by_country_or_territory)

### KYC (Know Your Customer)

KYC refers to the process of verifying the identity of your clients or customers. The primary purpose of KYC regulations is to prevent identity theft, financial fraud, money laundering, and terrorist financing. In the context of financial applications, including those involving cryptocurrencies like Bitcoin, KYC procedures involve collecting and verifying personal information from users. This could include requiring users to submit government-issued IDs, proof of address, and other personal data. The aim is to ensure that businesses know who they are dealing with, thereby reducing the risk of criminal activities.

### ODR (Online Dispute Resolution)

ODR is a digital approach to resolving disputes between parties over the internet. It encompasses a wide range of technologies and methods designed to facilitate the resolution of disputes without the need for physical presence in courtrooms. ODR is particularly relevant for e-commerce platforms, online marketplaces, and service providers where transactions occur digitally, and parties may be in different geographical locations. Implementing ODR mechanisms allows businesses to offer their users efficient and accessible means of resolving disputes, which can enhance trust and user satisfaction. It aligns with regulatory frameworks that emphasize consumer protection and the need for fair and transparent dispute resolution processes.


## Authentication & Authorization

In the realm of user experience (UX), moving away from traditional authentication methods like Google or Facebook sign-ins to more decentralized forms can initially surprise users accustomed to relying on such services. Traditionally, these authentication methods utilize OAuth protocols, allowing users to sign in to various services and applications using their existing social media or Google accounts. This process simplifies logins, as users don't need to remember multiple passwords, and it leverages the security infrastructure of these large platforms.

In contrast, as Bitcoin enthusiasts, we adopt a different approach to authentication, grounded in the principles of elliptic curve cryptography. This method involves the use of a pair of keys: a public key that can be shared with others, and a private key that remains confidential. This cryptographic approach is foundational to Bitcoin and many other blockchain technologies, providing a secure and decentralized mechanism for authentication.

### Elliptic Curve Cryptography and Mnemonics

Elliptic Curve Cryptography (ECC) is used within the Bitcoin protocol to generate cryptographic keys. Users can manage their identity through something known as a "mnemonic phrase" or "seed phrase." By securing this mnemonic in the same manner one would secure their Bitcoins, typically on a hardware wallet, users ensure the safety of their authentication mechanism.

## Appendix - How the mobile apps are constucted

| **Category**                      | **Description**                                                                                                                                                                                                 | **Actors**                          | **Communication**                                                                                                                                 | **Examples**                          | **Data Storage**                        | **Oracles**                                     |
|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------|------------------------------------------|-------------------------------------------------|
| **Chats**                         | Enable personal and professional communication with features like voice/video calls, file sharing, and integration with other services.                                                                  | Individuals, Groups                | Peer-to-peer (P2P), Group chats, Channels                                                                                                                | WhatsApp, Telegram, Slack            | User messages, Media files           | None                                            |
| **Teleconferencing**              | Enables virtual meetings and collaboration through audio and video conferencing tools, often integrating screen sharing, virtual whiteboards, and real-time document collaboration.                              | Teams, Business Professionals       | Group video or audio calls, Webinars, Virtual meeting rooms                                                                                              | Zoom, Microsoft Teams, Google Meet   | Meeting recordings, Shared files     | None                                            |
| **Social Media Platforms**        | Connect users worldwide to share life experiences, engage with businesses, and access personalized content.                                                                                                      | Individuals, Businesses, Public Figures | Walls, Direct Messages (DMs), Comments, Stories                                                                                                        | Facebook, Instagram, Twitter         | User-generated content, Profiles      | None                                            |
| **News Apps**                     | Transform news consumption with mobile access to breaking news, multimedia content, and interactive features like comments and social sharing.                                                                   | Readers, Journalists               | Comments sections under articles, Social sharing                                                                                                          | The New York Times, BBC News, Flipboard | News articles, Comments              | OpenWeatherMap (Weather forecasts)           |
| **Marketplaces**                  | Allow buying and selling products and require communication for transactions and inquiries.                                                                                                                     | Buyers, Sellers                     | Direct Messages (DMs), Public question and answer sections                                                                                                 | eBay, Amazon, Etsy                   | Product listings, Transactions        | None                                            |
| **Ride-Sharing and Delivery Services** | Apps for ride-sharing and delivery services allow communication between customers and drivers or delivery personnel for coordinating pickups, deliveries, and addressing issues or special instructions.        | Customers, Drivers/Delivery Personnel | Direct Messages (DMs), sometimes through an intermediary platform for privacy                                                                            | Uber, Lyft, DoorDash                 | Ride history, User locations          | OpenStreetMap (Maps), OpenTraffic (Traffic)  |
| **Gaming Apps**                   | Multiplayer gaming apps incorporate chat functionalities to enable players to strategize, compete, and socialize in real-time, enhancing the gaming experience and building online communities.                 | Players                             | In-game chat rooms, Voice chat, P2P messages                                                                                                             | Fortnite, PUBG Mobile, Discord       | Player profiles, Game states          | None                                            |
| **Real Estate and Housing Apps**  | Users can communicate with property owners, real estate agents, and other prospective buyers or renters for scheduling viewings, negotiating prices, or discussing property features and amenities.              | Buyers, Renters, Agents, Owners     | Direct Messages (DMs), Public forums for Q&A                                                                                                             | Zillow, Realtor.com, Trulia          | Property listings, User inquiries     | None                                            |
| **Food and Beverage Apps**        | Feature communication channels with chefs, restaurant owners, or other food enthusiasts for ordering food, booking tables, or participating in cooking and wine-tasting classes.                                 | Customers, Chefs, Restaurant Owners | Direct Messages (DMs), Comments on public pages                                                                                                          | Yelp, OpenTable, UberEats            | Menu items, Orders                    | None                                            |
| **Travel and Accommodation Apps** | Enable travelers to contact hosts, ask questions about listings, share travel experiences, and connect with locals or other travelers for tips and recommendations.                                              | Travelers, Hosts, Local Guides      | Direct Messages (DMs), Public reviews and Q&A sections                                                                                                    | Airbnb, Booking.com, TripAdvisor     | Booking details, User reviews         | OpenStreetMap (Maps), OpenWeatherMap (Weather)|
| **E-Learning Platforms**          | Feature communication tools allowing students to interact with teachers or mentors and collaborate with peers through forums, direct messages, and live video chats, facilitating a more interactive experience. | Students, Teachers, Mentors         | Direct Messages (DMs), Forums, Group chats, Video conferencing                                                                                            | Coursera, Udemy, Khan Academy        | Course content, Assignments           | None                                            |
| **Health and Fitness Apps**       | Connect users with personal trainers, nutritionists, or health coaches, and offer community features for sharing experiences, challenges, and tips, fostering a sense of support and motivation.                | Users, Coaches, Community Members  | Direct Messages (DMs), Community forums, Group challenges                                                                                                 | MyFitnessPal, Fitbit, Strava         | Health data, Workout logs             | None                                            |
| **Language Learning Apps**        | Incorporate social features that allow learners to practice with native speakers, join language exchange communities, or participate in discussion groups, enabling real-world practice and cultural exchange.   | Learners, Native Speakers          | Peer-to-peer exchanges, Community forums, Group chats                                                                                                     | Duolingo, Tandem, HelloTalk          | Learning materials, User progress     | None                                            |
| **Event and Ticketing Apps**      | Include features for users to communicate with event organizers, ask questions, request information, or chat with other attendees, improving the event experience and facilitating networking opportunities.     | Attendees, Organizers              | Direct Messages (DMs), Public forums, Event-specific chat rooms                                                                                           | Eventbrite                           | Event details, Tickets                | None                                            |
