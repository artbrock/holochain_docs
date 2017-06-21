There is a free to use developer space for Holochain Apps, which runs linux ubuntu and which satisfies all the various requirements of running and developing holochain

This space is not secure, nor can we guarantee any files kept there. 

Always keep copies of your files somewhere else (e.g. github)

# Create a user
```bash
#password is administrator
ssh administrator@162.243.136.142
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-64-generic x86_64)


$ Scripts/createHolochainDevUser *username*
[sudo] password for administrator: 

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

# To develop your Holochain App files
Use the above document to create your base holochain app
1. install `filezilla` https://filezilla-project.org/
2. add the server using these instructions
    * https://wiki.filezilla-project.org/FileZilla_Client_Tutorial_(en)
    * hostname: sftp://162.243.136.142
    * username: *username*
    * password: *username*  || your new password

3. https://www.ostraining.com/blog/webdesign/how-to-view-files-with-filezilla/