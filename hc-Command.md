
## Command Line Help

## hc --help
To get help from the command line, type ```$ hc --help```

You'll see this response:
```
NAME:
   hc - holochain peer command line interface

USAGE:
   hc [global options] command [command options] [arguments...]

VERSION:
   0.0.1

COMMANDS:
     gen, g     
     init, i    boostrap the holochain service
     dump, d    display a text dump of a chain
     test, t    run validation against test data for a chain in development
     serve, w   serve a web interface to access a chain
     status, s  display information about installed chains
     call, c    call an exposed function
     help, h    Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --verbose      verbose output
   --help, -h     show help
   --version, -v  print the version
```

## hc init
This command initializes the system with an identity and generates public/private keys for that identity in interacting with other peers on a holochain. You pass a single argument into init with the identifying information you'd like to attach to your public key record.

It takes the form: ``` hc init `"Fred Flintstone" fred@flintstone.com"' ```

## hc gen
If you have the DNA for an existing holochain you'd like to join. Copy the files to...  
Then type: ```hc gen from <source-file> <target-name>```


```hc gen dev <name>``` Developer mode

```hc gen chain <name>``` creates genesis entries


## hc status
To see what holochains are installed on your system, just type ```hc status```. You'll get a result showing each chain name and the the ID/hash of the DNA of the chain.

For example:
```
installed holochains:
     escrow <not-started>  
     flack 9yPX4cX3hA9DNkx6kNjdKRmPLdDZeJrPcWChpLv6X7PG
```

## hc test

``` hc test chat ```

## hc dump

``` hc dump chat ```

## hc call
