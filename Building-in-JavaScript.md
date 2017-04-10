You can write a Holochain application using Otto JavaScript for which we run a scripting Nucleus in the Holochain Go code.

Most of the Otto flavor of the JavaScript language is described on the [project's README page](https://github.com/robertkrimen/otto). Please note this is NOT a full implementation of JavaScript, and is missing a number of things you may be used to having, such as timer functions or access to a DOM. Because your app runs inside a scripting shell inside a Go program, most of those things don't make any sense in this context anyway.

Additional Holochain specific commands can be found in the [App Development page](App-Development-API).