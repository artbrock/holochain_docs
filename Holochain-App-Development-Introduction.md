# Holochain App Development

Holochain applications are

The development life-cycle of a Holochain app:
1. Install the Holochain developer tools
2. Chose a name and initialize a new Holochain App
3. Create and run unit tests and multi-node integration tests
4. Write application source code to do what the test's say!
5. rinse and repeat from 3.
6. distribute your app

## Install the holochain developer tools
Currently the `holochain tools` are a collection of bash scripts that automate important dev tasks.  These bash scripts can be found in the `bin` directory of repo.  One of these scripts will also "install" them by automatically adding that directory to your bash path.  Here's how, first `cd` to where you cloned the holochain repo.  Then:

```bash
./bin/holochain.system.install
source ~/.bashrc
```
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

    > **What now?**
    > * add your files to a git repository and push to some remove server (e.g. github)
    > 

## Play with your app or Run unit tests and multi-node integration tests
> TODO: play with app
### Run Tests
1. To learn how to write tests, check the `test` directories in the [examples](../../tree/master/examples), and look at [App Testing ](App-Testing)
2. Pre-requisites:
  - make sure you have [installed docker and docker-compose](Docker-Installation-for-Developers).
  - make sure to build a docker image for scenario testing with:
   ```bash
   $ holochain.system.buildImageForAppTests
   ```
3. To run all tests and multi-node integration tests
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

## Write application source code
1. Make sure to check out the [Testing Harness Documentation](App-Testing)

## Rinse repeat 3.

## Distribute your app
//TODO