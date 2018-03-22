# STOMP.js

This library provides a STOMP client for Web browser (using Web Sockets) or node.js applications (either using raw TCP sockets or Web Sockets).

# Project Status

__This project is _no longer maintained_ ([some context about this decision](http://jmesnil.net/weblog/2015/09/04/stepping-out-from-personal-open-source-projects/)).__

__If you encounter bugs with it or need enhancements, you can fork it and modify it as the project is under the Apache License 2.0.__

## Web Browser support

The library file is located in `lib/stomp.js` (a minified version is available in `lib/stomp.min.js`).
It does not require any dependency (except WebSocket support from the browser or an alternative to WebSocket!)

Online [documentation][doc] describes the library API (including the [annotated source code][annotated]).

## node.js support

Install the 'stompjs' module

    $ npm install stompjs

In the node.js app, require the module with:

    var Stomp = require('stompjs');

To connect to a STOMP broker over a TCP socket, use the `Stomp.overTCP(host, port)` method:

    var client = Stomp.overTCP('localhost', 61613);

To connect to a STOMP broker over a WebSocket, use instead the `Stomp.overWS(url)` method:

    var client = Stomp.overWS('ws://localhost:61614');

## Development Requirements

For development (testing, building) the project requires node.js. This allows us to run tests without the browser continuously during development (see `cake watch`).

    $ npm install

## Building and Testing

[![Build Status](https://secure.travis-ci.org/jmesnil/stomp-websocket.png)](http://travis-ci.org/jmesnil/stomp-websocket)

To build JavaScript from the CoffeeScript source code:

    $ cake build

To run tests:

    $ cake test

To continuously run tests on file changes:

    $ cake watch


## Browser Tests

* Make sure you have a running STOMP broker which supports the WebSocket protocol
 (see the [documentation][doc])
* Open in your web browser the project's [test page](browsertests/index.html)
* Check all tests pass

## Use

The project contains examples for using stomp.js
to send and receive STOMP messages from a server directly in the Web Browser or in a WebWorker.

# About IE8, IE9 use WebSocket

IE8, IE9 not support native WebSocket, But you can use [web-socket-js](https://github.com/gimite/web-socket-js) to simulate native WebSocekt. web-socket-js is depend on Adobe Flash Player version >= 10.0.0, so your IE must have flash. 

Next you should set up a simple Flash Socket Policy Server on your WebSocket server 843 port, you can see the detail on [A PolyFill for WebSockets](http://old.briangonzalez.org/posts/websockets-polyfill)

otherwise you will see a error in browser console
```
make sure the server is running and Flash socket policy file is correctly placed
```

Flash socket policy file is very simple, you can see below.

```
<cross-domain-policy>
  <allow-access-from domain="*" to-ports="*" />
</cross-domain-policy>
```

Last But not least, Stomp in IE8 and IE9 have bug, the RegExp not normal。

`datas.split(RegExp('' + Byte.NULL + Byte.LF + '*'))` in chrome will return `['some message', '']`, but in IE8 or IE9, it well return `['some message']`, the emptye string is missing. as a result the connect_success_callback function will not be callback, so a add a '' to the  frames. Problem solved! it working, but l can't promise everything will be ok in the feature.

```
 Frame.unmarshall = function (datas) {
      var frame, frames, last_frame, r
      frames = datas.split(RegExp('' + Byte.NULL + Byte.LF + '*'))

      // fix ie8, ie9, RegExp not normal problem
      // in chrome the frames length will be 2, but in ie8, ie9, it well be 1
      // by wdd 20180321
      if (frames.length === 1) {
        frames.push('')
      }
```

## Authors

 * [Jeff Mesnil](http://jmesnil.net/)
 * [Jeff Lindsay](http://github.com/progrium)

[doc]: http://jmesnil.net/stomp-websocket/doc/
[annotated]: http://jmesnil.net/stomp-websocket/doc/stomp.html
