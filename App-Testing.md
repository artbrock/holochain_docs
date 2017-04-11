# Test Driven App Development

We have provided a testing harness for test-driven holochain application development.

A single test consists of:

```
type TestData struct {
	Convey string      // a human readable description of the tests intent
	Zome   string      // the zome in which to find the function
	FnName string      // the function to call
	Input  interface{} // the function's input
	Output string      // the expected output to match against (full match)
	Err    string      // the expected error to match against
	Regexp string      // the expected out to match again (regular expression)
	Time   int         // offset in milliseconds from the start of the test at which to run this test.
}
```

Test files consist of an array of TestData which get executed in the order they appear in the file, unless the test has a non-zero `Time` value, in which case it is queued for executing at the specified time (in milliseconds as measured from the beginning of the when the test started.


## Single node testing
TODO: document testing files and process

## Multi-node testing
TODO: document multi-instance docker testing process here.