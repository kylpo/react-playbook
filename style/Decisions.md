Decisions Made
========

# React
## height={10} vs height='10px'
height={10}

### why
This defaults to px, which is the decided-on sizing unit for most cases. Also, React Native requires only a number, so may as well stay consistent between stacks.

# DOM
## margin-bottom or margin-top?
margin-top

### why
When viewing and building the site/app we go top down. It seems more logical that the first div on top (often a navbar) would not care that there is content below to be spaced. When the second div is added, it has surroundings to be aware of above, and should react according. I suppose this thinking is more component-centric.

#### more
[margin-bottom or margin-top | CSS-Tricks](https://css-tricks.com/margin-bottom-margin-top/?utm_source=frontendfocus&utm_medium=email)

## When to use margin vs padding?
- Use margin to separate the block from things outside it
- Use padding to move the contents away from the edges of the block
- it is all about concerns. Does the parent container care about its inner spacing? Or do the children care about their spacing?

# Perf
## Micro-perf: Prefer Booleans and Numbers where possible

### why
Strings are bigger to store, so garbage collection will be needed sooner.

```js
const ROBOT = 0;
const HUMAN = 1;
const SPIDER = 2;

let E_TYPE = {
  Robot: ROBOT,
  Human: HUMAN,
  Spider: SPIDER
};

// bad
// avoid uncached strings in heavy tasks (or better in general)
if (entity.type === "Robot") {

}

// good
// the compiler can resolve member expressions
// without much deepness pretty fast
if (entity.type === E_TYPE.Robot) {

}

// perfect
// right side of binary expression can even get unfold
if (entity.type === ROBOT) {

}
```

### Does this mean something like `typeof fun === 'function'` should be avoided?
No, `'function'` will be cached, and `typeof` is very fast

### A note on `undefined`
Use `someVar === undefined` over `typeof someVar === 'undefined'`, as it is less work for the compiler. See [this tweet](https://twitter.com/calebmer/status/845084035319779329).

#### more
[Writing efficient JavaScript – Medium](https://medium.com/@xilefmai/efficient-javascript-14a11651d563#.69425zp7f)

## Micro-perf: Try as much to avoid computations/vars in render
### Why
Because these vars are created and garbage collected soon after with each render (expensive)
```jsx
// BAD
render () {
  let make = `Make: ${this.props.make}`;

  return <div>{name}</div>;
}

// GOOD
render () {
  return <div>{`Make: ${this.props.make}`}</div>;
}

// GOOD
get make () {
  return `Make: ${this.props.make}`;
}

render () {
  return <div>{this.make}</div>;
}
```
from [react-bits/04.cached-vars-in-render.jsx](https://github.com/vasanthk/react-bits/blob/master/conventions/04.cached-vars-in-render.jsx)
