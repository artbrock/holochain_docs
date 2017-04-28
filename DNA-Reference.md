# Application DNA

Application DNA consists of the DNA file at the core in which a number of application properties, plus application code and entry types are specified.

Every Holochain app has a DNA file at its core.

These are the fields to be specified in the DNA file are as follows:

TODO: flesh out the descriptions of the fields.

**Version**
 : An integer value describing the version of this DNA file. Perhaps document this via a commented json example?

**Id**
 : A UUID which the system auto-generates when you clone existing code, to make sure your new holochain is unique and doesn't accidentally collide with the holochain whose code you are cloning. For example, you can clone the DNA of a slack team you are a part of, and the fact that we generate a new UUID for you, will keep you from accidentally creating a new slack team in the same data-space/DHT as your old one. NOTE: If you are not trying to create a new clone, use `hc join` instead of `hc clone`.

**Name**
 : A string value of the name of the application. It is best to edit this if you are cloning an existing chain to serve a new purpose. For example, if you want to create a new slack team, what is the new team called?

**RequiresVersion**
 : An integer value that specifies the minimum usable version of the Holochain library for the app for app code compatibility as well as data structure compatibility.

**Properties**
 : A hashmap of app defined strings that are available to the app via the `property` function.  This is especially useful for applications that will be cloned and customized.

## DHT Settings (not yet implemented)
Special settings for DHT configuration for this application... Possibly stuff for saturation & verification based on # signatures...

**HashType**
 : A string describing the hash type to be used for this application.  Should be from the list of hash types from the [multihash library](http://multiformats.io/multihash/)

**Sharding**
 : Identifier for sharding method (none, hashmask, proximity, etc.)

**Redundancy**
 : integer marking minimum online redundancy targets for data

**Encryption**
 : settings for point-to-point encryption (what are the options?)

## Zomes Entries
A list of Zome entries, for each zome which may include the following:

**Name**
 : string -- 

**Description**
 : string -- 

**NucleusType**
 : string -- 

**Code**
 : string -- file name of DNA code

**CodeHash**
 : Hash --

###	Functions   [ ]FunctionDef
**Name**
 : string -- 

**CallingType**
 : string ( "string" or "json" ) --

###	Entries     [ ]EntryDef
Inside a Zome section

**Name**
 : string --

**DataFormat**
 : string ( "string", "json", or "links" )

**Schema**
 : string // file name of schema or language schema directive

**SchemaHash**
: Hash

**Sharing**
 : string // **Public** is automatically shared to the DHT after committing to your chain. **Private** is NOT shared to the DHT, and remains only on your source chain. Or **Partial** selected fields are shared to DHT and structured for Merkle Proof of validity of those fields while not exposing hidden ones.

**Validator**
 : SchemaValidator
