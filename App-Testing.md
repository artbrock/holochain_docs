# Test Driven App Development

We have provided a testing harness for test-driven holochain application development.

A single test consists of a json object of the form:

``` go
{
	Convey: string      // a human readable description of the tests intent
	Zome:   string      // the zome in which to find the function
	FnName: string      // the function to call
	Input:  string      // the function's input
	Output: string      // the expected output to match against (full match)
	Err:    string      // the expected error to match against
	Regexp: string      // the expected out to match again (regular expression)
	Time:   int         // offset in milliseconds from the start of the test at which to run this test.
}
```

Test files (which live in the `test` [directory](File-Locations)) consist of an array of test which get executed in the order they appear in the file, unless the test has a non-zero `Time` value, in which case it is queued for executing at the specified time (in milliseconds as measured from the beginning of the when the test started.)


## Single node testing
1. Code you application (let's assume it lives in the directory: `my_app`)
2. Write your tests and place them into the `my_app/test` directory
3. Clone and run your app tests with `hc clone -force my_app my_app  & hc test my_app`

## Multi-node testing
We have created a docker cluster based testing-harness for muilt-node application testing. To use this framework for app development first clone the [holoSkel repo](https://github.com/metacurrency/holoSkel).  Consider that repo the root of your application, and replace the `dna`, and `ui` directories with your own application.
In the `test` directory you can also add your single node tests, but more importantly you need to add scenario sub-directories for a seperate multi-node test.  Each sub-directory should contain test files that will define the actions taken by the different nodes which get run simultaneously by the testing harness in separate docker instances.  You should think of each of these as roles in the scenario.  To run a scenario test simply type: `./Scripts/testScenario <scenario-name>`

To write mult-node tests, you must take into account timing.  The tests on the different nodes all start at relatively the same time, so you establish back & forth behavior by setting a time (in ms from the start) for the test to execute in the `Time` value.  You can add a _config.json file to the scenario directory of the following format:
``` json
{
    "GossipInterval":500,
    "Duration":3
}
```
This allows you to set the gossip interval for your test, as well as a minimum duration that all the nodes should be kept running for the test to succeed, i.e. so other nodes that still have test pending can gossip with them.

When you clone the repo, you will see that it comes with our example chat app, and you will find a "backnforth" scenario with two roles defined: person1 & person2.  Check out these examples to see how things work.

