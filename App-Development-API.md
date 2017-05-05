All Nuclei types provide access to the same Holochain API, which consists in a set of functions that allow you as a Holochain app developer to commit entries to your chain, put or get data from the DHT, and expose calls to a the UI.  

## Pre-Registered Values

These are system variables which are available within your application. They are displayed below in dot notation used in JavaScript. If you want **to access these values in Lisp** replace the dot with an underscore (so `App.DNA.Hash` becomes `App_DNA_Hash`).

### System Variables
 - **`HC.Version`** Returns the version of the Holochain software
 - **`HC.Status`** Object with status value constants: `Live`,`Deleted`,`Modified`,`Rejected`,`Any` for use with `StatusMask` when getting entries or links.
 - **`HC.LinkAction`** Object with link action value constants: `Add`,`Del` for use when committing Links entries.
 - **`HC.Merkle`** (to be implemented in Security Pass Milestone)

### Application Variables
 - **`App.Name`** holds the Name of this Holochain from the DNA.
 - **`App.DNA.Hash`** holds the unique identifier of this Holochain's DNA. Nodes must run the same DNA to be on the same holochain.
 - **`App.Key.Hash`** holds the hash of your public key. This is your self-validating address on the DHT which functions as a kind of alias pointing to your permanent key above.
 - **`App.Agent.Hash`** holds your peer's permanent node address on the DHT. This is the hash of the second entry (identity info) on your chain.
 - **`App.Agent.String`** holds the identity string used to initialize the holochain software with `hc init` If you  used JSON to embed multiple properties (such as FirstName, LastName, Email, etc), they can be retrieved here as App.Agent.FirstName, etc.

## Required Application Functions

There are a few functions that you **must implement** in your app as the Nucleus will call them as a result of other operations.

### `genesis`
This function is called during system genesis (from ```hc gen chain``` for example). It executes just after the initial genesis entries are committed to your chain (1st - DNA entry, 2nd Identity entry).  It enables you specify any additional operations you want performed when a person joins your holochain, for example, you may want to add some user/address/key information to the DHT to announce the presence of this new node.

### Validation functions:

### `validateCommit <entry-type> <entry> <header> <sources>`

This function gets called when an entry is about to be committed to a source chain.  Use this function to describe the agreements about data as it should be added to shared holochain.  This function gets called for all entry types.

### `validatePut <entry-type> <entry> <header> <sources>`

This function gets called when and entry is about to be committed to the DHT on any node.  It is very likely that this validation routine should check the same data integrity as validateCommit, but, as it happens during a different part of the data life-cycle, it may require additonal validation steps.  [add example here]  This function will only get called on entry types with "public" sharing, as they are the only types that get put to the DHT by the system.

### `validateLink <linkEntryType> <baseHash> <linkHash> <tag> <sources>`

This function gets called when ever a link is about to be added to the DHT.  Links are added for every linking element in the special "links" entry type.  Note that this is a DHT level validation routine, in that it gets called when the Link message is received by a DHT node, not when the linking entry is committed.  The regular validateCommit routine gets called as usual when that entry is committed to the source chain.

**The validation functions are the heart of ensuring distributed data integrity. Please consider them thoroughly and carefully as your systems data integrity is built from them.**

Validation functions are called under two distinct conditions.

 1. When data is about to be added to the local chain (validateCommit).
 2. Whenever any node of the DHT receives a request to store or change data in some way, the request and the changes are validated against the source chain of the person making the request (validatePut,validateLink.) These are the validation functions that check permissions and enforce any other kind of data integrity you intend to preserve. For example, in a distributed Twitter, only Bob should be able to attach (link) a "post" to Bob as if it is his, and only Bob should be able to link Bob as a "follower" of Alice. _Please keep in mind that system variables like App.Agent.Hash that return your own address will not return YOUR address when being executed by a remote DHT node. Code your functions as if anyone will be running them, because they will be._

- `<Sources>` an array of the sources that created this entry

## System Functions

### `property <name>`
Returns the named property.  These properties are defined by the app developer. It returns values from the DNA file that you set as properties of your application (e.g. Name, Language, Description, Author, etc.).

## Functions for Chain Operations (may have DHT side-effects)

### `commit <entry-type> <entry-data>`

Attempts to commit an entry to your local, source chain. It will cause callback to your `validate` function.  Returns either an error or the hash of the committed entry upon success.

TODO: document committing links.

### `mod <entry-type> <entry-data> <replaces>`

Attempts to commit an entry to your local, source chain that "replaces" a previous entry.  If <entry-type> is not private, `mod` will mark <replaces> as Modified on the DHT.  Additionally the modification action will be recorded in the entries' header in the local chain, which will be used by validation routes. Returns either an error or the hash of the committed entry upon success.

### `query` (still being implemented for direct query your local chain)

Keep in mind that you will want to retrieve most data from the DHT (shared data space), so that you are seeing what the rest of the nodes on your holochain are seeing. However, there are times you will want to query private data fields, or package up up data from your source chain for sending. Use this function.

### `del <hash> <message>`

Commits a DelEntry to the local chain with given delete message, and, if the entry type of <hash> is not private, moves the entry to the `Deleted` status on the DHT.

## Functions for DHT query 

### `get <hash> [<StatusMask>]`

Retrieves `<hash>` from the DHT according to the optional <StatusMask>. If unspecified <StatusMask> will be assumed to be `Live`

### `getLink <base> <tag> [<options>]`

Retrieves a list of links tagged as `<tag>` on `<base>` from the DHT.  Options is hash map of values.  Currently the options are
- `Load: <bool>` which if set to true tells the library to resolve get the entry values of the links.  With options as `{Load: false}` returns a list of the form `{Links: [{H:"QmY..."},..]}`  With options as `{Load: true }` returns a list of the form `{Links: [{H:"QmY...",E:"<entry value here>"},..]}`
- `StatusMask: <int>` which determine which status links to return.  Default is to return only Live links.  You can use defined constants `HC.Status.Live/Deleted/Modified/Rejected` as the <int> value.

## Deprecated Functions

### `expose <name> <as>`

This function used to declares to the Nucleus that it should expose your function `<name>` to the outside world for calling. `<as>` declares the calling and return parameter types. You now must declare these functions in the DNA file itself under the `Functions` section.

### `property <name>`

Currently, this function returns properties described in your DNA file.

This function used to also return this fixed values, but these have since been made accessible elsewhere:
- "\_id" returns the hash of the holochain DNA, which is its unique identifier (Replaced by App.DNA.Hash)
- "\_agent_id" returns your unique node id / DHT Address (Replaced by App.Agent.Hash)
- "\_agent_name" returns the indentifier string you used when initiating your chain (Replaced by App.Agent.String)

### `put <hash>`

This function used to publish data to the DHT.  Now puts occur as a part of the `commit` function.

Publishes `<hash>` to the DHT.  `<hash>` must be the hash of a previously committed entry. Every DHT node who receives this put request as it is circulated via gossip, will call back to retrieve the source data from the source chain that published it. They will then confirm the signatures, chain integrity, and any other validation rules in written in the app.

### `putmeta <hash> <meta-hash> <meta-tag>`

This function used to publish data to the DHT.  Now puts occur as a part of the `commit` function, when you commit entries of the special "links" type.

Associates `<meta-hash>` with `<hash>` as `<meta-tag>` on the DHT.  `<hash>` and `<meta-hash>` must be the hash of a previously committed entries. You might think of this as creating graph entries, linking one bit of data to other data.

### `version`

Returns Holochain version (Deprecated - replaced by the HC.VERSION system variable)

### `requires`

(Deprecated - Replaced by parsing requirements specified in DNA file) This function lets you tell the system about requirements your app needs, currently just a version number of holochain itself, i.e. to prevent an app from running on an older version of the system.  The return value is an object (JS) or hash (zygo) with a key "version" who's value is the version number desired.
