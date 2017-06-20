# Create a user
```bash
#password is administrator
ssh administrator@162.243.136.142
Scripts/createHolochainDevUser \<username\>

added user <username>
Cloning into 'holochain'...
remote: Counting objects: 4683, done.
remote: Compressing objects: 100% (169/169), done.
remote: Total 4683 (delta 159), reused 246 (delta 132), pack-reused 4382
Receiving objects: 100% (4683/4683), 1.56 MiB | 0 bytes/s, done.
Resolving deltas: 100% (2933/2933), done.
Checking connectivity... done.
cloned metacurrency/holochain
```
> **What do I have?**
> * a new user called <username>
> * with the password <username>
> * home directory /home/<username>
> * containing a directory `holochain`
>   * which has a pull of the latest master of metacurrency/holochain

# begin being able to develop
```bash
# password is <username>
ssh <username>@162.243.136.142
#maybe change your password
passwd
<press enter and type your password>
#install holochain in your user
holochain/bin/holochain.system.install
```
