
So what junk does holochain put on your computer and where does it put it?

## Linux
**Go:** When we have an automated installer, this manual go installay should disappear, but until then, you will need to [install the Go Language](http://golang.org/doc/install.html), and [set your $GOPATH]().

This should set up a directory for go programs at ```~/go``` and when you install the holochain program by typing ```go get github.com/metacurrency/holochain``` it will automatically install in subdirectories of the same name. It will also automatically grab other dependencies and install them in their respective paths under ```~/go```.

hc

bin

**Holochain:** In your user directory a new (invisible) directory should have been created called ```.holochain```. You can go to it by typing ```cd ~/.holochain```

 Subdirectory for each holochain named with the chain name with these contents:
 | Name | Description |
 |------|-------------|
 | chain.db | This is your local hashchain that you sign new entries to as you commit changes to your state in this holochain. This is a key-value store powered by ((Bolt/Bundt)) |
| dna.conf | The DNA configuration file which specifies the application code (zome files) and holochain name and identifiers |
| zome_\<name> | These are the application files. They will end in .zy if they're zygomys/Lisp files, .js if they're JavaScript, etc. |
| ui  | This is the directory where you'll find user interface files (HTML, CSS, JavaScript, etc.) You can install additional UX/UI systems or components by dropping them into subdirectories here. |

### Mac
Don't know -- Probably same as Linux...

### Windows
We haven't set up a Windows installation yet and tried it yet... I think we'll need an installer which will probably throw stuff into the installing user's Virtual Store at:
```%USERPROFILE%\AppData\Local\VirtualStore\MetaCurrency\Holochain\```

Meanwhile you'll need to [install Go manually](http://golang.org/doc/install.html) and things will end up wherever Go puts programs.
