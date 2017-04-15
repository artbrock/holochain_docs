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
  - in other words, in a sequence tests of messages sent and received between holochain servers, between 50 to 200 millis is required for message arrival.
  - the 50 to 200 millis number works well within the test harness. tests covering external networks may require much more time, or some other mechanism

## integrating tests into your holochain application

the holochain application skeleton can be found at [https://github.com/metacurrency/holoSkel](https://github.com/metacurrency/holoSkel)

directory structure:
- repo_root
  - dna
    - [dna specification](link-to-dna-specification)
  - ui
    - [ui specification](link-to-ui-specification)
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

- tests scenarios and roles may have any names, as long as the test files have the .json extension
- singleNodeTests can be run with 
    ```hc clone -force my_app my_app  && hc test my_app`
    ```

## Multi-node testing

Each sub-directory should contain test files that will define the actions taken by the different nodes which get run simultaneously by the testing harness in separate docker instances.  You should think of each of these as roles in the scenario.  To run a scenario test simply type: `./Scripts/testScenario <scenario-name>`

To write mult-node tests, you must take into account timing.  The tests on the different nodes all start at relatively the same time, so you establish back & forth behavior by setting a time (in ms from the start) for the test to execute in the `Time` value.  You can add a _config.json file to the scenario directory of the following format:
``` json
{
    "GossipInterval":500,
    "Duration":3
}
```
This allows you to set the gossip interval for your test, as well as a minimum duration that all the nodes should be kept running for the test to succeed, i.e. so other nodes that still have test pending can gossip with them.

When you clone the repo, you will see that it comes with our example chat app, and you will find a "backnforth" scenario with two roles defined: person1 & person2.  Check out these examples to see how things work.

