
So what junk does holochain put on your computer and where does it put it?

## Linux
**Go:** When we have an automated holochain installer, this manual go installation process should disappear, but until then, you will need to [install the Go Language](http://golang.org/doc/install.html), including setting your $GOPATH, and then add $GOPATH/bin to your $PATH.

This should set up a directory for go programs at ```~/go``` and when you install the holochain program by typing ```go get github.com/metacurrency/holochain``` it will automatically install in subdirectories of the same name. It will also automatically grab other dependencies and install them in their respective paths under ```~/go```.

There will be a subdirectory for the [command line shell](hc-Command) for interacting with the holochain program at ```cmd/hc```. If you're an early stage user of holochain, you may need to update your hc command by changing to this subdirector and typing: ```go install```. This should rebuild the hc command and install it in the path for go executables at ```~/go/bin```.

**Holochain:** In your user directory a new (invisible) directory should have been created called ```.holochain```. You can go to it by typing ```cd ~/.holochain```

 Subdirectory for each holochain named with the chain name with these contents:

 Name | Description
 ----:|:----------
dna | This directory contains the DNA that defines your application
dna/dna.json | The DNA configuration file which specifies the application code (zome files) and holochain name and identifiers
dna/<zome_name>/ | Directory contains the zome specific application files. They will end in .zy if they're zygomys/Lisp files, .js if they're JavaScript, etc.
db | This directory contains chain and DHT data
db/chain.db | This is your local hashchain that you sign new entries to as you commit changes to your state in this holochain. 
db/dht.db | This is your node's DHT store.  It is a key-value store powered by ((Bundt))
ui  | This is the directory where you'll find user interface files (HTML, CSS, JavaScript, etc.) You can install additional UX/UI systems or components by dropping them into sub-directories here.
test | This directory contains your [application tests](App-Testing).

### Mac
Probably same as Linux... We'll need to check and update this. Can someone help us out here?  @haizop ? @matthewjosef ?

### Windows
We haven't set up a Windows installation and tried it yet... I think we'll need an installer which to throw our mutable files into the installing user's Virtual Store at:
```
%USERPROFILE%\AppData\Local\VirtualStore\MetaCurrency\Holochain\
```

Meanwhile you'll need to [install Go manually](http://golang.org/doc/install.html) and things will end up wherever Go puts programs.
