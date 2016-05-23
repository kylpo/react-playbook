```
*

  _   _       _            
 | \ | | ___ | |_ ___  ___
 |  \| |/ _ \| __/ _ \/ __|
 | |\  | (_) | ||  __/\__ \
 |_| \_|\___/ \__\___||___/


```

# Immutable
- Used for equality checks
  - As easy as primitive checks:
```js
var x = 10
var y = 10
x == y   //true
x === y  //true
```
    but for objects and arrays, instead!
  - This works perfectly with the `shouldComponentUpdate` component lifecycle hook in React.
    - See [PureRenderMixin](https://facebook.github.io/react/docs/pure-render-mixin.html)

- Can be done __without__ libraries:
```js
Object.assign( {}, oldObj, {key: update} )
array.concat( [newItem] )
```
  But this is slow and takes up memory
- __Immutable.js__ is significantly faster, but does not have a native api like `Object.key()` or `obj[ 0 ]`
  - Must use obj.get( 'key' )
  - Also needs js-transit to serialize
- Consider: How should your components deal with immutable.js props? Especially when considering presentational components, using immutable's api may be undesirable.
- Can use react-addons-update to "mutate" objs/arrays for parts of app that really need the perf
