# All About `ref`s
`ref` is one of two special props in React--the other being `key`--because it isn't really a prop. Instead of being passed in to the associated component as a `this.props.ref`, React snatches it up and uses it to associate a reference to the component's instance or DOM node.

```jsx
class MyComponent extends React.Component {
  setRef = (node) => {
    this.node = node
  }

  render() {
    return (
      <div ref={this.setRef} />
    )
  }
}
```

Above example shows setting `this.node` to the `div` DOM node via a `ref` callback. Note: the old `ref` string way, `<div ref='node' />` is deprecated; don't do it!

## When is it a DOM node vs. a component's instance?
If it is placed on a primitive like `<div ref={...} />`, it will give you the DOM node instance. If it is placed on a composite component like `<MyComponent ref={...} />`, it will give you the component instance.

> `ref` on a *lowercase* `<tag />` -> DOM node

> `ref` on a *PascalCase* `<Tag />` -> component's instance

Why? Lowercase tags in React are your lowest level primitives. React treats them different from your custom composite components, which can **not** be lowercase.

## When is the `ref` first set?
After the first `render()`, but before `componentDidMount()`, your ref will be set.

Feel free to see for yourself:
```jsx
class Logs extends React.Component {
  componentDidMount() {
    console.log('cdm');
  }

  render() {
    console.log('render');
    return (
      <div ref={() => console.log('ref')} />
    )
  }
}

// Results:
// 'render'
// 'ref'
// 'cdm'
```

## When would you want a `ref`?
OK, you know how to get a `ref`, what it points to, and when it is set, but **when** should you actually reach for this power?

Ideally, never! `ref`s impact performance by disabling some production babel transforms ([inline-elements](https://babeljs.io/docs/plugins/transform-react-inline-elements/) and [constant-elements](https://babeljs.io/docs/plugins/transform-react-constant-elements/)). Netflix even goes as far as entirely preventing their custom renderer from using `ref`s ([source](https://medium.com/netflix-techblog/crafting-a-high-performance-tv-user-interface-using-react-3350e5a6ad3b)) for component inlining.

And you may be able to forgo a `ref` with declarative props, as the official docs say:

> For example, instead of exposing `open()` and `close()` methods on a `Dialog` component, pass an `isOpen` prop to it.

But realistically, you're likely to need some imperative functionality: animating, media playback, DOM measurements, etc. For example, an `Animate` component may be more unwieldy managing an `isAnimating` prop than just calling `this.animate.trigger()`.

## The cool stuff
With `ref` callbacks, we can do some pretty cool stuff!

### `refNode` pattern
Back when `ref` strings were the norm, it was difficult to extract out subcomponents:

```jsx
class MyTooBigComponent extends React.Component {
  componentDidMount() {
    // this.refs.componentToExtract is a DOM node
  }

  render() {
    return (
      <div>
        {/* ... other stuff */}

        <div ref='componentToExtract'>
          <span>...</span>
        </div>

      </div>
    )
  }
}
```

If you refactored it to

```jsx
class MySubComponent extends React.Component {
  render() {
    return (
      <div>
        <span>...</span>
      </div>
    )
  }
}

class MyTooBigComponent extends React.Component {
  componentDidMount() {
    // this.refs.componentToExtract is a component instance!
  }

  render() {
    return (
      <div>
        {/* ... other stuff */}

        <MySubComponent ref='componentToExtract' />

      </div>
    )
  }
}
```

you lose access to the DOM node. But with `ref` callbacks, you can pass the callback as a non-`ref` prop, and the subcomponent can set the `ref` correctly.

```jsx
class MySubComponent extends React.Component {
  render() {
    return (
      <div ref={this.props.refNode}>
        <span>...</span>
      </div>
    )
  }
}

class MyTooBigComponent extends React.Component {
  componentDidMount() {
    // this.componentToExtract is again the DOM node!
  }

  setRef = (node) => {
    this.componentToExtract = node
  }

  render() {
    return (
      <div>
        {/* ... other stuff */}

        <MySubComponent refNode={this.setRef} />

      </div>
    )
  }
}
```

This is the `refNode` pattern, and is standardized across all primitives in [constelation](https://github.com/constelation/monorepo), a react-native and web prototyping framework my team uses everyday.

> `refNode` pattern: composite components accept a `refNode` prop to set outermost DOM element's `ref`

### onLayout
Measuring dimensions of a DOM node is often the reason to set a `ref` in the first place.

```jsx
class MyComponent extends React.Component {
  componentDidMount() {
    if (this.node) {
      this.measure()
    }
  }

  measure = () => {
    const { height, width } = this.node.getBoundingClientRect()
    // ...
  }

  setRef = (node) => {
    this.node = node
  }

  render() {
    return (
      <Image refNode={this.setRef} />
    )
  }
}
```

This adds quite a bit of boilerplate to accomplish a simple task. Let's do better by combining our learnings of **when** a `ref` is set with the power of `ref` callbacks.

```jsx
class MyComponent extends React.Component {
  setRef = (node) => {
    // null-check for reason mentioned later
    if (node) {
      const { height, width } = node.getBoundingClientRect()
      // ...
    }
  }

  render() {
    return (
      <Image refNode={this.setRef} />
    )
  }
}
```

Cool! That radically reduces our boilerplate. We don't even need to store the `ref` for later use. In [constelation](https://github.com/constelation/monorepo), we've gone one step further in reducing boilerplate for our primitives. We abstracted the above code into a simple `onLayout` prop (similar to [react-native](https://facebook.github.io/react-native/docs/view.html#onlayout), but the measure only happens once).

```jsx
class MyComponent extends React.Component {
  handleLayout = ({ height, width }) => {
    // ...
  }

  render() {
    return (
      <Image onLayout={this.handleLayout} />
    )
  }
}
```

*Shoutout to [Mike Hobizal - @openmike503](https://twitter.com/openmike503) for the idea!*

### `linkRef`
Alright, fine, `ref` callbacks give us more power than `ref` strings, but using them the correct way requires much more boilerplate.

```jsx
  setRef = (node) => {
    this.node = node
  }

  // ... later
  <div ref={this.setRef} />
```

vs. `<div ref='myRef' />`

Wouldn't it be nice to use `ref` callbacks with the same DX (developer experience) of `ref` strings?

Well, if you're using [Preact](https://github.com/developit/preact), you're in luck. [linkref](https://github.com/developit/linkref), from Preact's creator, provides an abstraction for creating and setting function `ref`s.

![](https://camo.githubusercontent.com/a2afe02731bdae6d2eacd5d6c975d754423db525/687474703a2f2f692e696d6775722e636f6d2f56346b5467626e2e706e67)

Unfortunatelu, `linkref` does **not** [currently](https://github.com/developit/linkref/issues/2) work in React. I'm hoping it will someday, or a future babel plugin will accomplish this. [babel-plugin-transform-jsx-ref-to-function](https://github.com/fresk-nc/babel-plugin-transform-jsx-ref-to-function) comes close, but it uses inline `ref` callbacks, which hurts performance (as mentioned below). [This](https://twitter.com/thejameskyle/status/859420749844680708) tweet thread with @thejameskyle is also relevant.

## The gotchas
### Don't inline `ref` callbacks
Code examples look so much simpler and easier to follow when `ref` callbacks are inlined:

```jsx
<div ref={node => this.node = node} />
```

Which is why so many tutorials show them inlined, but **don't** bring this bad practice into your code! Arrow and `bind` functions in a `render()` produce a performance hit by creating a new function on **EVERY** re-render.

Do the right thing:

```jsx
<div ref={this.setRef} />
```

### `ref` can become `null` after it is set
I'll defer to Dan Abramov for this one:

> Ever wondered why callback refs get called with null during the updates? I wrote a bit about this: https://t.co/42kdCy0eKF - [Dan Abramov](https://twitter.com/dan_abramov/status/859159065498406913)

> That's a very long way to say "null means unmount, use it as a signal to perform cleanup" 😉😊 - [Glen Mailer](https://twitter.com/glenathan/status/859161300668166146)

### No `ref` for functional components
From the [official docs](https://facebook.github.io/react/docs/refs-and-the-dom.html#refs-and-functional-components):

> You may not use the `ref` attribute on functional components because they don't have instances.

Speaking of the official docs, if it has been a while, I recommend you re-read them! They have really improved recently.