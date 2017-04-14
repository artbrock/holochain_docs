# Technical Architecture
[White paper link](http://ceptr.org/whitepapers/holochain)
Okay, maybe everybody thinks their software is a special and unique snowflake and you've never seen anything like it before. This time, we really mean it.

Hopefully, that previous sentence will be made obsolete soon, but until then, please consider that Holochain doesn't work like anything you've used before, and you cannot think about it like a blockchain. It is necessary to get your bearings on:
 - what the system components are,
 - how the distinct parts inter-operate as a unified whole
 - and the strange jargon we use for various elements.

> **Jargon Disclaimer:** Apologies in advance for the unfamiliar language used for system components. To avoid confusion, we didn't want to call things familiar words, when they don't behave in the expected familiar ways. So [we used new words](Glossary), frequently drawing from biological counterparts when available.

For example: Once launched the code that runs applications in a Holochain is immutable without forking the chain. This code is the **DNA** specific to that holochain. Therefore the code is hashed into the first entry of each participant's chain

### Architecture Overview
Three Subsystems

Holochains have three main functional domains, plus whatever UI you provide to access your application. When you build a new holochain application, you need to code all of these systems.

1. Shared data space setting (holochain identity, DHT settings, etc.)
2. Data schema, chain entry types with associated application logic and validation rules for the authoring hashchain
3. Application Layer: This is nucleus of the system which every other part interacts with. Author chains use the app logic and validation rules. DHT nodes use the validation rules to confirm data integrity and provenance before publishing, the UI interfaces with the methods provided here as well.

![Holochain Sub-Systems](http://ceptr.org/images/Holochain_Subsystems.png)

### Functional Domains
Holochains, by design, should be used in the context of a group operating by a shared set of agreements. Generally speaking, you don't need a holochain if you are just managing your own data.

These agreements are encoded in the validation rules which are checked before authoring to one's local chain, and are also checked by every DHT node asked to publish the new data.

In essence these ensure holochain participants operate according the same rules. Just like in blockchains, if you collude to break validation rules, you essentially have forked the chain. If you commit things to your chain, or try to publish things which don't comply with the validation rules, the rest of the network/DHT rejects it.

#### 1. Group DNA / Holochain configuration
At this stage, a developer needs to set up the technical configuration of the collective agreements enforced by a holochain. This includes such things as: the holochain name, UUID, address & name spaces, data schemas, validation rules for chain entries and data propagation on the DHT,

#### 2. Individuals Authoring Content
As an individual, you can join a holochain by installing its holochain configuration and configuring your ID, keys, chain, and DHT node in accord with the DNA specs.

#### 3. Application API
Holochains function like a database. They don't have much end-user interface, and are primarily used by an application or program to store data. Unless you're a developer building one of these applications, you're not likely interact directly with a holochains. Hopefully, you install an application that does all that for you and the holochain stays nice and invisible enabling the application to store its information in a decentralized manner.

#### 4. Browser-Based UI
One easy way to provide a UX/UI for people to interact with your Application layer is to have a browser connect via web socket. Our initial Application Engines support JSON data exchange. You can use HTML/CSS/JavaScript for whatever UX you'd like to create.

### Two Distinct Sub-Systems
There are two modes to participate in a holochain: as a **chain author**, and as a **DHT node**. We expect most installations will be doing both things and acting as full peers in a P2P data system. However, each could be run in a separate
container, communicating only by network interface.

#### 1. Authoring on your Local Chain
![Holochain_Source](http://ceptr.org/images/Holochain_Source.png){:class="img-responsive"}
Your chain is your signed, sequential record of the data you create to share on the holochain. Depending on the holochain's validation rules, this data may also be immutable and non-repudiable. Your local chain/data-store follows this pattern:

1. Validates your new data
2. Stores the data in a new chain entry
3. Signs it to your chain
4. Indexes the content
5. Shares it to the DHT
6. Responds to validation requests from DHT nodes

#### 2. Running a DHT Node
![Holochain_DHT](http://ceptr.org/images/Holochain_DHT.png){:class="col-xl-4 col-lg-4 col-md-4 col-sm-4 pull-right img-responsive"}

For serving data shared across the network. When your node receives a request from another node to publish DHT data, it will first validate the signatures, chain links, and any other application specific data integrity in the entity's source chain who is publishing the data.


## Developer Resources
How the heck do you start building an app to run on holochains?
Documentation coming out of the Hackathon presently.

### Data schemas
We support JSON Data Schemas

### Validation Rules
Scripted in [Javascript](https://github.com/robertkrimen/otto) or [Lisp](https://github.com/glycerine/zygomys)

### UI via light-weight built-in web server
For now we expect to run these via a web browser UI.  Future development plans include templates for Electron Apps as well mobile platforms.
