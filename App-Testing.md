# Test Driven App Development

The holochain system includes a testing harness that allows you to test your application functions.  In addition we have build an integration harness that, using [docker-compose](https://docs.docker.com/compose/), simultaneously runs a set of tests in separate virtual nodes.  We strongly recommend using both these tools for application development.

### Test Files
A test file is essentially set collection of function calls into your application with specified inputs and expected outputs, or error values.  A test file consists of an array of json test objects of the form:

```go
[{
    Convey: string      // a human readable description of the tests intent
    Zome:   string      // the zome in which to find the function
    FnName: string      // the application function to call
    Input:  string      // the function's input
    Output: string      // the expected output to match against (full match)
    Err:    string      // the expected error to match against
    Regexp: string      // the expected out to match again (regular expression)
    Time:   int         // delay in millis before running this test (useful for multi-node testing)
},
{...}
]
```
### Notes:
- if `Time` is null or 0, tests are executed in the order discovered in the test file
  - in general the `Time` parameter is used to allow sufficient space for gossip operations between nodes on the DHT. 
  - in other words, to test a sequence of messages sent and received between holochain nodes, between 50 to 200 millis is required for message arrival.
    - role1.json >> "send message, Time = 0"
    - role2.json >> "check for message, Time = 100"
  - the 50 to 200 millis number works well within the test harness network. Tests crossing external networks may require much more time for messages to be delivered.

## Integrating tests into your holochain application
A holochain application directory (with the test directory expanded) should look something like this:

Directory structure:
- my_app
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
      - _config.json
    - scenario.myScenario.2
      - etc...

To run all the stand-alone tests (my_app/test/*.json), use this command:
    
    ```bash
    hc clone -force my_app my_app  && hc test my_app`
    ```
Note that all the test files with a .json extension in the test directory are automatically discovered and run.

Multi-node tests are created creating scenario (represented by a subdirectory in the `test` directory) with a number of roles, where each role represents one node in the multi-node test, and is defined by a test file in the scenario sub-directory.

To run the complete a mulit-node test of a particular scenario, use this command:

    ```bash
    holochain.app.testScenario <your-scenario-directory-name-here>
    ```

## Multi-node testing, how to construct a scenario

[Video showing tests being run](https://youtu.be/K1GPYY4imt0) (doesn't open new tab, maybe ctrl-click or w/e)

Each test/scenario sub-directory should contain one test file for each role required to model the test. The Script requires the name of the scenario directory [my_app/test/\<scenarioName\>] as a parameter. Test filenames are automatically discovered.

Tests pass or fail on the basis of the *content* of messages passed between roles/nodes on the network. In order to test the content of messages passed between roles, it is necessary for tests to account for the *amount of time* it takes for messages to travel between nodes on the network. This is achieved with the `Time` parameter of the test object. If roles 1 and 2 are called *back* and *forth* respectively, then when *forth* sends a message, *back* should wait at least 50ms for checking to see if there are messages that contain the expected content. If the message has not yet arrived, then the test will fail.

You can add a _config.json file to the scenario directory of the following format:
``` json
{
    "GossipInterval":500,
    "Duration":3
}
```
This allows you to set the gossip interval for your test, as well as a minimum duration that all the nodes should be kept running for the test to succeed, i.e. so other nodes that still have test pending can gossip with them.

In the chat application in the `examples` directory and you will find a [backnforth](https://github.com/metacurrency/holochain/tree/master/examples/chat/test/backnforth) scenario with two roles defined: person1 & person2.  Check out these examples to see how things work.

> TODO
> Note, it might make sense to implement both an automatic backnforth test generator, and also an asynchronous method of testing for the test message arrival (e.g. a message id)