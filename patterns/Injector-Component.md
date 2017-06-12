# A Naming Convention for Injector Components
## What is an "injector component"?

An injector component takes props, optionally computes new ones, then injects them into its child via `React.cloneElement()`. Crucially, it also does not add any new components to the DOM. It only exists in React's virtual DOM.

```jsx
export class InjectorComponent extends React.Component {
  render() {
    const propsToPass = computePropsToPass(this.props)

    // cloneElement requires a single child, no arrays
    const Child = React.Children.only(this.props.children)

    // does not render anything additional to Child
    return React.cloneElement(Child, propsToPass)
  }
}

// Used later like...

<InjectorComponent
  something={something}
  another='another'
>
  <ChildToInjectTo />
</InjectorComponent>
```

## When Are Injector Components Useful?
In the prototyping framework, [constelation](https://github.com/constelation/monorepo), we use a LOT of injector components to abstract out logic and nicely separate concerns like style, animation, and interactions from our layout components. React Native has also used this pattern in [TouchableWithoutFeedback](https://github.com/facebook/react-native/blob/master/Libraries/Components/Touchable/TouchableWithoutFeedback.js#L173).

Here is an example render using some injector components:

```jsx
<Event
  onClick={this.handleClick}
>
  <Animate
    type='fadeIn'
    duration='3000ms'
    ref={node => this.animated = node}
  >
    <Style
      backgroundColor='red'
      border='1px solid black'
      transition='opacity 10000ms ease'
    >
      <View
        height='500px'
        width='200px'
        alignHorizontal='center'
        alignVertical='center'
      >
        <Text>Click me</Text>
      </View>
    </Style>
  </Animate>
</Event>
```

But here’s the thing, do you know which of these components are injectors? How many elements are actually added to the DOM?

## The `<Injector_ >` Naming Convention
With `constelation`, we've chosen to denote injector components with a postfix `_`. Think of the postfix `_` as being a blank space that needs to be filled by a child. Also, the name chains well. Below could read like: "an Evented, Animated, Styled View with a Text child"

```jsx
<Event_
  onClick={this.handleClick}
>
  <Animate_
    type='fadeIn'
    duration='3000ms'
    ref={node => this.animated = node}
  >
    <Style_
      backgroundColor='red'
      border='1px solid black'
      transition='opacity 10000ms ease'
    >
      <View
        height='500px'
        width='200px'
        alignHorizontal='center'
        alignVertical='center'
      >
        <Text>Click me</Text>
      </View>
    </Style_>
  </Animate_>
</Event_>
```

Notice how much easier it is to identify the injectors? Also, we can now easily see that there are two elements added to the DOM (two do not have a postfix `_`).

## It Gets Better
Can you spot the errors in this render?

```jsx
<Style_ />

<TouchableWithoutFeedback_>
  <Child1 />
  <Child2 />
</TouchableWithoutFeedback_>
```

Yes! Injector components should never be self-closing, and they should never wrap multiple children. We can fix this at code-time and not wait for the errors at runtime.

This naming convention helps developers understand the component's contract, and certainly would've helped me better understand how `TouchableWithoutFeedback` and `TouchableOpacity` differ.

## Even Better with Tooling
Naming conventions enable tooling. I've edited my vim color scheme to color injector components as props are colored, which further reinforces the concept and allows me to easily skim renders and identify them.

![](https://github.com/kylpo/notes/blob/master/assets/InjectorComponents.png?raw=true)

In the future, we could also create lint rules to identify errors at code-time using this convention.

**more naming conventions => more helpful tooling**

---

Note: I publish these to learn from your responses! Please let me know if you have any thoughts on the subject.
