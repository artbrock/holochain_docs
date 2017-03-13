All Nuclei types provide access to the same Holochain API, which consists in a set of functions that allow you as a Holochain app developer to commit entries to your chain, as well as inspect chain entries and properties.  Additionally there are a few functions, like `validate` that you *must* implement as the Nuclei will call them.

## Functions you can call

### `version` 

Returns Holochain version

### `commit <entry-type> <entry-data>`

Attempts to commit an entry to the chain.  Will cause your `validate` function to be called.  Returns either an error or the hash of the commited entry

### `property <name>`

Returns the named property.  Most of these properties are defined by the app developer, but there are a few that are system defined:  

- "_id" returns the hash of the holochain, which is it's id.
- "_agent_id" returns your unique node id
- "_agent_name" returns the indentifier string you used when initiating your chain

### `expose <name> <as>`

Declares to the Nucleus that it should expose your function `<name>` to the outside world for calling.  `<as>` declares the calling and return parameter types.  Currently this can be either `"JSON"` or `"STRING"`  In the Javascript nucleus these are defined on a global `MC` object, i.e. you would use `MC.JSON` or `MC.STRING`

### `put <hash>`

Publishes `<hash>` to the DHT.  `<hash>` must be the hash of a previously committed entry.

### `get <hash>`

Retrieves `<hash>` from the DHT. 

### `putmeta <hash> <meta-hash> <meta-tag>`

Associates `<meta-hash>` with `<hash>` as `<meta-tag>` on the DHT.  `<hash>` and `<meta-hash>` must be the hash of a previously committed entries.  

### `getmeta <hash> <meta-tag>`

Retrieves a list of meta values of tagged as `<meta-tag>` on `<hash>` from the DHT. 

## Functions you must write that the Nuclei will call

### `requires`

This function lets you tell the system about requirements your app needs, currently just a version number of holochain itself, i.e. to prevent an app from running on an older version of the system.  The return value is an object (JS) or hash (zygo) with a key "version" who's value is the version number desired.

### `validate <entry-type> <entry-data> <props>`

This function will be called when an entry is about to be committed, or when a DHT node is confirming that the entry is valid before publishing it.  In this function you should add all your application logic about what constitutes valid nodes.  The `<props>` argument will be an argument with various properties useful for validation:
- `<MetaTag>` if not an empty string, this value indicates validation in the context of putMeta, and will be the value of the tag being putmetaed
- `<Sources>` an array of the sources that created this entry

### `genesis`

This function will be called just after the initial genesis entries (the dna and the identity) are added.  It allows you take any chain initialization actions, for example using put and putmeta to add data to the DHT. 