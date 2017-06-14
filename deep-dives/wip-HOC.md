# What is a HOC?
"A Higher-Order Component"

What does that mean?

"Well, if a _higher-order function_ is a **function** that **takes a function** and **returns a new function** as the result, then a _higher-order component_ must be a **component** that **takes a component** and **returns a new component** as the result, right?"

That is often the community's response, but isn't that just a component with children? How about the description that "A Higher Order Component is just a React Component that wraps another one"?

Again, isn't that just a component with children?

Fortunately, [React's Official Docs](https://facebook.github.io/react/docs/higher-order-components.html) have been updated with a description that makes more sense:

> A higher-order component is a **function** that **takes a component** and **returns a new component**

Ahh, good. A HOC is the `function` that returns the component, not the returned component itself.

```jsx
// This HOC takes a component...
function enhance(WrappedComponent, config) {

  // ...and returns another component
  return class extends React.Component {
    render() {
      const propsToPass = getPropsToPass(config)

      return (
        <WrappedComponent {...propsToPass} />
      )
    }
  }
}
```

# How is it used
Usualy, it is used to enhance a component as it is exported, like

```jsx
import enhanceWithTouch from 'enhance-with-touch'

class MyComponent extends React.Component {
  render() {
    return (
      <div />
    )
  }
}

// use HOC to enhance MyComponent with onPress functionality
export default enhanceWithTouch(MyComponent, {onPress: (e) => console.log(e)})
```

or it is used to enhance a component after it has been imported, like

```jsx
import SomeComponent from 'SomeComponent'

// use HOC to enhance SomeComponent before it is used in the render()
const EnhancedComponent = enhance(SomeComponent)

class MyComponent extends React.Component {
  // ...

  render() {
    return (
      <EnhancedComponent />
    )
  }
}
```

Note: Redux's `connect(mapStateToProps, mapDispatchToProps)(MyComponent)` is also a HOC, it just looks a bit more fancy because it is also using currying.

# Considerations when building your own
First of all, you may not need to! Our community may've already built it.

[Recompose](https://github.com/acdlite/recompose), for example, is a popular HOC utility belt that provides many useful HOCs, like `pure()` and `onlyUpdateForKeys(['prop1', 'prop2'])` to easily set up `shouldComponentUpdate`s. As its docs say, "think of it like lodash for React."

If you are building your own though, be sure to:
- pass `props` that aren't related to your HOC, so it will play well with other wrappers
- hoist `static`s so the original component's API is not compromised. You might like [hoist-non-react-statics](https://github.com/mridgway/hoist-non-react-statics) for this.
- provide a debug label using `displayName`
- `ref`s aren't passed through, so use `refNode` pattern. (TODO: link to refs article)

---

Props to [Official Docs](https://facebook.github.io/react/docs/higher-order-components.html#convention-pass-unrelated-props-through-to-the-wrapped-component) for greatly improving its HOC documentation!