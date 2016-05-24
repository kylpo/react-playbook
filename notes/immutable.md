```
*

  _   _       _            
 | \ | | ___ | |_ ___  ___
 |  \| |/ _ \| __/ _ \/ __|
 | |\  | (_) | ||  __/\__ \
 |_| \_|\___/ \__\___||___/


```

# Immutable

!!ACTIVELY BEING WRITTEN!!

## Overview

Used for equality checks - as easy as primitive checks like
```js
var x = 10
var y = 10
x == y   //true
x === y  //true
```

but for objects and arrays, instead!

This works perfectly with the `shouldComponentUpdate` component lifecycle hook in React.
  - See [PureRenderMixin](https://facebook.github.io/react/docs/pure-render-mixin.html)


Can be done __without__ libraries:
```js
Object.assign( {}, oldObj, {key: update} )
array.concat( [newItem] )
```

But this is slow and takes up memory

__Immutable.js__ is significantly faster, but does not have a native api like `Object.key()` or `obj[ 0 ]`
  - Must use obj.get( 'key' )
  - Also needs js-transit to serialize

Can use react-addons-update to "mutate" objs/arrays for parts of app that really need the perf

Watch [Render 2016 - Lee Byron](https://vimeo.com/album/3953264/video/166790294) for an overview of fully immutable front-end.

## Pros
Enables fast `shouldComponentUpdate`s and memoizable Selectors.

[Records](https://facebook.github.io/immutable-js/docs/#/Record) are a good match for your `/models` and work well with React's PropTypes.

Clearly shows App-components (has Immutable prop data) vs lib-components (no Immutable prop data)

## Cons
Your components will have to deal with immutable.js props. Especially for presentational components, using immutable's api may be undesirable.
