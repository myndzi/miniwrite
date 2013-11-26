# miniwrite

[![Build Status](https://secure.travis-ci.org/Bartvds/miniwrite.png?branch=master)](http://travis-ci.org/Bartvds/miniwrite) [![Dependency Status](https://gemnasium.com/Bartvds/miniwrite.png)](https://gemnasium.com/Bartvds/miniwrite) [![NPM version](https://badge.fury.io/js/miniwrite.png)](http://badge.fury.io/js/miniwrite)

> Minimal output-stream writer API. 

A pluggable output writer interface to embed/expose in tools and reporters. 

## API

Main usage:
````js
// simplified node.js.browser compatible console.log()
var mw = miniwrite.console();

// write plain text line
mw.writeln('hello world!');
````

## Helpers

Buffer writes:
````js
// buffer own lines
var mw = miniwrite.buffer();
// buffer other writes (handy for testing)
var mw = miniwrite.buffer(myMiniWrite);

// get buffer
var str = mw.concat();
var str = mw.concat('\n\n', '\t');
// iterate buffer if you must
mw.lines.forEach(function(line) {
	//..
})
// reset buffer
mw.clear();
````

Adapter to buffer character writes via `mw.write(chars)`, auto-flushes on newlines:
````js
var mw = miniwrite.chars(miniwrite.console());

// write plain text line
mw.write('hello');
mw.write(' ');
mw.writeln('world!');

mw.write('one');
mw.write('two');
mw.flush(true);
````

Spread of multiple writers
````js
var mw = miniwrite.multi([myANSIConsole, myRemoteSocket, myDiskLogger]);
mw.enabled = true;
mw.targets.forEach(function(subw, num) {
	//.. 
});
````

Proxy to toggle stream or swap output target:
````js
var mw = miniwrite.peek(myMiniWrite, callback);
mw.enabled = true;
mw.target = myOherWrite;
mw.callback = function(line) // string ,or false to ignore
````

Proxy to toggle stream or swap output target:
````js
var mw = miniwrite.proxy(myMiniWrite);
mw.enabled = true;
mw.target = myOherWrite;
mw.target = myOherWrite;
````

Convenience preset for [grunt](https://github.com/gruntjs/grunt) (same as in `grunt ~0.4.1`):
````js
var mw = miniwrite.grunt(grunt);
````
## Examples

Custom output-stream:
````js
var mw = {};
mw.writeln = function(line) {
	myWebSocketHyperStream.send({line: line})
};
// pass to supporting tools
awesomeModule.useMiniWritePlz(mw);
````

Tap into output
````js
awesomeModule.writer = miniwrite.splitter([awesomeModule.writer, myMiniWrite]);
````

## Installation

Not yet published to package managers. 

Link to a github commit if you feel adventurous.

```shell
$ npm install miniwrite --save-dev
```

## History

* 0.1.0 - Extracted styling to [ministyle](https://github.com/Bartvds/ministyle).
* 0.0.1 - Extracted code from [tv4-reporter](https://github.com/Bartvds/tv4-reporter) et al.

## Build

Nothing much here as the code is still being hammered out.

~~Install development dependencies in your git checkout:~~

    $ npm install

~~Build and run tests:~~

    $ grunt

See the `Gruntfile.js` for additional commands.

## Contributing

In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

*Note:* this is an opinionated module: please create a [ticket](https://github.com/Bartvds/miniwrite/issues) to discuss any big ideas. Pull requests for bug fixes are of course always welcome. 

## License

Copyright (c) 2013 Bart van der Schoor

Licensed under the MIT license.