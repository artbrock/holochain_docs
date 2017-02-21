# Technical Architecture
[White paper link](https://ceptr.org/projects/whitepapers/)
Okay, maybe everybody thinks their software is a special and unique snowflake and you've never seen anything like it before. This time, it's for real.

Hopefully, that previous sentence will be made obsolete soon, but until then, please consider that Holochain doesn't work like anything you've used before, and you cannot think about it like a blockchain. It is necessary to get your bearings on:
 - what the system components are,
 - how the distinct parts inter-operate as a unified whole
 - and the strange jargon we use for various elements.

> **Jargon Disclaimer:** Apologies in advance for the unfamiliar language used for system components. To avoid confusion, we didn't want to call things familiar words, when they don't behave in the expected familiar ways. So [we used new words](Glossary), frequently drawing from biological counterparts when available.

For example: Once launched the code that runs applications in a Holochain is immutable without forking the chain. This code is the **DNA** specific to that holochain. Therefore the code is hashed into the first entry of each participant's chain

### Architecture Overview
Three Subsystems -- Two Modes
![Holochain Sub-Systems](http://ceptr.org/images/Holochain_Subsystems.png)

#### 1. The local signed chain:
#### 2. The shared DHT:
#### 3. End-User Applications:

## Developer Resources
How the heck do you start building an app to run on holochains?

### Data schemas
Hmmm...

### Validation Rules
Scripted in [Zygomys](https://github.com/glycerine/zygomys)

### Sockets for Web UI
For now we expect to run these via a web browser UI.
