# Enhancing Components
Lets say you'd like to extract out some utility code out of a component for re-use with other components. It could be something that connects to stores, something that calculates values and passes them along, something that adds a loading spinner while retrieving data, etc. We'll broadly call that something an **enhancement** going forward, but you could also think of it as a configuration or manipulation.

So, how do you go about extracting and reusing this enhancement? Well, you have a some options...

*TL;DR*
- *Static enhancement? Use `HOC` or `@decorator`.*
- *Dynamic enhancement? Use `cloneElement` or `child function`.*

## HOC

## @decorator

## cloneElement

## child function

## Summary

## Other resources
- [React Higher Order Components in depth – franleplant – Medium](https://medium.com/@franleplant/react-higher-order-components-in-depth-cf9032ee6c3e)
- [Write a Higher Order Component from Scratch - react Video Tutorial #free @eggheadio](https://egghead.io/lessons/react-write-a-higher-order-component-from-scratch) for a good intro to HOC (other than the terminology)

Really, it comes down to whether you'd like this enhancement done inside a component's `render()` (i.e. runtime/dynamic) or not.

---

HOC, class decorators, cloneElement, child function


Pros and Cons of each of the 4 options

class decorators
- [Exploring EcmaScript Decorators – Google Developers – Medium](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841)
  - scroll to "Decorating a class"
- [wycats/javascript-decorators](https://github.com/wycats/javascript-decorators#class-declaration)
- use for similar functionality as `react.createClass()`'s `mixin` to modify the class.


[Using Functions as Children and Render Props in React Components](https://medium.com/codedaily/using-functions-as-children-and-render-props-in-react-components-113315b044ea)


[Techniques for decomposing React components – DailyJS – Medium](https://medium.com/dailyjs/techniques-for-decomposing-react-components-e8a1081ef5da)

## HOCs
Basically a HOC enhances a component with new capabilities. This way you can abstract common behaviors into reusable pieces.

## Types of HOCs
Wrappers -> wrap children and introduce something to dom

Manipulators (or Injectors) -> pass props (injects props into child(ren))

## Function as Children
[Ryan Florence on Twitter: "@wincent @timbucki @sebmck I think we're talking about <Thing>{() => ()}</Thing> v. <Thing render={() => ()}/> In which case, I, too, prefer the "attribute" form."](https://twitter.com/ryanflorence/status/865302840696225792)

## HOC
### But first, a detour
What is a HOC?

"A Higher-Order Component"

What does that mean?

"Well, if a _higher-order function_ is a **function** that **takes a function** and **returns a new function** as the result, then a _higher-order component_ must be a **component** that **takes a component** and **returns a new component** as the result, right?"

That is often the community's response, but isn't that just a component with children?

Or how about the description that "A Higher Order Component is just a React Component that wraps another one"?

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

### Back on topic
OK, we know what a HOC is, but when should we use it?

**Consider using a HOC when it is an enhancement to a component definition or declaration. I.e. it is used outside of a `render()`**

```jsx
class MyComponent extends React.Component {
  render() {
    return (
      <div />
    )
  }
}

// use HOC to enhance MyComponent with onPress functionality
const EnhancedMyComponent = enhanceWithTouch(MyComponent, {onPress: (e) => console.log(e)})

// render an instance of the enhanced component
ReactDom.Render(<EnhancedMyComponent />, /* ... */)
```

Above, we are using a HOC to essentially _statically_ configure a new `MyComponent` definition (without mutating the original), then rendering an instance of it. It is used like a `mixin`, with the added benefit that it doesn't affect the original class.

Also, consider using [recompose](https://github.com/acdlite/recompose) for a lodash-like helper to HOCs.

### Dynamic Enhancement
What happens if I want to change the enhancement of my component at runtime? When local state changes, for example.

You *could* technically use a HOC in your render, and update its arguments depending on state:

```jsx
class MyComponent extends React.Component {
  // ...

  render() {
    const Enhanced = enhanced(
      MyOtherComponent,
      {enhance: this.state.shouldEnhance),
    )

    return (
      <Enhanced />
    )
  }
}
```

But then you'll be recreating the `Enhanced` component on **every** re-render and trigger unmounts and remounts. Read more from [the official docs](https://facebook.github.io/react/docs/higher-order-components.html#dont-use-hocs-inside-the-render-method).

You *could* technically expose all of your configuration options in the resulting enhanced component for updates, but then you lose out on a clear seperation of concerns:

```jsx
const AnimationComponent = enhanceWithAnimation(SomeComponent, animationConfig)
const AnimationTouchableComponent = enhanceWithTouch(AnimationComponent, touchableConfig)
const AnimationTouchableStyledComponent = enhanceWithStyle(AnimationTouchableComponent, styledConfig)

class MyComponent extends React.Component {
  // ...

  render() {
    return (
      <AnimationTouchableStyledComponent
        animationConfig={this.state.animationConfig}
        touchableConfig={this.state.touchableConfig}
        styledConfig={this.state.styledConfig}
      />
    )
  }
}
```

## Enter `cloneElement`
As mentioned in __TODO__, `React.cloneElement()` is used for **dynamic**, **runtime** component enhancement with a clear **separation of concerns**.

But using it isn't exactly easy or intuitive. Let's learn more about it.

`React.cloneElement()` allows us to clone a runtime _element_, and apply an enhancement. With it, we can update our enhancement in our render and not worry about triggering unmounts and remounts!

```jsx
class EnhancingCloneElement extends React.Component {
  render() {
    // React.cloneElement() requires a single child
    const WrappedElement = React.Children.only(this.props.children)

    // return new element with an `enhance` prop
    return React.cloneElement(WrappedElement, {enhance: this.props.enhance})
  }
}

class MyComponent extends React.Component {
  // ...

  render() {
    return (
      <EnhancingCloneElement
        enhance={this.state.shouldEnhance}
      >
        <div />
      </EnhancingCloneElement>
    )
  }
}

ReactDom.Render(<MyComponent />, /* ... */)
```

Contrived example, as a `div` won't care about an `enhance` prop, and we could've just passed it in directly to the `div` if we wanted, but what about if `<EnhancingCloneElement />` computed something complicated, then translated the result to something `div` could use? That is quite a powerful abstraction!

More on `cloneElement()` in a future post.

## Wrapping it up
Use `HOC` for static enhancement and `cloneElement` for dynamic enhancement. If you know upfront how to enhance a component without the enhancement changing, use a HOC. Else use `cloneElement` to enhance a runtime element.


---

Note: I publish these to learn from your responses! Please let me know if you have any thoughts on the subject.
