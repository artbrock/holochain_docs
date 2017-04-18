# Test Driven App Development

## Test file format
Each test file consists of an array of json objects of the form:

    ``` go
    {
	Convey: string      // a human readable description of the tests intent
	Zome:   string      // the zome in which to find the function
	FnName: string      // the function to call
	Input:  string      // the function's input
	Output: string      // the expected output to match against (full match)
	Err:    string      // the expected error to match against
	Regexp: string      // the expected out to match again (regular expression)
	Time:   int         // delay in millis before running this test
    }
    ```
    
- if `Time` is null or 0, tests are executed in the order discovered in the test file
  - in general the `Time` parameter is used to allow sufficient space for gossip operations between nodes on the DHT. 
  - in other words, to test a sequence of messages sent and received between holochain servers, between 50 to 200 millis is required for message arrival.
    - role1.json >> "send message, Time = 0"
    - role2.json >> "check for message, Time = 100"
  - the 50 to 200 millis number works well within the test harness network. tests crossing external networks may require much more time for messages to be delivered.


## Integrating tests into your holochain application
The holochain application skeleton can be found at [https://github.com/metacurrency/holoSkel](https://github.com/metacurrency/holoSkel)

directory structure:
- repo_root
  - [dna](DNA-Reference)
  - [ui](UI-Reference)
  - test
    - singleNodeTest.1.json
    - singleNodeTest.2.json
    - scenario.myScenario.1
      - role1.json
      - role2.json
      - ...
      - roleN.json
    - scenario.myScenario.2
      - etc...

- test files with a .json extension are automatically discovered
- to run all the singleNodeTests, repo_root/test/*.json, use this command:
    
    ```bash
    hc clone -force my_app my_app  && hc test my_app`
    ```
- to synchronously run the complete suite of role's from a particular scenario, use this command:

    ```bash
    repo_root/Scripts/testScenario <scenarioName>
    ```

## Multi-node testing, how to construct a scenario

[Demo Video](https://youtu.be/K1GPYY4imt0)

Each test/scenario sub-directory should contain one test file for each role required to model the test. Filenames are automatically discovered.

Tests pass or fail on the basis of the *content* of messages passed between roles/nodes on in the network. In order to test the content of messages passed between roles, it is necessary for tests to account for the *amount of time* it takes for messages to travel between nodes on the network. This is achived with the `Time` parameter of the test object. If roles 1 and 2 are called *back* and *forth* respectively, then when *forth* sends a message, *back* should wait at least 50ms for checking to see if there are messages that contain the expected content. If the message has not yet arrived, then the test will fail.

Note, it might make sense to implment both an automatic backnforth test generator, and also an asynchronous method of testing for the test arrival (e.g. a message id)

You can add a _config.json file to the scenario directory of the following format:
``` json
{
    "GossipInterval":500,
    "Duration":3
}
```
This allows you to set the gossip interval for your test, as well as a minimum duration that all the nodes should be kept running for the test to succeed, i.e. so other nodes that still have test pending can gossip with them.

When you clone the repo, you will see that it comes with our example chat app, and you will find a "backnforth" scenario with two roles defined: person1 & person2.  Check out these examples to see how things work.

