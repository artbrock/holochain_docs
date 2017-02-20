# Glossary of Terms in Holochains

**Absolutism**
 : A view which says there is ONE absolute frame of reference or view of reality. For example, global ledger consensus systems assert the state of the ledger is the one authoritative absolute truth of the system.

**Agent**
 : An entity which operates with agency -- meaning a human or a program, which makes choices outside of whatever code is written into holochain

**Application Engine**
 : (a.k.a. Application Space) See Nucleus

**Author**
 : The agent who originally created the information on their source chain / author chain

**Blacklist**
 : A list of nodes who get blocked for attempting to propagate invalid information

**Blockchain**
 : An architecture used by Bitcoin, Ethereum and many cryptocurrencies to decentralize maintenance of a global ledger for token accounting

**CALM**
 : Consistency as Logical Monotonicity (see Monotonicity)

**CAP Theorem**
  : Consistency-Availability-PartitionTolerance. Often depicted as a triangle, where you can only achieve two of three options. Unfortunately in the real-world, you always have to include 'P' because networks are not 100% reliable. So the question often comes down to are you optimzing for data Consistency or data Availablily (making sure new data is not visibility to anyone until its visible to everyone). Holochains are eventually consistent, but our notion of Consistency is not that there is one set of absolutely true data, but rather you know exactly what was asserted by whom and when.

**Ceptr**
 : Short for Receptor. The distributed application and communications framework that Holochains are one small part of.

**Chain Entry**
 : A new transaction added to your source chain

**Consensus**
 : In the context of decentralized systems, it is the agreement among the nodes about the state of the system.

**Countersigning**
 : When all the agents involved with a transaction sign that transaction to each other's source chains.

**Cryptographic Proof**
 : Used to determine that data is untampered: Hashes are equal and Signatures match.

**Data Positivism**
 : The notion that data has "truth" in and of itself. Holochains are Data Relativistic, recognizing no independent truth, only who said what and when.

**Decentralized**
 : The elmination of the need for a central server or authority to maintain system integrity.

**DHT**
 : Distributed Hash Table: A structure that enables data to be shared across many machines: easily retrieved, untampered proof by hash.

**Distributed**
 : A system whose integrity is maintained between ALL nodes of the network (see P2P).

**DNA**
 : In the context of Holochains, this is the hash of instructions (app code and data schemas) that are valid to operate within that chain. In biology, it is the instructions coded into the nucleus of all the cells of an organism (DeoxyriboNucleic Acid)

**DPKI**
 : Distributed Public Key Infrastructure - A decentralized framework for matching public keys to addresses/users, which often supports other identity data management.

**End-to-End Encryption**
 : The cryptographic keys used to encrypt and decrypt are stored only at the end points.

**Entry**
 : A record in a source chain recorded by the agent who controls that chain

**Eventual Consistency**
 : A term in distributed computing which refers to the fact that data may be temporarily out of sync in parts of the network, but will eventually become consistent.

**Flagged**
 : Data that has been shared to the DHT, but faills to validate according the shared rules gets flagged as invalid. Nodes who keep producing invalid data may also get flagged, and then blacklisted.

**Full Peer**
 : A full holochain peer is a machine which is running both as a Source Chain for creating new data and a DHT Node for synchronizing shared data

**Genotype**
 : In this context, two holochains may have identical genotypes (same code/DNA) but will be used by different groups to be expressed in different ways, thus they are in fact separate and unique. What is different is the group of people and/or what they are communicating to each other. (See phenotype)

**Global Ledger**
 : A centralized and chronological record of all transactions in a particular system.

**Gossip**
 : Gossip refers to the communications that nodes in the DHT do to keep each other up to date with the data they're supposed to be collectively holding, and managing.

**Hash**
 : A mathematical algorithm that compactly maps data in a one-way function that is infeasible to invert. The workhorse of modern cryptography.

**HashChain**
 : The successive use of cryptographic hash functions to a piece of data that allows many single-use keys to be created from a single key.

**Holochain**
 : Holochain does not refer to any single chain, but the collective construct of multiple source chains which post their public data to a shared data space (usually a DHT), which validates the authorship, provenance, and any required application logic or business rules BEFORE propagating the data.

**HolochainID**
 : The Hash by which a holochain is known . This is the hash of the holochains DNA (application code and data schemas) combined with unique identifying information about the group using this DNA.

**Immune System**
 : Holochains are designed for eventual consistency and for detecting attempts to compromise data consistency by bad actors on the network. We refer to the mechanisms encoded in a holochain to protect itself as the holochain's immune system. For example: blacklisting someone who keeps posting invalid data, or counterfeits data as if was created by others.

**Merkle Proof**
 : A mechanism that extends the ability to authenticate a small amount of data to allowing the authentication of large databases. E.G. A user of a Merkle tree with a publicly known and trusted root can ask for a Merkle proof to verify any value in the tree is correct.

**Merkle Tree**
 : A hash tree that allows efficient and secure verification of the contents of large data structures (see hash tree).

**MetaData**
 : (in the DHT) Our DHT allows people to store data (which gets hashed and then stored at the address that is the hash of the data), as well as store metadata about that the data at that hash, such as who has verified it, or what data it links to.

**Monotonic**
 : In the case of holochain, this refers to the idea that data elements only increase, never decrease. In distributed systems, it can be very hard to synchronize the removal of data, so we keep a record of the existence of the data, and mark it deleted (or expired, or flagged as invalid). But if you remove the data, there's nothing to attach the synchronization markers to.

**Multi-party Transaction**
 : A transaction which involves multiple different agents, and must be countersigned by all parties to each of their source chains before propagation on the shared DHT.

**Neighborhood**
 : As a DHT grows larger with more and more members, it is segmented or sharded into neighborhoods which managed data together. Nodes in a neighborhood "gossip" to update each other, and also track participation information to make sure their neighbors haven't gone rogue (See Immune System)

**Node**
 : A node is a machine participating in the DHT peer-to-peer communications involved in sharing and validating data.

**NodeID**
 : The address of a node in the DHT

**Nucleus**
 : Application Container implemented for a specific language

**P2P**
 : Peer-to-Peer: An approach to distributed system development where every peer is an equal to other peers and they coordinate in that manner.

**P3**
 : Protocol for Pluggable Protocols - Ceptr's self-describing protocols stack which enables interoperability between holochains.

**Phenotype**
 : In this context, a phentoype is the way in which two holochains with identical code (genotype) express themselves in different ways thus making them separate and unique. What is different is the group of people and/or what they are communicating to each other. (See genotype)

**Private Key**
 : A key known only to the recipient in a cryptographic system that is used to decrypt a message (see public key).

**Provenance**
 : The official record of origin of data.

**Public Key**
 : A key known to all nodes in a cryptographic system that is used to encrypt a message.

**Schema**
 : A definition used to define what data can be used in a context, as well as some parameters for validating that data (is it required, in a specific range, etc.)

**Semantic Tree**
 : A native data structure of Ceptr. Trees are used to show the structure of data and each node in the tree has a semantic marker referencing its definition and methods.

**Semtrex**
 : SEM Tree Regular Expressions: A universal parsing system for matching against symantic trees

**Shard**
 : A large DHT gets segmented or sharded into neighborhoods which manage data together. This allows the data to be shared without everybody needing to a full copy of ALL the data (as in a global ledger)

**Shared Store**
 : The wholeness of holochains come from combining local signed source chains with a shared data store via DHT.

**Signature**
 : A cryptographic signature is usually creating by creating a cryptographic hash of some data and encrypting that hash with your private key. This proves it was you who signed it (or at least somweone who had your keys, and that the data being signed hasn't been altered because it resolves to the expected hash).

**SNARK**
 : Succinct Non-interactive ARgument of Knowledge - A form of Zero Knowledge Proof that can be used for show validation of a particular process

**Source**
 : Typically refers to the agent or person that authored data or sent a message.

**Source Chain**
 : (a.k.a Authoring Chain) This is the local signed hash chain that you commit new data to before sharing it to the validating DHT.

**SourceID**
 : The identity of the source of a particular message, or piece of data or metadata

**TimeStamp**
 : Holochain activities are recorded with the time and date something happened acording to the time on their machine. Holochains do not have a guaranteed global time, but may refuse to synchronize transactions with nodes whose clocks  are too far out of sync.

**User**
 : In the context of data attribution it refers to the Author or Agent who created the data. In the context of an Application or UI/UX, it refers to the person using the app (seeing the data)

**Validating DHT**
 : A DHT where every node executes consistent validation rules on data before propagating that data.

**Validation**
 : Confirming that data is valid according to the shared rules of a holochain. This should happen before data is committed to your source chain, and must happen as data is propagated across a DHT.

**Validation Rules**
 : The rules which enforce valid data that can be committed to source chains as well as data that can propagate via the DHT.

**Zero Knowledge Proof**
 : A method in which one agent can prove to another agent that something is true, without having to share any additional information except that it is true.

**Zome**
 : (as in Chromosome) Each Nucleus can contain multiple Zomes (for example, the JavaScript App Engine can contain multiple applications). All data elements committed to source chain or propagated on the DHT, comply with the data schema and validation rules of a particular Zome.
