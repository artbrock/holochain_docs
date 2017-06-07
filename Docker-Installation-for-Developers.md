1. Install the latest version of Docker on your machine
    1. [Docker Installation](https://docs.docker.com/engine/installation/). The Community edition; stable is sufficient. 
    2. Whilst you will not need to know any of this to use holochain on docker, there are extensive instructions about Docker here [docker getting started](https://docs.docker.com/get-started/)
    3. On linux, it is recommended to add your user to the `docker` group as in: [Post Installation Steps](https://docs.docker.com/engine/installation/linux/linux-postinstall/), rather than use `sudo` before all script commands. Holochain Apps cannot exploit the kinds of security concerns mentioned in the Post Installation Steps document.


2. Confirm that docker installation and permissions are working by running:
```bash
$ docker info
```
3. Install the docker-compose version
```bash
curl -L https://github.com/docker/compose/releases/download/1.13.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose