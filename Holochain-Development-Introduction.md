# Holochain Development

A `Holochain App` is a set of files that the `Holochain Core` uses to produce the desired behaviour. The `Core` provides all the guarantees users require from `Holochains`, whilst the `App` source code provides the specific behaviour of the `Holochain App`.

The development lifecycle of a holochain app:
1. download docker and the app skeleton
2. build developer image
3. run multi-node tests
4. alter the source
5. commit changes to git
6. rince and repeat 2.
7. distribute your app

## Download docker and the app skeleton
1. If you have not already, install docker and docker compose: [Docker Installation](Docker-Installation-for-Developers)

2. The holochain app skeleton can be found at https://github.com/metacurrency/holoSkel

    ```bash 
    $ #navigate to where you wanna be
    $ mkdir myHolochainApp
    $ cd myHolochainApp
    $ git clone https://github.com/metacurrency/holoSkel.git .
    ```
> What do I have?
> * These files contain the source code files for a simple chat app. This is a suitable starting point for developing a new holochain app.
> * Scripts for running, testing and distributing your holochain app.
  


There is a skeleton/example Holochain App at https://github.com/metacurrency/holoSkel.
The skeleton also contains scripts for testing your app. These scripts require Docker, and if you have Docker installed, you only ever need to worry about the source code for your app. Throughout the whole development lifecycle of your app, the holochain core will be inside docker containers, and you will never need to touch it.

## Running, Testing and Distributing your app
Holochain Apps can be run inside Docker containers in a production environment. There are always pros and cons however:
* `Holochain Apps` cannot produce unsecure behaviour on the host machine through "root exploits".
* Distribution of your `app` will not require reference to installation of the holochain `core`, as the docker build system will take care of this.
* In situations where users have `apps` which require different versions of the `core`, this will be hidden by the docker tools.

### Running the app
1. [Install Docker](Docker-Installation-for-Developers)
2. 
3. Pick a name for your new chat app. Lets call it myHolochainApp, and build the app.

    ```bash
    $ # build a docker image of the app, and give that docker image the tag "myHolochainApp"
    $ docker build -t myHolochainApp .
    ````
    > What does my image contain?
    The docker image created contains:
    * a small distribution of linux, designed for running the Go programming language, called "Alpine"
    * the Go programming language
    * all the Go libraries that the Holochain Core depends on
    * the Holochain Core
    * and finally, your myHolochainApp

    This is a developer image of your app. There are two more stages that are needed 
    In order for the docker image to be built, your app must pass all of its own tests
    

    $ # spin up a docker container from the image tagged "myHolochainApp"
    $ docker run -Pdt myHolochainApp
    ```


    * `-P` map all exposed ports onto random ports on the host
    * `-d` run the container in daemonised mode
    * `-t` the image we built, tagged as `clutter` in the line before

    by default, `hc` will serve your holochain on port 3141. Because docker is a container, this is always a good port to use, and docker with `-P` will manage passing through port 3141 to an open port on your host machine. to find out what port your holochain app has been mapped to, run:

    ```bash
    $ docker ps --latest
    ```

    ```bash
    dbb55a7828b7        clutter             "Scripts/chain.clo..."   6 minutes ago       Up 6 minutes        0.0.0.0:32934->3141/tcp   agitated_brahmagupta
    ```

    * This shows that our new container has our holochain app accessible through port `32934` on our host machine (e.g. `http://localhost:32934`)
    * we can also see that the container is called `agitated_brahmagupta` (docker names are good chat)
    * and that the first bunch of characters of the hash of the container are `dbb55a7828b7`

4. We can destroy this container instance using either the name or the hash of the container

    ```bash
    $ docker kill agitated_brahmagupta
    or
    $ docker kill dbb
    or
    $ docker kill agi
    ```

5. docker works very well in a development cycle. Each time you have editted code, and you want to test the new version, run `docker build...`, `docker run...`

6. TODO: If you have implemented tests for your holochain, you will see the output each time you call `docker run...`
    `hc test clutter` needs to be added to the script

7. Distributed apps need to be, well, distributed. Docker makes it very easy for you to test many instances of your app and how they interact with each other. To set up a cluster of nodes, use `docker-compose up` and then `docker-compose scale`

    ```bash
    $ docker-compose up
    $ docker-compose scale hc=2
    $ docker ps --last=2
    ```
    > this means:
    * `scale` how many instances of some service do you want to spin up
    * `hc` is the holochain service. Each one of these runs an instance of your holochain app
    * `scale hc=2` means: make it so there are 2 hc containers running. The number 2 can be replaced with however many instances you would like to have. Remember that to access them from the outside world, there must be a spare port on the host machine to connect to.

8. docker compose makes it easy to take down your containers, and rebuild the images:

    ```bash
    $ docker-compose down
    $ docker-compose build
    $ docker-compose up
    ```

9. TODO check out if docker-compose updates running containers if images are changed????

## The Bootstrap Server
hc instances use our holochain of holochains to self locate onto the network. The (admitedly rather simple!) output of this can be seen at http://bootstrap.holochain.net:10000