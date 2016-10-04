# JavaScript guidelines


* When returning or assining use ternaries.

```javascript
// bad
if (foo) {
  return getPosts({ id: foo })
} else {
  return showError('Invalid ID')
}

// good
return foo ? getPosts({ id: foo }) : showError('Invalid ID')

// even better
return foo
  ? getPosts({ id: foo })
  : showError('Invalid ID')
```

* Cache values that are used more than once

```javascript
// bad
function showPost (index) {
  getPost(posts[index].id)
  getAuthor(posts[index].id)
}

// worse
function saveMatchingPosts (id) {
  savePosts(posts.filter(post => post.id === id))
  closePosts(posts.filter(post => post.id === id))
}

// good
function showPost (index) {
  const postId = posts[index].id

  getPost(postId)
  getAuthor(postId)
}

function saveMatchingPosts (id) {
  const matchingPosts = posts.filter(post => post.id === id)

  savePosts(matchingPosts)
  closePosts(matchingPosts)
}
```

* In conditionals (AND / &&) prefer checking the most likely to be false in the first place. If that’s hard to estimate, prefer checks that are less complex (performance wise). This is because if the first check fails, the rest won’t be evaluated.

```javascript

// bad
if (complexCheckFunction(x) && simpleCheckFunction(x) && y) {
  // stuff
}

// good
if (y && simpleCheckFunction(x) && complexCheckFunction(x)) {
  // stuff
}
```

* Instead of for-loops try [Array methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) like map, reduce, filter.

```javascript
const array = [
  { key: 1, value: 'One' },
  { key: 2, value: 'Two' }
]

// bad
let obj = {}
for (var i = 0; i < array.length; i++) {
  if (array[i].key % 2 === 0) {
    obj[array[i].key] = array[i].value
  }
}

// good
const obj = array
  .filter(elem => elem.key % 2 === 0)
  .reduce((prev, curr) => {
    prev[curr.key] = curr.value
  }, {})
```

* Prefer the functional programming approach when possible.
  Examples:
  * Instead of mutating the state in each function, try returning new value (pure functions).

```javascript
// bad

if (foo) {
  setPosts(foo)
} else {
  clearPosts()
}

function setPosts (x) {
  this.posts = fetchPosts(x)
}

function clearPosts () {
  this.posts = fetchEmptyPosts()
}

// good
this.posts = foo
  ? getState(foo)
  : getClearPosts(bar)

function getState (x) {
  return getPosts(x)
}

function getClearPosts () {
  return fetchEmptyPosts()
}
```


<!-- * "classes are for designers", so don't scope with `ids` and `classes` in js. JavaScript developers should use [data-* attributes](http://roytomeij.com/2012/dont-use-class-names-to-find-HTML-elements-with-JS.html), [js-* prefix for classes](http://coderwall.com/p/qktuzw) or [role attribute](https://github.com/kossnocorp/role) (go to [discussion](https://github.com/monterail/rules/pull/4))
* prefix jQuery objects with `$` sign unless you are working with [angular project](http://angularjs.org/) (go to [discussion](https://github.com/monterail/rules/pull/10)) -->
* [Detect and inform about network issues](http://html5demos.com/offline-events#view-source), especially in SPA
* Prefer $(this) over $(@) in CoffeeScript.
* Try to avoid using [date.js](http://www.datejs.com/), use [moment.js](http://momentjs.com/) instead. datejs overwrites native methods and tends to break stuff.
* Use `camelCase` for variable and function names. The only exception to this rule are JSON properties which are `under_scored`.
* Use trailing commas in **multiline object literails** and **multiline arrays**. If you'd like to add an element you won't left your comma on the previous line. It helps to keep VCS history clean.
```javascript
// Bad          // Good         // Bad            // Good
let arr = [     let arr = [     let obj = {       let obj = {
  'foo',          'foo',          foo: 'foz',       foo: 'foz',
  'bar'           'bar',          bar: 'baz'        bar: 'baz',
];              ];              };                };
```

## JavaScript on Rails

* [use callbacks](https://gist.github.com/3019231) instead of low-level `$.ajax` when possible
* [move javascript tags](https://github.com/rails/rails/pull/7888) from `HEAD` to the bottom (go to [discussion](https://github.com/monterail/rules/pull/2))
