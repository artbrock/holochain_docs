# FAQ

If you have more questions for our FAQ, please make a suggestion.

<!-- TOC START min:2 max:3 link:true update:true -->
  - [Why do you call it "Holochain?"](#why-do-you-call-it-holochain)
    - [A Unified Cryptographic WHOLE](#a-unified-cryptographic-whole)
    - [Holographic Storage](#holographic-storage)
    - [Holarchy](#holarchy)
  - [How is a Holochain different from a blockchain?](#how-is-a-holochain-different-from-a-blockchain)
  - [How is a Holochain different from a DHT (Distributed Hash Table)?](#how-is-a-holochain-different-from-a-dht-distributed-hash-table)
  - [What kind of projects is Holochain good for?](#what-kind-of-projects-is-holochain-good-for)
  - [What kind of projects is it NOT good for?](#what-kind-of-projects-is-it-not-good-for)
  - [Can you run a cryptocurrency on a Holochain?](#can-you-run-a-cryptocurrency-on-a-holochain)

<!-- TOC END -->

## Why do you call it "Holochain?"

### A Unified Cryptographic WHOLE
**Hashchains + Signing + DHT** --  multiple cryptographic technologies and composed them into a new unity.  
  1. **Independent Hashchains** provide immutable data integrity and definitive time sequence from the vantage point of each node. Truthfully, we're using hash trees -- blockchains do too, but they're not called blocktrees, so we're not calling these holotrees.
  2. **Signing** of chains, messages, and validation confirmations maintain authorship, provenance, and accountability. Countersigning of transactions/interactions between multiple parties provide non-repudiation and "locking" of chains.
  3. **DHT** (Distributed Hash Table) leverages cryptographic hashes for content addressable storage, randomization of interactions to discourage collusion, and performs validation of #1 and #2.

### Holographic Storage
Every node has a resilient sample of the whole. Like cutting a hologram, if you were to cut the network in half (make it so half the nodes were isolated from the other half), you would have two, whole, functioning systems, not two partial, broken systems.

This seems to be the strategy used to create resilience in natural systems. For example, where is your DNA stored? Every cell carries its own copy, with different functions expressed based on the role of that cell.

Where is the English language stored? Every speaker has a different version -- different areas of expertise, or exposure to different slang or specialized vocabularies. Nobody has a complete copy, nor is anyone's version exactly the same as anyone else,  If you disappeared half of the English speakers, it would not degrade the language much.

If you keep cutting a hologram smaller and smaller eventually the image degrades enough to stop being recognizable, and depending on the resiliency rules for DHT neighborhoods, holochains would likely share a similar fate. Although, if the process of killing off the nodes was not instantaneous, the network might be able to keep reshuffling data to keep it alive.

### Holarchy
Holochains are composable with each other into new levels of unification. In other words, holochains can build on decentralized capacities provided by other holochains, making new holistic patterns possible. Like bodies build new unity on holographic storage patterns that cells use for DNA, and a society build new unity on the holographic storage patterns of language, and so on.

*Share examples of how we're bootstrapping using a holochain of holochains, DPKI, neighborhood gossip systems, tagging system for locking top hashes, etc.*



## How is a Holochain different from a blockchain?
Long before blockchains were [hash chains](https://en.wikipedia.org/wiki/Hash_chain) and [hash trees](https://en.wikipedia.org/wiki/Merkle_tree). These structures ensure tamper-proof data integrity as progressive versions or additions to data are made. These kinds of hashes are often used as reference points to ensure data hasn't been messed with -- like making sure you're getting the program you meant to download, not some virus in its place.

Instead of trying to manage global consensus for every change to a huge blockchain ledger, every participant has [their own signed hash chain](https://medium.com/metacurrency-project/perspectives-on-blockchains-and-cryptocurrencies-7ef391605bd1#.kmous6d7z) ([countersigned for transactions](https://medium.com/metacurrency-project/beyond-blockchain-simple-scalable-cryptocurrencies-1eb7aebac6ae#.u1idviscz) involving others). After data is signed to local chains, it is shared to a [DHT](https://en.wikipedia.org/wiki/Distributed_hash_table) where every node runs the same validation rules (like blockchain nodes all run the [same validation rules](https://bitcoin.org/en/bitcoin-core/features/validation)). If someone breaks those rules, the DHT rejects their data -- their chain has forked away from the holochain.

The initial [Bitcoin white paper](https://bitcoin.org/bitcoin.pdf) introduced a blockchain as an architecture for decentralized production of a chain of digital currency transactions. This solved two problems (time/sequence of transactions, and randomizing who writes to the chain) with one main innovation of bundling transactions into blocks which somebody wins the prize of being able to commit to the chain if they [solve a busywork problem](https://en.bitcoin.it/wiki/Hashcash) faster than others.

Now Bitcoin and blockchain have pervaded people's consciousness and many perceive it as a solution for all sorts of decentralized applications. However, when the problems are framed slightly differently, there are much more efficient and elegant solutions (like holochains) which don't have the [processing bottlenecks](https://www.google.com/search?q=blockchain+bottleneck) of global consensus, storage requirements of everyone having a [FULL copy](https://blockchain.info/charts/blocks-size) of all the data, or [wasting so much electricity](https://blog.p2pfoundation.net/essay-of-the-day-bitcoin-mining-and-its-energy-footprint/2015/12/20) on busywork.

## How is a Holochain different from a DHT (Distributed Hash Table)?
DHTs enable key/value pair storage and retrieval across many machines. The only validation rules they have is the hash of the data itself to confirm what you're getting is probably what you intended to get. They have no other means to confirm authenticity, provenance, timelines, or integrity of data sources.

In fact, since many DHTs are used for illegal file sharing (Napster, Bittorrent, Sharezaa, etc.), they are designed to protect anonymity of uploaders so they won't get in trouble. File sharing DHTs frequently serve virus infected files, planted by uploaders trying to infect digital pirates. There's no accountability for actions or reliable way to ensure bad data doesn't spread.

By embedding validation rules as a condition for the propagation of data, our DHT keeps its data bound to signed source chains. This can provide similar consistency and rule enforcement as blockchain ledgers asynchronously so bottlenecks of immediate consensus become of the thing of the past.

The DHT leverages the signed source chains to ensure tamper-proof immutability of data, as well as cryptographic signatures to verify its origins and provenance.

The Holochain DHT also emulates aspects of a graph database by enabling people to connect links to other hashes in the DHT tagged with semantic markers. This helps solve the problem of finding the hashes that you want to retrieve from the DHT. For example, if I have the hash of your user identity, I could query it for links to blogs you've published to a holochain so that I can find them without knowing either the hash or the content. This is part of how we eliminate the need for tracking nodes that many DHTs rely on.

## What kind of projects is Holochain good for?
Sharing collaborative data without centralized control. Imagine a completely decentralized Wikipedia, DNS without root servers, or the ability to have fast reliable queries on a fully distributed PKI, etc.

* **Social Networks, Social Media & VRM:** You want to run a social network without a company like Facebook in the middle. You want to share, post, publish, or tweet to shared space, while automatically keeping a copy of these things on your own device.
* **Supply Chains & Open Value Networks:** You want to have information that crosses the boundaries of companies, organizations, countries, which is collaboratively shared and managed, but not under the central control of any one of those organizations.
* **Cooperatives and New Commons:** You want to create something which is truly held collectively and not by any particular individual. This is especially good for digital assets.
* **P2P Platforms:** Peer-to-Peer applications where every person has similar capabilities, access, responsibilities, and value is produced collectively.
* **Collective Intelligence:** Governance, decision-making frameworks, feedback systems, ratings, currencies, annotations, or work flow systems.
* **Collaborative Applications:** Chats, Discussion Boards, Scheduling Apps, Wikis, Documentation, etc.
* **Reputational, or Mutual Credit Cryptocurrencies:** Currencies where issuance can be accounted for by actions of peers (like ratings), or through double-entry accounting are well-suited for holochains. Fiat currencies where tokens are thought to exist independent of accountability by agents are more challenging to implement on holochains.


## When NOT to Use Holochain
You probably SHOULD NOT use holochain for:
* **Just for yourself:** You generally don't need distributed tools to just run something for yourself. The exception would be if you want to run a holochain to synchronize certain data across a bunch of your devices (phone, laptop, desktop, cloud server, etc.)
* **Anonymous / Secret / Private data:** Not only do we need to do a security audit of our encryption and permissions, but you're publishing to a shared DHT space, so unless you really know what you're doing, you should not assume data is private. Some time in the future, I'm sure some applications will add an anonymization layer (like TOR), but that is not native.
* **Large files:** Think of holochains more like a database than a file system. Nobody wants to be forced to load and host your big files on their devices just because their in the neighborhood of its hash. Use something like [IPFS](http://ipfs.io) if you want a decentralized file system.
* **Data positivist-oriented apps:** If you have built all of your application logic around the idea that data exists as an absolute truth, not as an assertion by an agent at a time, then you would need to rethink your whole approach before putting it in a Holochain app. This is why _most existing cryptocurrencies would need significant refactoring_ to move from blockchain to holochain, since they are organized around managing the existence of cryptographic tokens.

## What is Holochain's Consensus Algorithm?
In making holochain, our goal is to keep it as simple as possible, but no simpler for providing data integrity for fully distributed applications. As we understand it, information integrity does not require consensus about an absolute order of events. You know how we know? Because it works in the real world --meaning the physically distributed systems outside of computers). Atoms, molecules, cells, bodies each maintain their own state just fine without consensus or a global ledger.  

Not only is there no consensus about an absolute order of events, but if you understand the General Theory of Relativity, then you'll understand there is in fact no real sequence of events, only sequence relative to a particular vantage point.

That's how holochains are implemented. Each source chain for each person/agent/participant in a Holochain preserves the immutable data integrity and order of events of that agent's actions from their vantage point. As data is published from a source chain to the validating DHT, then other agents sign their validation, per the shared "physics" encoded into the DNA of that Holochain.

The minor exception to the singular vantage point of each chain, is the case when a multi-party transaction is signed to each party's chain. That is an act of consensus -- but consensus on a very small scale -- just between the parties involved in the transaction. Each party signs the exact same transaction to with links to each of their previous chain entries. Luckily, it's pretty easy to reach consensus between 2 or 3 parties. In fact, that is already why they're doing a transaction together, because they all agree to it.

Holochains do sign every change of data and timestamp (without a universal time synchronization solution), This provides ample foundation for most applications which need solid data integrity for shared data in a fully distributed multi-agent system. Surely, there will be people who will build consensus algorithms on top of foundation (maybe like rounds, witnesses, supermajorities of Swirld),

However, if your system is designed around data having one absolute true state, not one which is dynamic and varied based on vantage point, we would suggest you rethink your design. So far, for every problem space where people thought they needed an absolute sequence of events or global consensus, we have been able to map an alternate approach without those requirements. Also, we already know this is how the world outside of computers works, so to design your system to require (or construct) an artificial reality is probably setting yourself up for failure, or at the very least for massive amounts of unnecessary computation, communication, and fragility within your system.

**TL;DR;** Holochains don't manage consensus, at least not about some absolute perspective on data or sequence of events. They manage distributed data integrity. Holochains do rely on consensus about the validation rules (DNA) which define that integrity, but so does every blockchain or blockchain alternative. If you have different validation rules, you're not on the same chain. These validation rules as establish the "data physics," then the applications are built on that.

## Can you run a cryptocurrency on a Holochain?
Theoretically, yes -- but for the moment, we'd discourage it.

If you don't know how to issue currencies through mutual credit, or how to account for them through double entry accounting, then you probably shouldn't build one on holochains. If you do understand those key principles, than it is not too difficult to build a cryptocurrency for which holochain provides ample accounting and data integrity.

However, you probably shouldn't try to do it in the way everyone is used to building cryptocurrencies on a global ledger of cryptographic tokens. [Determining the status of tokens/coins](https://en.bitcoin.it/wiki/Double-spending) is what create the need for global consensus (about the existence/status/validity of the token or coin). However, there are [other approaches to making currencies](https://medium.com/metacurrency-project/perspectives-on-blockchains-and-cryptocurrencies-7ef391605bd1) which, for example, involve [issuance via mutual credit](https://medium.com/metacurrency-project/beyond-blockchain-simple-scalable-cryptocurrencies-1eb7aebac6ae) instead of issuance by fiat.

Unfortunately, this is a hotly contested topic by many who often don't have a deep understanding of currency design nor cryptography, so we're not going to go too deep in this FAQ. We intend to publish a white paper on this topic soon, as well as launch some currencies built this way.
