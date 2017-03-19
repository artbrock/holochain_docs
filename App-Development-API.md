All Nuclei types provide access to the same Holochain API, which consists in a set of functions that allow you as a Holochain app developer to commit entries to your chain, put or get data from the DHT, and expose calls to a the UI.  

## Pre-Registered Values 

These are system variables which are available within your application. They are displayed below in dot notation used in JavaScript. If you want **to access these values in Lisp** replace the dot with an underscore (so App.DNAHash becomes App_DNAHash).

### System Variables
 - **`HC.VERSION`** (being implemented) Returns the version of the Holochain software
 - **`HC.JSON`** Used with `expose` to specify that a function expects to receive data in this format
 - **`HC.STRING`** Used with `expose` to specify that a function expects to receive data in this format
 - **`HC.MERKLE`** (to be implemented in Security Pass Milestone)

### Application Variables
 - **`App.DNAHash`** Stores the unique identifier of this Holochain's DNA. Nodes must run the same DNA to be on the same holochain.
 - **`App.Agent.Hash`** Stores your peer's permanent node address on the DHT. This is the hash of the 2nd entry on your chain.
 - **`App.Agent.String`** Stores the identity string used to initialize the holochain software with `hc init` If you  used JSON to embed multiple properties (such as FirstName, LastName, Email, etc), they can be retrieved here as App.Agent.FirstName, etc.

## Required Application Functions 

There are a few functions that you **must** implement in your app as the Nucleus will call them as a result of other operations.

### `genesis` 
This function is called during system genesis (from ```hc gen chain``` for example). It executes just after the initial genesis entries are committed to your chain (1st - DNA entry, 2nd Identity entry).  It enables you specify any additional operations you want performed when a person joins your holochain, for example, you may want to add some user/address/key information to the DHT to announce the presence of this new node.

### `validate <entry-type> <entry-data> <props>`

Note: This function will soon be deprecated and replaced by having validation functions for each type of DHT operation which requires it (e.g. validatePut, validatePutMeta, validateDel, validateDelMeta, etc.)

**The validation functions are the heart of ensuring distributed data integrity. Please consider them thoroughly and carefully as your systems data integrity is built from them.**

Validation functions are called under two distinct conditions. 

 1. When you try to commit something to your own source chain, the data will be validated to ensure you aren't breaking shared validation agreements which would fork you out of your shared holochain.
 2. Whenever any node of the DHT receives a request to store or change data in some way, the request and the changes are validated against the source chain of the person making the request These are the validation functions that check permissions and enforce any other kind of data integrity you intend to preserve. For example, in a distributed Twitter, only Bob should be able to attach (putmeta) a "post" to Bob as if it is his, and only Bob should be able to putmeta Bob as a "follower" of Alice. _Please keep in mind that system variables like App.Agent.Hash that return your own address will not return YOUR address when being executed by a remote DHT node. Code your functions as if anyone will be running them, because they will be._

In this function you should add all your application logic about what constitutes valid nodes.  The `<props>` argument will be an argument with various properties useful for validation such:
- `<MetaTag>` if not an empty string, this value indicates validation in the context of putMeta, and will be the value of the tag being putmetaed
- `<Sources>` an array of the sources that created this entry

## System Functions

### `property <name>`

Returns the named property.  These properties are defined by the app developer. It returns values from the DNA file that you set as properties of your application (e.g. Name, Language, Description, Author, etc.).

### `expose <name> <as>`

Declares to the Nucleus that it should expose your function `<name>` to the outside world for calling.  `<as>` declares the calling and return parameter types.  Currently this can be either `"JSON"` or `"STRING"`  In the JavaScript nucleus these are defined on a global `HC` object, i.e. you would use `HC.JSON` or `HC.STRING`

## Functions for Chain Operations

### `commit <entry-type> <entry-data>`

Attempts to commit an entry to your local, source chain. It will cause callback to your `validate` function.  Returns either an error or the hash of the committed entry upon success.

### `query` (still being implemented for direct query your local chain)

Keep in mind that you will want to retrieve most data from the DHT (shared data space), so that you are seeing what the rest of the nodes on your holochain are seeing. However, there are times you will want to query private data fields, or package up up data from your source chain for sending. Use this function.

## DHT Functions

### `put <hash>`

Publishes `<hash>` to the DHT.  `<hash>` must be the hash of a previously committed entry. Every DHT node who receives this put request as it is circulated via gossip, will call back to retrieve the source data from the source chain that published it. They will then confirm the signatures, chain integrity, and any other validation rules in written in the app.

### `get <hash>`

Retrieves `<hash>` from the DHT. 

### `putmeta <hash> <meta-hash> <meta-tag>`

Associates `<meta-hash>` with `<hash>` as `<meta-tag>` on the DHT.  `<hash>` and `<meta-hash>` must be the hash of a previously committed entries. You might think of this as creating graph entries, linking one bit of data to other data.

### `getmeta <hash> <meta-tag>`

Retrieves a list of meta values of tagged as `<meta-tag>` on `<hash>` from the DHT. 

## Deprecated Functions

### `property <name>`

This function still works to return properties named in your DNA file which match your properties schema, but the previous fixed values for the application and holochain have been moved to system variables specified above. 

- "_id" returns the hash of the holochain DNA, which is its unique identifier (Replaced by App.DNAHash)
- "_agent_id" returns your unique node id / DHT Address (Replaced by App.Agent.Hash)
- "_agent_name" returns the indentifier string you used when initiating your chain (Replaced by App.Agent.String)

### `version` 

Returns Holochain version (Deprecated - replaced by the HC.VERSION system variable)

### `requires` 

(Deprecated - Replaced by parsing requirements specified in DNA file) This function lets you tell the system about requirements your app needs, currently just a version number of holochain itself, i.e. to prevent an app from running on an older version of the system.  The return value is an object (JS) or hash (zygo) with a key "version" who's value is the version number desired.

