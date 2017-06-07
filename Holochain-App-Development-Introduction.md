# Holochain App Development

A `Holochain App` is a set of files that the `Holochain Core` uses to produce the desired behaviour. The `Core` provides all the guarantees users require from `Holochains`, whilst the `App` source code provides the specific behaviour of the `Holochain App`.

> currently the `holochain tools` are set up for development on a *nix / bash environment.

The development life-cycle of a holochain app:
1. Install the holochain developer tools
2. Chose a name and initialise a new Holochain App
3. Run unit tests and multi-node integration tests
4. Alter the source
5. rinse and repeat 3.
6. distribute your app

## Install the holochain developer tools
1. If you have not already:
    1. If you are on bash (nix / (maybe sygwin??)<br>
       Install [holochain system tools](Install-Holochain-on-nix)
    2. To do multinode testing, requires 1. above and:<br>
       Install docker and docker compose: [Docker Installation](Docker-Installation-for-Developers)

## Chose a name and initialise a new Holochain App
1. Let's choose `myHolochainApp` as the app name.
2. If you already have files in your app, then `init` will ask you to make sure you are in the correct directory
3. If you do not have files, `init` will ask you if you want to copy skeleton files from an existing example
4. `init` will ask you what name you want to give your Holochain App
5. `init` will show you all the choices you have made to confirm

    ```bash 
    $ mkdir myHolochainApp
    $ cd myHolochainApp
    $ holochain.app.init
    #### init will ask some questions

    #### example output after answering questions
    HC: All information entered. Check details: 
    
    HC:  copy from example: "chat"
    HC:      readable name: "myHolochainApp"
    Is this correct? (Y/n) Y
    # TYPE "Y"
    HC: copied example app from: chat
    HC: holochain app initialised
    ```
    > **What do I have?**
    > * Assuming you copied the "chat" example, you have dna, test and ui directories containing basic setup for a simple chat Holochain App. This is a suitable starting point for developing any new Holochain App.

    > **What did it do?**
    > * initialized your app directory with a `.hc` directory containing configuration details
    > * added some entries into `.gitignore` coherent with developing your App on top of a git repository

## Run unit tests and multi-node integration tests
1. To learn how to write tests, check the examples, and look at [App Testing ](App-Testing)
2. To run all tests and multi-node integration tests
    ```bash
    # from anywhere inside your App directory structure
    #   <scenarioName> has tab-completion
    #   the chat example app has one, called "backnforth"
    $ holochain.app.testScenario <scenarioName>
    ```
    > **What do I have?**
    > * each run of testScenario is logged in logs.holochain/cluster.log
    > * logs are in logs.holochain/testScenario.<scenarioName.log

    > **What did it do?**
    > * built your App from scratch, against the pinned version of the Holochain Core source code
    > * ran all your unit tests (*.json inside `/test` directory of App)
    > * started a local bootstrap server for your roles to discover each other
    > * each role defined in <scenarioName> gets its own `hc serve` instance
    > * syncronised the start of each `hc serve` instance within 100ms
    > * each `hc serve` behaves as defined in its .json file
    > * logs are in myHolochainApp/logs.holochain

## Alter your source
1. Make sure to check out the [Testing Harness Documentation](App-Testing)

## Rinse repeat 3.

## Distribute your app
//TODO