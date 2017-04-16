# Application DNA

Application DNA consists of the DNA file at the core in which a number of application properties, plus application code and entry types are specified.

Every Holochain app has a DNA file at its core.

These are the fields to be specified in the DNA file are as follows:

TODO: flesh out the descriptions of the fields.

## Version
An integer value describing the version of this DNA file, or, perhaps document this via a commented json example?

## Id
A UUID

## Name
A string value of the name of the application.

## RequiresVersion
An integer value that specifies the minimum usable version of the Holochain library for the app.

## Properties
A hashmap of app defined strings that are available to the app via the `property` function.  This is especially useful for applications that will be cloned and customized.

## HashType
A string describing the hash type to be used for this application.  Should be from the list of hash types from the [multihash library](http://multiformats.io/multihash/)

## Zomes
A list of Zome entries, where each zome is defined follows:

###	Name        string
###	Description string
###	Code        string // file name of DNA code
###	CodeHash    Hash
###	Entries     []EntryDef
####	Name       string
####	DataFormat string // "string" "json" or "links"
####	Schema     string // file name of schema or language schema directive
####	SchemaHash Hash
####	Sharing    string // "public" "private" or "shared"
####	validator  SchemaValidator
###	NucleusType string
###	Functions   []FunctionDef
####	Name        string
####	CallingType string // "string" or "json"

