# Bluebird.Js-Cheat-Sheet
Bluebird is a fully-featured Promise library for JavaScript. The strongest feature of Bluebird is that it allows you to "promisify" other Node modules in order to use them asynchronously. Promisify is a concept applied to callback functions.

https://cheatsheetmaker.com/bluebirdjs

### Reference

<http://bluebirdjs.com/docs/api-reference.html>

### Generators
```
User.login = Promise.coroutine(function* (email, password) {
  let user = yield User.find({email: email}).fetch()
  return user
})﻿
```

See [Promise.coroutine](http://bluebirdjs.com/docs/api/promise.coroutine.html).

### Promise-returning methods
```
User.login = Promise.method((email, password) => {
  if (!valid)
    throw new Error("Email not valid")

  return /* promise */
})﻿
```

See [Promise.method](http://bluebirdjs.com/docs/api/promise.method.html).

### Node-style functions
```
var readFile = Promise.promisify(fs.readFile)
var fs = Promise.promisifyAll(require('fs'))﻿
```

See [Promisification](http://bluebirdjs.com/docs/api/promisification.html).

### Chain of promises
```
function getPhotos() {
  return Promise.try(() => {
    if (err) throw new Error("boo")
    return result
  })
}

getPhotos().then(···)﻿
```

Use [Promise.try](http://bluebirdjs.com/docs/api/promise.try.html).

### Object
```
Promise.props({
  photos: get('photos'),
  posts: get('posts')
})
.then(res => {
  res.photos
  res.posts
})﻿
```

Use [Promise.props](http://bluebirdjs.com/docs/api/promise.props.html).

### Multiple promises (array)
*   [Promise.all](http://bluebirdjs.com/docs/api/promise.all.html)(\[p\]) - expect all to pass
*   [Promise.some](http://bluebirdjs.com/docs/api/promise.some.html)(\[p\], count) - expect `count` to pass
*   [Promise.any](http://bluebirdjs.com/docs/api/promise.any.html)(\[p\]) - same as `some([p], 1)`
*   [Promise.race](http://bluebirdjs.com/docs/api/promise.race.html)(\[p\], count) - use `.any` instead
*   [Promise.map](http://bluebirdjs.com/docs/api/promise.map.html)(\[p\], fn, options) - supports concurrency

```
Promise.all([ promise1, promise2 ])
  .then(results => {
    results[0]
    results[1]
  })

// succeeds if one succeeds first
Promise.any(promises)
  .then(results => {
  })
```

```
Promise.map(urls, url => fetch(url))
  .then(···)

```

Use [Promise.map](http://bluebirdjs.com/docs/api/promise.map.html) to "promisify" a list of values.

### Multiple promises
```
Promise.join(
  getPictures(),
  getMessages(),
  getTweets(),
  function (pics, msgs, tweets) {
    return ···
  }
)﻿
```

Use [Promise.join](http://bluebirdjs.com/docs/api/promise.join.html)

### Multiple return values

```
.then(function () { return [ 'abc', 'def' ] }) 
```

Use [Promise.spread](http://bluebirdjs.com/docs/api/promise.spread.html)

### Example

```js
promise
  .then(okFn, errFn)
  .spread(okFn, errFn)        // *
  .catch(errFn)
  .catch(TypeError, errFn)    // *
  .finally(fn)
  .map(function (e) { ··· })  // *
  .each(function (e) { ··· }) // *
```

Those marked with `*` are non-standard Promise API that only work with Bluebird promises.
