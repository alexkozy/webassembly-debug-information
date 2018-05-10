# WebAssembly debug information

## Input
1. there are two type of clients: IDEs and static analysis tool;
1. different IDEs provide similar capabilities for different languages, e.g. stepping, inspecting current program state, breakpoints, e.t.c;
1. WebAssembly is going to support tremendous amount of different languages;

## Proposed architecture
![](https://raw.githubusercontent.com/ak239/webassembly-debug-information/master/overview.png?token=AAaBsp4XmbD-HUYXDuINSU58l8j-YMxUks5a_ZAiwA%3D%3D)
<br/>
As part of this proposal we need to specify two different APIs:
* WebAssembly runtime API exposed in raw terms without any knowledge about original language:
  * something for memory/current stack inspection,
  * something for basic debugging features like breakpoints and stepping,
  * something for compiled WebAssembly bytecode execution.
* API for IDEs:
  * sets of methods which IDE may use to provide debugging experience in terms of original language instead of low level WASM.
<br/>
Advantages of suggested approach:
1. to debug JavaScript most IDEs right now uses one of the exposed from browser [DevTools protocol](https://chromedevtools.github.io/devtools-protocol/tot/Runtime/),
