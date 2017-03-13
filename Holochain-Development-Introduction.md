#Holochain Development

First we will take you through the development process using an example Holochain App DNA from our own repository, and then we will show you how to use a skeleton to start your own project.

##Installing the toolchain

1. Install the ***latest*** version of Docker Compose directly from [the docker website](https://docs.docker.com/compose/install/) - ( You may have to install Docker too)
2. Clone the `metacurrency/holoSkel` repository [from github](https://github.com/metacurrency/holoSkel

    ```bash 
    $ #navigate to where you wanna be
    $ mkdir myHolochainApp
    $ cd myHolochainApp
    $ git clone https://github.com/metacurrency/holoSkel.git .
    ```
3. This will create a skeleton holochain app for you in the current directory (`.`), and also downloads a set of example holochain apps in to `./examples`. `examples/clutter` is a simple decentralised messaging app built on Holochain. Our toolchain uses docker containers to remove the need for you to maintain the holochain software. To run an example of the clutter app:

    ```bash
    $ cd examples/clutter
    $ docker build -t clutter
    $ docker run -Pdt clutter
    ```
    > this means `docker run`:
    * `-P` map all exposed ports onto random ports on the host
    * `-d` run the container in daemonised mode
    * `-t` the image we built, tagged as `clutter` in the line before

    by default, `hc` will serve your holochain on port 3141. Because docker is a container, this is always a good port to use, and docker with `-P` will manage passing through port 3141 to an open port on your host machine. to find out what port your holochain app has been mapped to, run:

    ```bash
    $ docker ps --latest
    ````

All you need to do is to code your holochain DNA, and run `docker-compose up`.

    ```bash
    $ cd examples/clutter
    $ docker-compose up
    ````
