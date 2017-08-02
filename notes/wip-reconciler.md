# Reconciler
This is primarily about React's virtual DOM, and how it performs updates

## Foundation
Understand that React works with a single, big, deeply nested JSON object of React Elements.

```js
{
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
}
```

Primitive types, ones that map one-to-one with your DOM elements, have a string `type`. These are your `div`s, `span`s, `h1`s, etc. React's lifecycle hooks are not applied to these, the React Element is simply an immutable snapshot of the attributes of a DOM element, and React simply updates the DOM where they have changed, or does not update them at all if they do not change.

Note: maybe not say React Element, maybe `vdom` is better, as [WTF is JSX](https://jasonformat.com/wtf-is-jsx/) approaches this.

You could return the object representation (shown above) of your react elements in you react components, if you'd like:

```jsx
render() {
  return (
    {{"$$typeof":Symbol.for("react.element"), "type":"div","key":null,"ref":null,"props":{"children":[
      {{"$$typeof":Symbol.for("react.element"), "type":"div","key":null,"ref":null,"props":{"className":"a className"},"_owner":null}}

      {{"$$typeof":Symbol.for("react.element"), "type":"span","key":null,"ref":null,"props":{"something":true},"_owner":null}}
    ]},"_owner":null}}
  )
}
```

would map to `html` like:

```jsx
<div>
  <div class="a className"/>
  <span/>
</div>
```

And this would be as performant as possible, but it would be tedious to write. So, we have a helper function to generate and return these objects: `React.createElement()`.

```jsx
render() {
  return (
    React.createElement('div', null, [
      React.createElement('div', {className: "a className"})
      React.createElement('span', {something: true})
    ])
  )
}
```

## Perf problems of `React.createElement()`
It is a function that performs logic and returns an object. Performing any logic is slower than just having the object itself.

Fortunately, there are some babel plugins to help us out.

### [transform-react-constant-elements](https://babeljs.io/docs/plugins/transform-react-constant-elements/)
Use variables to store react elements.

"transform hoists element creation to the top level for subtrees that are fully static, which reduces calls to React.createElement and the resulting allocations" - [compiler optimizations](https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html#compiler-optimizations)

This transform hoists elements out of your `return` and places it in a `var` in your module. The hoisted `var` only runs `React.createElement()` once to generate the element's object, and the object is used as a constant in the `render`--saving `React.createElement()` calls in all subsequent `render`s.


```jsx
class MyComponent extends React.Component {
  render() {
    return (
      React.createElement('div', null, [
        React.createElement('div', {className: "a className"})
        React.createElement('span', {something: this.state.dynamicValue})
      ])
    )
  }
}
```

becomes

```jsx
// only called once
const hoistedElement = React.createElement('div', {className: "a className"})
// hoistedElement is now {{"$$typeof":Symbol.for("react.element"), "type":"span","key":null,"ref":null,"props":{"className":"a className","_owner":null}}

class MyComponent extends React.Component {
  render() {
    return (
      React.createElement('div', null, [
        {hoistedElement}
        React.createElement('span', {something: this.state.dynamicValue})
      ])
    )
  }
}
```

Note: not all elements are `transform-react-constant-elements`-able. See this [before](https://github.com/kylpo/babel-exploration/blob/master/2-after-emotion.js) and [after](https://github.com/kylpo/babel-exploration/blob/master/3a-after-constant.js).

De-opts: `ref` prop, `templateLiteral` in prop value, `variable` in prop value, and any children with any of these.

### [transform-react-inline-elements](https://babeljs.io/docs/plugins/transform-react-inline-elements/)
We've already established that `React.createElement()` trades convenience for performance, and that writing the objects by hand trades performance for convenience. This transform gives us the best of both worlds.

With it, we can use `React.createElement()`, then the transform will convert it to the resulting object (e.g. `{{"$$typeof":Symbol.for("react.element"), "type":"span","key":null,"ref":null,"props":null,"_owner":null}}`). This eliminates the `React.createElement()` call completely.

```jsx
class MyComponent extends React.Component {
  render() {
    return (
      React.createElement('div', null)
    )
  }
}
```

becomes

```jsx
class MyComponent extends React.Component {
  render() {
    return {"$$typeof":Symbol.for("react.element"), "type":"span","key":null,"ref":null,"props":{"className":"a className","_owner":null}
  }
}
```

This was the original intent of the transform, I think, but in practice today, it actually just converts `React.createElement()` to `BabelHelpers.jsx()`, which is a more optimized version for product. Specifically, it avoids a `for` loop mapping over props.

Note: not all elements can benefit from this transform. See this [before](https://github.com/kylpo/babel-exploration/blob/master/2-after-emotion.js) and [after](https://github.com/kylpo/babel-exploration/blob/master/3b-after-inline.js).

De-opts: `ref` prop, spreading `{...props}`.

### Powerful combo
`inline` reduces code execution by subverting `React.createElement`, but still creates new object literals on every re-render.

`constant` reduces re-render code execution by storing a single resultant React Element object literal, then referring to it in the `render()`.

## Where does JSX fit in to this?
`JSX` is what you're likely used to using, but it is really just sugar on top of `React.createElement()`. It relies on a babel transform to convert invalid javascript code into valid `React.createElement()` code. `JSX` represents objects (specifically React Element objects).

```jsx
render() {
  return (
    React.createElement('div', null, [
      React.createElement('div', {className: "a className"})
      React.createElement('span', {something: true})
    ])
  )
}
```

becomes

```jsx
render() {
  return (
    <div>
      <div className="a className" />
      <span something />
    </div>
  )
}
```

## Where do non-primitive elements fit in to this?
TODO [Components and Props - React](https://facebook.github.io/react/docs/components-and-props.html)

## Stateless Functional Component optimization
First, read this excellent article: [45% Faster React Functional Components, Now](https://medium.com/missive-app/45-faster-react-functional-components-now-3509a668e69f)

So, when extracting a set of components into a new subcomponent, consider using the Functional form if it doesn't need React's lifecycle hooks or state.

Note for self: this is made so much easier with Preact's `render(props)` API. Refactoring to/from SFC to Class is much easier.

#### Extra Credit
TODO: use naming convention and babel transform to automatically call SFC this way. I don't want to dirty my purrrty `JSX`

## The Differ
React's VDOM is all about combining your one, big render object with a diffing algorith, then finally the DOM updater.

In traversing your render object, the differ checks for `type` changes and `prop` changes.
- same type, same props => no update
- DOM element changes => update only the `attribute` that changed in the DOM (no React lifecycle hooks run === super performant)
- Custom element changes => call `componentWillReceiveProps()` and `componentWillUpdate()` on the underlying instance, then `render()` and recurse result with diffing algorithm.
- ... others... read [Reconciliation - React](https://facebook.github.io/react/docs/reconciliation.html)

So, the less custom elements in your tree, the better? When is a re-render called? When `setState()` or `forceUpdate()` is called in a custom component. TODO: rephrase above from "one, big render() object" to something more like "a component's render object -- at the start is is just the render of your Root."

## Resources
- [Introducing JSX - React](https://facebook.github.io/react/docs/introducing-jsx.html)
- [Rendering Elements - React](https://facebook.github.io/react/docs/rendering-elements.html)
- [Reconciliation - React](https://facebook.github.io/react/docs/reconciliation.html)