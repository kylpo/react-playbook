# All about React's `shouldComponentUpdate()`
## TL;DR
- more like `shouldComponentRerender()`
- never `PureComponent` a component with children
- `cloneElement()` of a `PureComponent` is tricky

## A brief introduction
By default, when a component changes and re-renders, all components in its `render()` are also re-rendered, and their subcomponents are re-rendered, and so on, all the way down. `shouldComponentUpdate()` is a class-based lifecycle method that empowers developers to change the default of a component, and prevent re-rendering completely, or only enable it in certain cases.

```jsx
class MyComponent extends React.Component {
  /**
  * Default shouldComponentUpdate when not defined
  *
  * shouldComponentUpdate() {
  *   return true
  * }
  */

  render() {
    //...
  }
}
```

Note: this hook is only available to class-based components, not Stateless Functional Components.

### Why do we care about limiting re-renders?
Performance!

The less code we run, the more performant it'll be. In fact, the most performant thing we could do in our React apps today would be to write all of our components with a `shouldComponentUpdate() { return false }`. Of course, that is just about as helpful as saying "the most secure computer is the one that is turned off, and buried 100 feet under ground". Our app would be completely static and non-interactive if all of our components had a `sCU => false`.

There are some components that really should be static and non-interactive though. Once rendered the first time, they should live the rest of their time without updating and re-rendering. This is where we'll want to `sCU => false`.

There are also components that should re-render if and only if certain (or any) props have actually changed. This is where custom `shouldComponentUpdate()` and `React.PureComponent` come in.

```jsx
class CustomShouldComponentUpdate extends React.Component {
   shouldComponentUpdate(nextProps, nextState) {
     return nextProps.trigger !== this.props.trigger
   }

  render() {
    //...
  }
}
```

### PureComponent
In our class-based components, we can extend `React.PureComponent` instead of `React.Component` to only re-render if a prop or state has changed. It essentially inherits a `shouldComponentUpdate` that checks the current props and state with the new ones.

```jsx
class MyPureComponent extends React.PureComponent {
  /**
  * Default PureComponent shouldComponentUpdate
  *
  * shouldComponentUpdate(nextProps, nextState) {
  *   // essentially (this.props !== nextProps || this.state !== nextState)
  *   return shallowCompare(this, nextProps, nextState)
  * }
  */

  render() {
    //...
  }
}
```

Be careful with the equality checks of functions, objects, and arrays though. You'll want to use their **references** (not creating new ones in `render()`s) and consider using an immutable lib for deep equality checks.

Note: Some Stateless Functional Components (not class-based) may be "pure" in the sense that they do not have side effects, but they will always re-render, so don't confuse them as `PureComponent`s.

## OK, intro over. Lets dig in!
### Should be renamed to `shouldComponentRerender`?
Given this `sCU => false` component,
```jsx
class NoUpdate extends React.Component {
  shouldComponentUpdate() {
    return false
  }

  componentWillReceiveProps(nextProps) {
    console.log('cWRP: ', nextProps)
  }

  render() {
    console.log('render: ', this.props)
  }
}
```

and this component that just updates the prop it passes to the above component after a timeout,
```jsx
class UpdatesChildProp extends React.Component {
  state = {
    updates: 'initial',
  }

  componentDidMount() {
    setTimeout(() => this.setState({updates: 'updated'}), 400)
  }

  render() {
    return (
      <NoUpdate updatingProp={this.state.updates} />
    )
  }
}

ReactDom.render(<UpdatesChildProp />)
```

what do you expect to see in `console`?

```
render: {updatingProp: "initial"}
cWRP: {updatingProp: "updated"}
```

What does this mean? `sCU => false` will not re-render, but its props will still update.

> So really, it is best to think of `shouldComponentUpdate` as more of a `shouldComponentRerender`.

Note: this is actually a pretty cool thing! One use case where we don't want to re-render a component, but still respond to an updated prop is for enabling a declarative prop for an imperative operation. An `<Animate>` component, for example, might accept a `triggerOnChangedValue` prop, which will call an `.animate()` method. Using this component will not re-render, it'll just `animate()` when the prop changes: `<Animate triggerOnChangedValue={this.state.animationTrigger} />`.

## When to avoid `PureComponent`
When Dan Abramov tweeted ["PSA: React.PureComponent can make your app slower if you use it everywhere."](https://twitter.com/dan_abramov/status/820668074223353858), he was referring to `PureComponent`'s `shouldComponentUpdate()` executing code for each update. If a component is re-rendered more often that it is prevented, then it is wastefully executing that code. As mentioned in the intro, this'll happen when passing in new objects, arrays, and functions, but there is another case...

### Exploring `shouldComponentUpdate` and `children`
Let's start with some controls. Given this page, what do we expect to see in console?
```jsx
const Div = (props) => {
  console.log('Div Render');

  return (
    <div>{props.children}</div>
  )
}

export default class Home extends React.Component {
  state = {}

  componentDidMount() {
    setTimeout(() => this.setState({hi: 'hi'}), 2000)
  }

  render() {
    return (
      <Div>{this.state.hi}</Div>
    )
  }
}
```
```bash
Div Render # initial mount

Div Render # after setState
```

Given this added code, what do we expect to see in console?
```jsx
class WrappingWontUpdate extends React.Component {
  shouldComponentUpdate() {
    return false
  }

  render() {
    console.log('WrappingWontUpdate Render');
    return this.props.children
  }
}

export default class Home extends React.Component {
  //...

  render() {
    return (
      <WrappingWontUpdate>
        <Div>{this.state.hi}</Div>
      </WrappingWontUpdate>
    )
  }
}
```
```bash
WrappingWontUpdate Render # initial mount
Div Render # initial mount

# nothing after setState
```

Makes sense, makes sense. Let's explore `PureComponent`.
```jsx
class PureComponentWrapper extends React.PureComponent {
  render() {
    console.log('PureComponentWrapper Render');
    return (
      <div>{this.props.children}</div>
    )
  }
}

export default class Home extends React.Component {
  //...

  render() {
    return (
      <PureComponentWrapper>
        <Div>{this.state.hi}</Div>
      </PureComponentWrapper>
    )
  }
}
```
```bash
PureComponentWrapper Render # initial mount
Div Render # initial mount

PureComponentWrapper Render # after setState
Div Render # after setState
```

Hmmm, we have a wrapping PureComponent, but still its update is run. Ah, this must be because the child is updated. So what happens when we try this render?
```jsx
<PureComponentWrapper>
  <WontUpdate />
</PureComponentWrapper>
```
```bash
PureComponentWrapper Render # initial mount
WontUpdate Render # initial mount

PureComponentWrapper Render # after setState
```

Uhhh, what? Why is `PureComponentWrapper` re-rendering even when the child hasn't changed? Does `PureComponent` not actually do anything?

One more exploration:
```jsx
class PureComponentWithSubcomponents extends React.PureComponent {
  render() {
    console.log('PureComponentWithSubcomponents Render');

    return (
      <span>
        <Div />
      </span>
    )
  }
}

export default class Home extends React.Component {
  state = {}

  componentDidMount() {
    setTimeout(() => this.setState({hi: 'hi'}), 2000)
  }

  render() {
    return (
      <PureComponentWithSubcomponents />
    )
  }
}
```
```bash
PureComponentWithSubcomponents Render # initial mount
Div Render # initial mount

# nothing after setState
```

Yes, the control above shows that `PureComponent` does indeed work when children are not passed to it. The difference is that this `PureComponent` only has subcomponents in its render, not children passed in via the parent. Bummer, this means we can't benefit from `PureComponent`s with children.

> New rule: Never `PureComponent` a component with a `children` prop. `PureComponent`s with subcomponents in their render are still OK though.

See [this](https://github.com/facebook/react/issues/8669) github issue for more.

## `cloneElement()` of a `PureComponent` child
This has been a long post, so I'll get right to the point:

> When cloning a `PureComponent`, the 2nd argument of the `cloneElement()` can be a new object, but no value of that object should be a new object, array, or function.

Below is an example of safe usage:
```jsx
class Cloning_ extends React.Component {
  render() {
    console.log('Cloning Render')

    const { children, ...propsToPass } = this.props
    const child = React.Children.only(children)

    console.log('passProps', propsToPass)

    return React.cloneElement(child, propsToPass)
  }
}

class PureComponent extends React.PureComponent {
  render() {
    console.log('PureComponent Render')
    return (
      <div />
    )
  }
}

export default class App extends React.Component {
  state = {}

  componentDidMount() {
    setTimeout(() => this.setState({hi: 'hi'}), 2000)
  }

  render() {
    return (
      <Cloning_ hi='hi'>
        <PureComponent />
      </Cloning_>
    )
  }
}

```
```bash
# first render
Cloning Render
passProps Object {hi: "hi"}
PureComponent Render

# after setState
Cloning Render
passProps Object {hi: "hi"}
# GOOD! missing PureComponent Render
```

And now the result of the bad case, where `Cloning_` returns a new object with a new object: `React.cloneElement(child, {propsObject: propsToPass})`
```bash
# first render
Cloning Render
passProps Object {hi: "hi"}
PureComponent Render

# after setState
Cloning Render
passProps Object {hi: "hi"}
PureComponent Render # BAD!
```

See [this](https://github.com/facebook/react/issues/7412) github issue for more.

# Other reads
- [Optimizing Performance - React](https://facebook.github.io/react/docs/optimizing-performance.html#shouldcomponentupdate-in-action)
- [React PureComponent Pitfalls – ShakaCode](https://blog.shakacode.com/react-purecomponent-pitfalls-d057882f4b6e)
- [Take `children` off `props` · Issue #4694 · facebook/react](https://github.com/facebook/react/issues/4694)
- [Optimizing React Rendering (Part 1) – Flexport Engineering](https://flexport.engineering/optimizing-react-rendering-part-1-9634469dca02)
- ["React Q: Should we always use PureComponent and a pure "mixin" for SFC? Or bc most components are not costly to render it's better not?"](https://twitter.com/tommy/status/854366714812747776)
- [Why Did This React Component Re-render? by Eric Lathrop](http://ericlathrop.com/2017/02/why-did-this-react-component-rerender/)
- [React is Slow, React is Fast: Optimizing React Apps in Practice – DailyJS – Medium](https://medium.com/dailyjs/react-is-slow-react-is-fast-optimizing-react-apps-in-practice-394176a11fba#.tkrfivb1w)
