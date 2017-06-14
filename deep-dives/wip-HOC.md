# What is a HOC?
"A Higher-Order Component"

What does that mean?

"Well, if a _higher-order function_ is a **function** that **takes a function** and **returns a new function** as the result, then a _higher-order component_ must be a **component** that **takes a component** and **returns a new component** as the result, right?"

That is often the community's response, but isn't that just a component with children?

How about the description that "A Higher Order Component is just a React Component that wraps another one"?

Again, isn't that just a component with children?

Fortunately, [React's Official Docs](https://facebook.github.io/react/docs/higher-order-components.html) have been updated with a description that makes more sense:

> a higher-order component is a **function** that **takes a component** and **returns a new component**

Ahh, good. A HOC is the `function` that returns the component, not the component itself.

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

Note: Redux's `connect(mapStateToProps, mapDispatchToProps)(MyComponent)` is also a HOC, it just looks a bit more fancy because it is also using currying.

# Considerations when building your own
First of all, you may not need to! The community may've already built it. [recompose](https://github.com/acdlite/recompose), for example, is a popular HOC utility belt that provides many useful HOCs, like `pure()` and `onlyUpdateForKeys(['prop1', 'prop2'])` to easily set up `shouldComponentUpdate`s. As its docs say, "think of it like lodash for React."

If you are building your own, be sure to:
- `passProps` that aren't related to HOC
- hoist statics
- debug label w/ `displayName`
- `ref`s aren't passed through, so use `refNode` pattern
  - TODO: link to my article

See [Official Docs](https://facebook.github.io/react/docs/higher-order-components.html#convention-pass-unrelated-props-through-to-the-wrapped-component)