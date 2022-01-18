# Map proxy

## Description

JavaScript proxy objects don't work with maps because they have native properties that are not proxied.
This library make them work.

## Usage

```javascript

const { mapHandler } = require('map-proxy')

const handler = {
  get: (target, prop, receiver) => {
    if (prop === 'foo') {
      return 'bar'
    }
    return Reflect.get(target, prop, receiver)
  }
}
const newHandler = mapHandler(handler)

const nativeProxy = new Proxy(new Map(), handler)
const mapProxy = new Proxy(new Map(), newHandler)

console.log(nativeProxy.foo)
// => bar
nativeProxy.forEach(() => {})
// => throws
//    Uncaught TypeError: Method Map.prototype.forEach called on incompatible receiver #<Map>
//        at Proxy.forEach (<anonymous>)

console.log(mapProxy.foo)
// => bar
mapProxy.forEach(() => {})
// okay


```

## API

### mapHandler(handler)

The input is a [proxy handler](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy).

Returns a new proxy handler which wraps the input in a handler which make proxies work with maps.

