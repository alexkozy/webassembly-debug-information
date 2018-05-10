# WebAssembly debug information

## Input
1. there are two type of clients: IDEs and static analysis tool;
1. different IDEs provide similar capabilities for different languages, e.g. stepping, inspecting current program state, breakpoints, e.t.c;
1. WebAssembly is going to support tremendous amount of different languages;

## Proposed architecture
![overview](https://raw.githubusercontent.com/ak239/webassembly-debug-information/master/overview.png?token=AAaBsp4XmbD-HUYXDuINSU58l8j-YMxUks5a_ZAiwA%3D%3D)
<br/>
<br/>
As part of this proposal we need to specify two different APIs:
* WebAssembly runtime API exposed in raw terms without any knowledge about original language:
  * something for memory/current stack inspection,
  * something for basic debugging features like breakpoints and stepping,
  * something for compiled WebAssembly bytecode execution.
* API for IDEs:
  * sets of methods which IDE may use to provide debugging experience in terms of original language.
<br/>
<br/>
### Advantages
1. In modern ecosystem a lot of IDEs (e.g. VSCode, WebStorm) use DevTools protocol (mostly Runtime and Debugger domains) to debug JavaScript, so adding support for new WebAssembly domain should be easy for them.
1. It is not required to specify debug information format which will support all existing languages and will be extensible enough to support future languages, with this proposal C/C++ may use something on top of DWARF, JVM languages may prefer some other format. WASM should only have some kind of reference implementation that other developers may use as an example.
1. More opportunities for compiler developers, they may output debug information in form of one of the existing format, e.g. SourceMap or DWARF and just reuse existing adapter from this format to IDE API, or they can introduce completely new format and write own adapter or introduce dynamic format which will expose IDE API right away.
1. Debug information service:
	* may require connection to WASM runtime, e.g. to implement evaluation of expressions in original language;
	* may not require connection to WASM runtime, e.g. if it is implemented on top of SourceMap where all information is static;
	* may be hybrid when all required information is embedded into format and we can evaluate something but do not need separate connection to WebAssembly format, e.g. DWARF.

### Disadvantages
1. If we allow service to have connection to WASM runtime then some part of services can be used as part of static analysis tool and some of them can not.

## Action items
1. describe basic WebAssembly runtime API in form of another DevTools protocol domain
1. describe possible IDE API based on most common IDE capabilities, describe how IDE can use this API (at least consider extension for [Language Service Protocol](https://microsoft.github.io/language-server-protocol/))
1. implements static debug information service on top of SourceMapV3
1. implements dynamic debug information service on top of DWARF for C
