# All about React's `shouldComponentUpdate()`
## tl;dr
- should be renamed to `shouldRerender()`
- keep stateful components shallow
- never `PureComponent` a component with children
- `cloneElement` of a `PureComponent` is tricky

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

Note: this hook is only available to class-based components, not stateless functional components.

### Why do we care about limiting re-renders?
For performance!

The less code we run, the more performant it'll be. In fact, the most performant thing we could do in our React apps today would be to write all of our components with a `shouldComponentUpdate() { return false }`. Of course, that is just about as helpful as saying "the most secure computer is the one that is turned off, and buried 100 feet under ground". Our app would be completely static and non-interactive if all of our components had a `sCU => false`.

There are some components that really should be static and non-interactive though. Once rendered the first time, they should live the rest of their days without updating and re-rendering. This is where we'll want to `sCU => false`.

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

Be careful with the equality checks of functions, objects, and arrays though. You'll want to use their references and consider using an immutable lib for deep equality checks.

## OK, intro over. Lets dig in!
### Should be renamed to `shouldComponentRerender`
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

and this component which just updates the prop it passes to the above component after a timeout,
```jsx
class UpdatesChildProp extends React.Component {
  state = {
    updates: 'hello',
  }

  componentDidMount() {
    setTimeout(() => this.setState({updates: 'bye'}), 400)
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
"render: hello"
"cWRP: bye"
```

What does this mean? `sCU => false` will not re-render, but its props will still update. So really, it is best to think of `shouldComponentUpdate` as more of a `shouldComponentRerender`.

Note: this is actually a pretty cool thing. One use case where we don't want to re-render a component, but still respond to an updated prop is for enabling a declarative prop for an imperative operation. An `<Animate>` component, for example, might accept a `triggerOnChangedValue` prop, which will call an `.animate()` method. Using this component will not compute re-renders, it'll just animate it with the prop change: `<Animate triggerOnChangedValue={this.state.animationTrigger} />`.

## Keep Stateful Components Shallow

[Jason Miller 🦊⚛ on Twitter: "found this lying around https://t.co/2FYSvqw6XX https://t.co/BZSNNjN8b6"](https://twitter.com/_developit/status/867120306539954176)

[Optimizing React Rendering (Part 1) – Flexport Engineering](https://flexport.engineering/optimizing-react-rendering-part-1-9634469dca02)


- [Dan Abramov on Twitter: "PSA: React.PureComponent can make your app slower if you use it everywhere."](https://twitter.com/dan_abramov/status/820668074223353858)
- React.PureComponent, shallowCompare
- stateless functional component can be pure, but it'll still rerender every time
- use primitives over objects/arrays, as http://ericlathrop.com/2017/02/why-did-this-react-component-rerender/ mentions
- scu => false will not prevent props from updating
  - actually a feature. Animate_'s `start*` to trigger animations without causing a rerender.
    - combines scu with cwrp. Advanced understanding to get here.

be sure to look through your [gdoc](https://docs.google.com/document/d/1KfC1aoQbd9bAVXQGNNGm5-1hCG-P2TeX-vabaVZG6I8/edit)

CDM: child, then parent

reference https://facebook.github.io/react/docs/optimizing-performance.html

cSU == false
- still calls `willReceiveProps`

PureComponent re-rendering in Animate_
- children problem?
- animate() doing something to force re-render?
- I think it is a child mismatch problem
  - because child in render does not match what it returns?
- [Ives van Hoorne on Twitter: "Really good point by @angelpiscola, creating objects in render will also force a rerender for pure components, even if props didn't change. https://t.co/shKMYVOg1c"](https://twitter.com/Ives13/status/863129346592178178)

# Reduce re-renders by keeping stateful components shallow
Spawned from "One of the best tactics that you can apply early on is to try to keep components shallow. It will help you see which props components actually use and cut an extra re-render computation." - comment of https://marmelab.com/blog/2017/02/06/react-is-slow-react-is-fast.html

Would it be best to keep components as small as possible. No letting the depth increase by more than, say 3 components and height be no more than 3 sibling components?

That way a when a single component in a render needs to be updated, only a few other things will re-render, not many unrelated components.

How would this apply to Page level components?

"By removing the draft Tweet state from updating the main Redux state on every keypress and keeping things local in the React component’s state, we were able to reduce the overhead by over 50% (above, right)." - https://medium.com/@paularmstrong/twitter-lite-and-high-performance-react-progressive-web-apps-at-scale-d28a00e780a3

- https://twitter.com/_developit/status/872068281628282880
- ![](https://pbs.twimg.com/media/DBo1eKqV0AEms32.jpg)
- "Ok, that make sense. The more depth your tree has the more time the diffing will take."

---
Connect components that are as deep in the tree as possible. Changes affect fewer components for updates. Vs changing something at the root.

Thought about while reading https://dev.bleacherreport.com/3-things-i-learned-about-working-with-data-in-redux-5fa0d5f89c8b#.ij978jnby
---

# links
[Tommy Leunen on Twitter: "React Q: Should we always use PureComponent and a pure "mixin" for SFC? Or bc most components are not costly to render it's better not?"](https://twitter.com/tommy/status/854366714812747776)
- [Daniel Steigerwald on Twitter: "ShouldUpdate check belongs to data (redux connect), not view. Learned that the hard way."](https://twitter.com/steida/status/854374773270380544)

`React.cloneElement()` breaks PureComponent children because the passedProps is a new object every time?
- [React is Slow, React is Fast: Optimizing React Apps in Practice – DailyJS – Medium](https://medium.com/dailyjs/react-is-slow-react-is-fast-optimizing-react-apps-in-practice-394176a11fba#.tkrfivb1w)

[Quick Redux tips for connecting your React components](https://medium.com/dailyjs/quick-redux-tips-for-connecting-your-react-components-e08da72f5b3)

## shouldComponentUpdate
Did you know that you should never pure a comp with children? I certainly didn't before I did some digging. Join my on that adventure, then sick around for done other goodies if you'd like.

Given this page, what do we expect to see in console?
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

  ...

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

Makes sense, makes sense. Let's explore PureComponent.
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

  ...

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

Uhhh, what? Why is a pureComponent re-rendering even when the child hasn't changed?

One more exploration:
```jsx
class PureComponentWithChildren extends React.PureComponent {

  render() {
    console.log('PureComponentWithChildren Render');

    return (
      <PureComponentWrapper>
        <Div />
      </PureComponentWrapper>
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
      <PureComponentWithChildren />
    )
  }
}
```
```bash
PureComponentWithChildren Render # initial mount
PureComponentWrapper Render # initial mount
Div Render # initial mount

# nothing after setState
```

Bummer, this means we can't benefit from `PureComponent` with children defined in a parent component.
[Children prop gets recreated killing PureComponent optimizations · Issue #8669 · facebook/react](https://github.com/facebook/react/issues/8669)


**New rule: only allow `PureComponent` in components that do not have a `children` prop**


Other reads
- [React.PureComponent Considered Harmful – Hacker Noon](https://hackernoon.com/react-purecomponent-considered-harmful-8155b5c1d4bc)
- [React PureComponent Pitfalls – ShakaCode](https://blog.shakacode.com/react-purecomponent-pitfalls-d057882f4b6e)
- [Take `children` off `props` · Issue #4694 · facebook/react](https://github.com/facebook/react/issues/4694)


# `cloneElement()` of a PureComponent child
Beware: when cloning a PureComponent, the same don't-pass-objects-or-arrays rule applies. The 2nd argument of the `cloneElement()` will always be an object, but no value of that object should be an object or array.
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

Above is the desired effect. Below is what'll happen if `Cloning_` returns an object in an object: `React.cloneElement(child, {propsObject: propsToPass})`
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

[[Perf] shouldComponentUpdate/pure components do not work with react element/node type props · Issue #7412 · facebook/react](https://github.com/facebook/react/issues/7412)
