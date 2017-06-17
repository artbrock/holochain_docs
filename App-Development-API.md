All Nuclei types provide access to the same Holochain API, which consists in a set of functions that allow you as a Holochain app developer to commit entries to your chain, put or get data from the DHT, and expose calls to a the UI.  

## Pre-Registered Values

There are two kinds of values that are available to your application, system globals and app specific values. 
  They are displayed below using the dot notation used in JavaScript.  **To access these values in the Lisp nucleus** replace the dots with underscores (so `App.DNA.Hash` becomes `App_DNA_Hash`).

### System Globals and Constants
 - **`HC.Version`** Returns the version of the Holochain software
 - **`HC.Status`** Object with status value constants: `Live`,`Deleted`,`Modified`,`Rejected`,`Any` for use with `StatusMask` when getting entries or links.
 - **`HC.GetMask`** Object with get option value constants: `Default`,`Entry`,`EntryType`,`Sources`,`All`
 - **`HC.LinkAction`** Object with link action value constants: `Add`,`Del` for use when committing Links entries.
 - **`HC.PkgReq`** Object with package request constants: `Chain`, `ChainOpt`, `EntryTypes`
 - **`HC.PkgReq.ChainOpt`** Object with package request `Chain` request constants: `None`, `Headers`, `Entries`, `Full` (see validation for example uses of these package request constants)
 - **`HC.Merkle`** (to be implemented in Security Pass Milestone)


### Application Globals
 - **`App.Name`** holds the Name of this Holochain from the DNA.
 - **`App.DNA.Hash`** holds the unique identifier of this Holochain's DNA. Nodes must run the same DNA to be on the same holochain.
 - **`App.Key.Hash`** holds the hash of your public key. This is your self-validating address on the DHT which functions as a kind of alias pointing to your permanent key above.
 - **`App.Agent.Hash`** holds your peer's permanent node address on the DHT. This is the hash of the second entry (identity info) on your chain.
 - **`App.Agent.String`** holds the identity string used to initialize the holochain software with `hc init` If you  used JSON to embed multiple properties (such as FirstName, LastName, Email, etc), they can be retrieved here as App.Agent.FirstName, etc.

## System Functions

### `property <name>`
Returns the named property.  These properties are defined by the app developer. It returns values from the DNA file that you set as properties of your application (e.g. Name, Language, Description, Author, etc.).

### `makeHash <entry-data>`
Returns the hash of the given entry data.

### `call <zome-name> <function-name> <arguments>`
Calls an exposed function from another zome. `<arguments>` is a string or an object/hash depending on the CallingType that was specified in the function's definition.

### `debug <value>`
Outputs `<value>` to the debug log.  `<value>` can be any type, and will get converted to a string.

## Functions for Chain Actions (may have DHT side-effects)

### `commit <entry-type> <entry-data>`

Attempts to commit an entry to your local, source chain. It will cause callback to your `validate` function.  Returns either an error or the hash of the committed entry upon success.

TODO: document committing links.

### `update <entry-type> <entry-data> <replaces>`

Attempts to commit an entry to your local, source chain that "replaces" a previous entry.  If `<entry-type>` is not private, `update` will mark `<replaces>` as Modified on the DHT.  Additionally the modification action will be recorded in the entries' header in the local chain, which will be used by validation routes. Returns either an error or the hash of the committed entry upon success.

### `query` (still being implemented for direct query your local chain)

Keep in mind that you will want to retrieve most data from the DHT (shared data space), so that you are seeing what the rest of the nodes on your holochain are seeing. However, there are times you will want to query private data fields, or package up up data from your source chain for sending. Use this function.

### `remove <hash> <message>`

Commits a DelEntry to the local chain with given delete message, and, if the entry type of `<hash>` is not private, moves the entry to the `Deleted` status on the DHT.

## Node-to-Node communication Functions

### `send <to> <message>`
Sends a message to a node.  The return value of this function will be what ever is returned by the `receive` function on the receiving node, see below.

### `receive <from> <message>`
This function will get called by the system when a message is received by a node.  The return value of the function will be sent back to the sender and will be the result of the `send` function that sent the message.

## Functions for DHT query 

### `get <hash> [<options>]`

Retrieves `<hash>` from the DHT.  Options is a hash map of modifiers about how/what to retrieve. Currently the options are:

- `StatusMask: <int>` determines which status entries to return.  You can use defined constants `HC.Status.Default/Live/Deleted/Modified/Rejected` as the `<int>` value.  If unspecified `<StatusMask>` will be assumed to be `Default` which is returns only live values entries resolving any modified entries.
- `GetMask: <int>` determines what to return.  You can use defined constants `HC.GetMask.Default/Entry/EntryType/Sources/All`. If unspecified `<GetMaks>` will be assumed to be `Default` which is returns just the entry type.  If more than one element in the mask is chosen the result will be an object using the mask value as the key of all the chosen return types.
- `Local: <boolean>` if true indicates that the get should return data only from the local chain, *NOT* from the DHT. 

### `getLink <base> <tag> [<options>]`

Retrieves a list of links tagged as `<tag>` on `<base>` from the DHT.  Options is hash map of values.  Currently the options are:
- `Load: <bool>` which if set to true tells the library to resolve get the entry values of the links.  With options as `{Load: false}` returns a list of the form `{Links: [{H:"QmY..."},..]}`  With options as `{Load: true }` returns a list of the form `{Links: [{H:"QmY...",E:"<entry value here>"},..]}`
- `StatusMask: <int>` which determine which status links to return.  Default is to return only Live links.  You can use defined constants `HC.Status.Live/Deleted/Rejected` as the <int> value.

## Required Application Functions

There are a few functions that you **must implement** in your app as the Nucleus will call them as a result of other operations.

### `genesis`
This function is called during system genesis (from ```hc gen chain``` for example). It executes just after the initial genesis entries are committed to your chain (1st - DNA entry, 2nd Identity entry).  It enables you specify any additional operations you want performed when a person joins your holochain, for example, you may want to add some user/address/key information to the DHT to announce the presence of this new node.

### Validation functions:

### `validateCommit <entry-type> <entry> <header> <package> <sources>`

This function gets called when an entry is about to be committed to a source chain.  Use this function to describe the agreements about data as it should be added to shared holochain.  This function gets called for all entry types.

### `validatePut <entry-type> <entry> <header> <package> <sources>`

This function gets called when and entry is about to be committed to the DHT on any node.  It is very likely that this validation routine should check the same data integrity as validateCommit, but, as it happens during a different part of the data life-cycle, it may require additional validation steps.  [add example here]  This function will only get called on entry types with "public" sharing, as they are the only types that get put to the DHT by the system.

### `validateMod <entry-type> <entry> <header> <replaces> <package> <sources>`
This function gets called as a consequence of a `mod` command being issued. `<replaces>` is the hash of the entry being replaced. Often you may be validating that only agent who committed the entry can modify it.  Such a validation routine might look like this:

``` javascript
function validateMod(entry_type,entry,header,replaces,pkg,sources) {
    var valid = false;
    if (entry_type=="your_type") {
        var orig_sources = get(replaces,{GetMask:HC.GetMask.Sources});
        //Note: error checking on this get removed for simplicity
        valid = (orig_sources.length == 1 && orig_sources[0] == sources[0]);
    }
    return valid;
}
```

### `validateDel <entry-type> <hash> <package> <sources>`

This function gets called as a consequence of a `del` command being issued.

### `validateLink <linkEntryType> <baseHash> <links> <pkg> <sources>`

This function gets called when ever a links are being to the DHT.  Links are added for every linking element in the special "links" entry type.  Note that this is a DHT level validation routine, in that it gets called when the Link message is received by a DHT node, not when the linking entry is committed.  The regular validateCommit routine gets called as usual when that linking entry is committed to the source chain.

### Validation Packaging

Each one of the validate commands has a corresponding package building command that will get called on the source node as part of the validation cycle so that the appropriate data can get sent back to the node trying to validate the action.  The functions are:

### `validatePutPkg <entry-type>`
### `validateModPkg <entry-type>`
### `validateDelPkg <entry-type>`
### `validateLinkPkg <entry-type>`

Note that a `commit` action will trigger a call to validatePutPkg locally when committing happens as validateCommit must have the same data available to it as does validatePut. 

All these functions should simply return nil if the data required by their corresponding validation function is just the minimum default of the Entry and Header of the action. Otherwise these functions must return a "Package Request" object, which specifies what data to be sent to the validating node.

So, for example, a JavaScript nucleus a package request function that indicates that the entire chain should be sent in the validation package, would look like this:
```JavaScript
function validatePutPkg(entry_type) {
  var req = {};
  req[HC.PkgReq.Chain]=HC.PkgReq.ChainOpt.Full;
  return req;
}
``` 

Or, if all that's needed is the headers of an entry types "foo" and "bar" the function would look like this:
```JavaScript
function validatePutPkg(entry_type) {
  var req = {};
  req[HC.PkgReq.Chain]=HC.PkgReq.ChainOpt.Headers;
  req[HC.PkgReq.EntryTypes]=["foo","bar"];
  return req;
}
```

**The validation functions are the heart of ensuring distributed data integrity. Please consider them thoroughly and carefully as your systems data integrity is built from them.**

Validation functions are called under two distinct conditions.

 1. When data is about to be added to the local chain (validateCommit).
 2. Whenever any node of the DHT receives a request to store or change data in some way, the request and the changes are validated against the source chain of the person making the request (validatePut,validateLink.) These are the validation functions that check permissions and enforce any other kind of data integrity you intend to preserve. For example, in a distributed Twitter, only Bob should be able to attach (link) a "post" to Bob as if it is his, and only Bob should be able to link Bob as a "follower" of Alice. _Please keep in mind that system variables like App.Agent.Hash that return your own address will not return YOUR address when being executed by a remote DHT node. Code your functions as if anyone will be running them, because they will be._

Common parameters:

- `<package>` the data package created by validateXPkg
- `<sources>` an array of the sources that created were involved in taking the action
