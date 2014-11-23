[![Build Status](https://secure.travis-ci.org/node-inspector/v8-debug.png?branch=master)](http://travis-ci.org/node-inspector/v8-debug)

# v8-debug 
Provides extending API for [node](http://github.com/ry/node) internal debugger protocol (based on [v8 debugger protocol](https://code.google.com/p/v8/wiki/DebuggerProtocol))

This is a part of [node-inspector](http://github.com/node-inspector/node-inspector).

## Installation

    npm install v8-debug

## API

| Command | Params | Type | Description |
| :---: | :---: | :---: | :--- |
|register|||*Register new debug processor command like 'lookup'.*|
||name|**{String}**| *Name of command.*|
||callback|**{Function}**|*function(request, response) modify your response in this function.*|
|command|||*Call debug processor command like 'lookup'*|
||name|**{String}**| *Name of command.*|
||attributes|**{Object}**| *Extra parameters, that passes as command arguments.*|
||userdata|**{Object}**| *Data than needs to be stored, but can't be serialised before call processor callback.* (Not implemented now)|
|commandToEvent|||*Convert command response object to default event object with same name*|
||request|**{Object}**|*Request object created by debugger*|
||response|**{Object}**|*Response object that needs to be converted*|

## Usage

```js
var debug = require('v8-debug');

//register 'console' event in v8 debugger protocol
debug.register('console', function(request, response) {
  debug.commandToEvent(request, response);
});
//Now debugger can emit new event 'console'

console.log = (function(fn) {
  return function() {
    //Call 'console' command. (Emit console event)
    debug.command('console', {message: arguments[0]} /*, userdata*/);
    return fn.apply(console, arguments);
  }
} (console.log));
```
For more experience see [protocol documentation](https://github.com/buggerjs/bugger-v8-client/blob/master/PROTOCOL.md)
