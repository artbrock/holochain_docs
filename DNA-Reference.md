# Application DNA

Application DNA consists of the DNA file at the core in which a number of application properties, plus application code and entry types are specified. Every Holochain app has a DNA file at its core.

The fields to be specified in the DNA file are as follows:

**Version**
 : An integer value describing the version of this DNA file.

**UUID**
 : A UUID, (which the system auto-generates when you clone existing code) to make sure your new holochain is unique and doesn't accidentally collide with the holochain whose code you are cloning. For example, you can clone the DNA of a chat team you are a part of, and the fact that we generate a new UUID for you, will keep you from accidentally creating a new chat team in the same data-space/DHT as your old one. NOTE: If you are not trying to create a new clone, use `hc join` instead of `hc clone`.

**Name**
 : A string value of the name of the application. It is best to edit this if you are cloning an existing chain to serve a new purpose. For example, if you want to create a new slack team, what is the new team called?

**RequiresVersion**
 : An integer value that specifies the minimum usable version of the Holochain library for the app for app code compatibility as well as data structure compatibility.

**Properties**
 : A hashmap of app defined strings that are available to the app via the `property` function.  This is especially useful for applications that will be cloned and customized.

## Zomes 
A holochain application may be broken into different functional domains called Zomes. In keeping with the biological language of DNA and the different Nuclei as the virtual machines that run the DNA code, it seemed appropriate to think of these as Chromosomes (which is way to long to type, so we went with zomes). One example of why you might have different zomes, is that you may be mashing a few different holochain apps together into a new unified holochain app. Each of those previously separate apps, could be a Zome in your new app.

Each Zomes section contains some basic info about the Zome (Name, Description, NucleusType, Code (filename), Hash of that file, etc), as well as identifying what entry types (data structures) are used within this Zome, and what functions in this zome are available for calling from UIs or other Zomes.

**Name**
 : (string) Name of the Zome (What functionality is this part of your application managing?) Please don't put spaces in the Zome name, we'll probably remove them, but haven't implemented this yet.

**Description**
 : (string) Description of the functionality/features being managed within this Zome.

**NucleusType**
 : (string) Which virtual machine should be used to process the code in this Zome? Valid values are: JavaScript, Lisp, (and as we build them: Ruby, Lua, etc.)

**Code**
 : (string) File name to find the code for this Zome. By convention, please use <ZomeName>.<LanguageExtension>, like chat.js, sort.zy (for zygomys Lisp). Once we complete ticket [#100](https://github.com/metacurrency/holochain/issues/100) we'll look for this filename by default if you leave this out of the DNA.

**CodeHash**
 : (hash) The hash of the code automatically gets generated when you `hc gen chain`. The value of a hash looks a little strange in TOML files as it's a comma delimited array of numbers. Having this specified in the DNA is part of what locks in the version of code peers on a holochain are agreeing to run.

### Zomes.Functions   [ ]FunctionDef
The functions that are used inside this Zome that you want to expose to be callable by some other part of the holochain system (such as a web UI, or accessible to another Zome)

**Name**
 : (string) Function Name 

**CallingType**
 : (string) How are parameters structured when passed to this function, as well as how the results are returned. Options: "string" or "json" 

### Zomes.Entries     [ ]EntryDef
Inside a Zome section A list of Zome entries, for each zome which may include the following:

**Name**
  : (string) Name of this Entry Type (or Data Structure)

**DataFormat**
  : (string) What format is this data structure? Options: string, json, links (a predefined data structure used for creating semantic links between entries in the DHT)

**Schema**
 : (string) The file name of schema or language schema directive (e.g. "blogpost.json"). You can leave this out of the DNA if there is no schema file (for DataFormats that are strings or links, for example)

**SchemaHash**
 : (hash) The hash of the schema file automatically gets generated when you `hc gen chain`. Having this specified in the DNA is part of what locks in the data structures peers on a holochain are sharing. This value looks a little strange in TOML files as it's a comma delimited array of numbers. 

**Sharing**
 : (string) **Public** entries are automatically shared to the DHT after being committed to your chain. **Private** entries are NOT shared to the DHT, and remain only on your source chain. **Partial** entries mean selected fields are shared to DHT and structured for Merkle Proof of validation of those fields while not exposing hidden ones.

## DHTConfig
Settings for DHT behaviors and requirements specific to your application. (STATUS: Not Completely Implemented -- will be complete with ticket [#151](https://github.com/metacurrency/holochain/issues/151)) Future features may possibly include parameters for saturation & multi-sig verification thresholds

### Implemented:

**HashType**
 : (string) Identifies hash type to be used for this application.  Should be from the list of hash types from the [multihash library](http://multiformats.io/multihash/)

### Not Implemented:

**NeighborhoodSize**
 : (integer) Establishes minimum online redundancy targets for data, and size of peer sets for sync gossip. A neighborhood size of ZERO means no sharding (every node syncs all data with every other node). ONE means you are running this as a centralized application and gossip is turned OFF. For most applications we recommend neighborhoods no smaller than 8 for nearness or 32 for hashmask sharding. 

**ShardingMethod**
 : Identifier for sharding method (none, XOR, hashmask, other nearness algorithms?, etc.)

**MaxLinkSets**
 : (integer) Maximum number of results to return on a GetLinks query to keep computation and traffic to a reasonable size. You need to break these result sets into multiple "pages" of results retrieve more.

**ValidationTimeout**
 : (integer) Time period in seconds, until data that needs to be validated against a source remains "alive" to keep trying to get validation from that source. If someone commits something and then goes offline, how long do they have to come back online before DHT sync requests consider that data invalid? 

**PeerTimeout**
 : (integer) Time period in seconds, until a node drops a peer from its neighborhood list for failing to respond to gossip requests.

**WireEncryption**
 : settings for point-to-point encryption of messages on the network (none, AES, what are the options?)

**DataEncryption**
 : What are the options for encrypting data at rest in the dht.db that don't break db functionality? Is there really a point to trying to do this?

**MaxEntrySize**
  : Sets the maximum allowable size of entries for this holochain
