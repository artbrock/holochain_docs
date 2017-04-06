# Holochain Application Development

## Holochain Documentation Notes
Show Diagrams and Subsystems

### Nucleus
As the nucleus of a cell contains the DNA instructions for that cell, holochains have a few different nucleii which can contain the Application code for a segment of the holochain. 
 - **Zygomys** - Zygo is a Lisp environment which runs inside go. You run the Zygo nucleus if you want to build your application in Lisp. The business rules, data validation components, and UI interfaces for your distributed app would be written in Zygomys Lisp. We found Zygo and it seemed like a fun place to start since we're fans of Lisp. This is currently the default Holochain Nucleus. (Status: Operational)
 - **Javascript** - You can also use a JavaScript for developing your code.  The JavaScript Nucleus is implemented using the [Otto](https://github.com/robertkrimen/otto) go engine. (Status: Operational)
 - **P3** - Since holochains are a component of the larger Ceptr project, we will implement our semantic trees and self describing protocols, P3 (Protocol for Pluggable Protocols).
 -  **Other** - (Status: Not Implemented) Do you like to code in Lua? Ruby? Python? Scheme? PHP? You can implement a Nucleus for your favorite language to code your distributed application by following the process we used for the above Nucleii. This page has a great [list Go Scripting Languages and VMs.](https://github.com/golang/go/wiki/Projects#virtual-machines-and-languages)

### Distributed Application Development
**Data Schemas:** You need to provide data schemas to validate data stored in chain entries or passed over the network (between the UI and application, and between application instances on different DHT nodes which need to get, put, find, replicate/gossip & validate data with each other).

**Validation Rules:** Validation rules are at the heart of holochains in that they are the basis for that only data that conforms to shared validation rules, propagates across the DHT (shared data space). They are used to ensuring data provenance, integrity and accountability. 

When you commit a new entry on your local source chain, the chain will call back you validation rules to make sure you don't sign invalid states to your own chain. Then once signed your chain, new entry data is put() to the DHT, where each node in the target neighborood (based on current DHT sharding) will run the validation rules on their side, and sign the results, before publishing the data.

**Cryptographic Signatures and Chain Integrity** The underlying holochain system provides two built-in validation rules which should be exposed to your application, and you may want to wrap to expose to the user interaces or other functions within your application. They are:

 - **ValidateSig(SourceNodeID,sig,hash):** You supply the address of the source chain (which be yourself to confirm locally, or can be someone else to have them confirm their signature), and the signature, and hash that you want to confirm. The system confirms that this is the signing of that hash based on source keys.
 - **ValidateChain(SourceNodeID,hashID):** Which confirms that a the provided hash is in fact signed to that source chain. It returns either an error (if its not in the chain), or the hash of it's previous chain link.

### Application Logic
Application logic that needs to be validated. For example, if you're application is a mutual credit currency, you probably need to validate that a person spending currency had an adequate balance from which to spend (or at least an adequate credit limit). This probably involves stepping through that person's whole chain and calculating the running balance.

In other words, if you need to validate someone was in a certain state such that they were allowed to perform an action, you probably have to build to that state from the history in their chain, from the beginning.

There are a couple possible shortcuts. 

1. **App Chain:** A holochain will likely have multiple applications built into it (e.g. key management app, messaging app, currency app). Each of those applications generate state-changing entries which are stored to the source chain.
2. **Audit Point:** (not implemented yet)


### Loosely Coupled UX / UI


Notes on Holochain data types and how to CRUD them

CRUD - At the level of the base object which essentially is the context for you holochain:
 - **Create** = Build / Launch the holochain app on your system
 - **Read** = Look up base DNA properties (id, name, description, shemas, code, etc.)
 - **Update** = There is no changing your base DNA except by forking... and creating a new AUDIT entry in you chains for carrover of chain state data.
 - **Delete** = The only way to "delete" a distributed holochain once it has started running is for EVERY participant to delete it from their local chain/node software installation. If anyone fails to delete/stop their node, then it is still running.

