# Create a user
```bash
#password is administrator
ssh administrator@162.243.136.142
Scripts/createHolochainDevUser *username*

added user *username*
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
> * a new user called *username*
> * with the password *username*
> * home directory /home/*username*
> * containing a directory `holochain`
>   * which has a pull of the latest master of metacurrency/holochain

# begin being able to develop
```bash
# password is *username*
ssh <username>@162.243.136.142
#maybe change your password
passwd
<press enter and type your password>
#install holochain in your user
holochain/bin/holochain.system.install
```
# What happens now?
you have completed Step 1 of this document: https://github.com/metacurrency/holochain/wiki/Holochain-App-Development-Introduction

# For the development of source code files, you can log into your user using sftp
1. install `filezilla` https://filezilla-project.org/
2. add the server using these instructions
    * https://wiki.filezilla-project.org/FileZilla_Client_Tutorial_(en)
    * hostname: sftp://162.243.136.142
    * username: *username*
    * password: *username*  || your new password

3. https://www.ostraining.com/blog/webdesign/how-to-view-files-with-filezilla/