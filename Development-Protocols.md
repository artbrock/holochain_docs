## Social Protocols
We are committed to foster a vibrant thriving community, including growing a culture that breaks cycles of marginalization and dominance behavior. In support of this, some open source communities adopt [Codes of Conduct](http://contributor-covenant.org/version/1/3/0/).  We are still working on our social protocols, and empower each team to describe its own *Protocols for Inclusion*.  Until our teams have published their guidelines, please use the link above as a general guideline.

## Coordination

* For task management we use [Waffle](https://waffle.io/metacurrency/holochain) or for the non-kan-ban view [github's issues](https://github.com/metacurrency/holochain/issues)
* All tickets should be "bite-sized" i.e. no more than a week's worth of coding work. Larger tasks are represented in [Milestones](https://github.com/metacurrency/holochain/milestones?direction=asc&sort=due_date&state=all).
* Chat with us on [Gitter](https://gitter.im/metacurrency/holochain) or [Slack](http://ceptr.org/slack)
* We have a weekly [dev-coord hangout](http://ceptr.org/devchat) on Tuesday's 9am PST/ 12pm EST 

## Test Driven Development
We use **test driven development**. When you add a new function or feature, be sure to add the tests that make sure it works.  Pull requests without tests will most-likely not be accepted!

## Code Formatting Conventions
All Go code must be formatted with [gofmt](https://blog.golang.org/go-fmt-your-code).
To make this easier consider using a [git-hook](https://gist.github.com/timotree3/d69b0fb90c8affbd705765abeabc489d#file-pre-commit) or configuring your editor with one of these: ([Emacs][], [vim][], [Sublime][], [Eclipse][])

[Emacs]: https://github.com/dominikh/go-mode.el
[vim]: https://github.com/fatih/vim-go
[Sublime]: https://github.com/DisposaBoy/GoSublime
[Eclipse]: https://github.com/GoClipse/goclipse

For Atom, you could try this [package](https://atom.io/packages/save-commands) but it requires some configuration.