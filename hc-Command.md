
## Command Line Help

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
If you have an existing holochain you'd like to join. Copy the files...
```hc gen from <source>```


```hc gen dev``` Developer mode

## hc status

returns: 
```
installed holochains:                                               
     chat <not-started>  
```

## hc test

``` hc test chat ```

## hc dump

``` hc dump chat ```

## hc call

