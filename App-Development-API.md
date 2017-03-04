All Nuclei types provide access to the same Holochain API, which consists in a set of functions that allow you as a Holochain app developer to commit entries to your chain, as well as inspect chain entries and properties.  Additionally there are a few functions, like `validate` that you *must* implement as the Nuclei will call them.

## Functions you can call

### `version` 

Returns Holochain version

### `commit <entry-type> <entry-data>`

Attempts to commit an entry to the chain.  Will cause your `validate` function to be called.  Returns either an error or the hash of the commited entry

### `property <name>`

Returns the named property.  Most of these properties are defined by the app developer, but there are a few that are system defined:  

- "_id" returns the hash of the holochain, which is it's id.

### `expose <name> <as>`

Declares to the Nucleus that it should expose your function `<name>` to the outside world for calling.  `<as>` declares the calling and return parameter types.  Currently this can be either `"JSON"` or `"STRING"`

### `put <hash>`

Publishes `<hash>` to the DHT.  `<hash>` must be the hash of a previously committed entry.

### `get <hash>`

Retrieves `<hash>` from the DHT. 


## Functions you must write that the Nuclei will call

### `validate <entry-type> <entry-data>`

This is the function that will be called when an entry is about to be committed, or when a DHT node is confirming that the entry is valid before publishing it.