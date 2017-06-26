# All About Refs
`ref` is one of two special props in React--the other being `key`--because it isn't really a prop. Instead of being passed in to the associated component as a `this.props.ref`, React snatches it up, and uses it to associate a reference to the component's instance or DOM node.

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

Above example shows setting `this.node` to the `div` DOM node via a `ref callback`. Note: the old way, `<div ref='node' />` is deprecated; don't do it!

## When is it a DOM node vs. a component's instance?
If it is placed on a primitive like `<div ref={...} />`, it will give you the DOM node instance. If it is placed on a composite component like `<MyComponent ref={...} />`, it will give you the component instance.

> `ref` on a *lowercase* `<tag />` -> DOM node

> `ref` on a *PascalCase* `<Tag />` -> component's instance

Why? Lowercase tags in React are your lowest level primitives. React treats them different from your custom composite components, which can **not** be lowercase.

## When is the `ref` first set?
After the first `render()`, but before `componentDidMount()`, your ref will be accessible.

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

// 'render'
// 'ref'
// 'cdm'
```

## When would you want a `ref`?
OK, you know how to get a `ref`, what it points to, and when it is set, but **when** should you actually reach for this power?

Ideally, never! `ref`s impact performance be disabling some production babel transforms ([inline-elements](https://babeljs.io/docs/plugins/transform-react-inline-elements/) and [constant-elements](https://babeljs.io/docs/plugins/transform-react-constant-elements/). Netflix even goes as far as entirely preventing their custom renderer from using `ref`s ([source](https://medium.com/netflix-techblog/crafting-a-high-performance-tv-user-interface-using-react-3350e5a6ad3b)) for component inlining.

You may be able to avoid needing a `ref` with declarative props, as the official docs say:

> For example, instead of exposing `open()` and `close()` methods on a `Dialog` component, pass an `isOpen` prop to it.

But reallistically, you're likely to need some imperative functionality: animating, media playback, DOM measurements, etc. For example, an `Animate` component may be more unwieldy managing an `isAnimating` prop than just calling a `this.animate.trigger()`.









 the that is not actually passed down as a prop to the component. Instead, it is automatically called of React's magical props, in that it is used by React itself, not by the component that attach the `ref` to. Cool, you likely already knew that, but did you know:

If you haven't read the official docs or haven't read it in a year or so, I recommend you read it. It even covers functional components not being `ref`-able.

I do have a few things to add to the topic that justify this post though.

https://github.com/kylpo/react-playbook/blob/master/best-practices/react.md#refs

https://facebook.github.io/react/docs/refs-and-the-dom.html#caveats

[Refs and the DOM - React Express](http://www.react.express/refs_and_the_dom)

## RefNode
```js
refNode={node => this.node = node}
ref={this.props.refNode}
```


## Gotchas
#### `ref` can become `null` after it is set
I'll defer to Dan Abramov for this one:

> Ever wondered why callback refs get called with null during the updates? I wrote a bit about this: https://t.co/42kdCy0eKF - [Dan Abramov](https://twitter.com/dan_abramov/status/859159065498406913)

> That's a very long way to say "null means unmount, use it as a signal to perform cleanup" 😉😊 - [Glen Mailer](https://twitter.com/glenathan/status/859161300668166146)

#### No `ref` for functional components
- You may not use the ref attribute on functional components because they don't have instances. https://facebook.github.io/react/docs/refs-and-the-dom.html#refs-and-functional-components

## ref callbacks
- callbacks, how to avoid boilerplate? called on when unmounted (passes null)?
- how to reduce boilerplate with something like https://github.com/fresk-nc/babel-plugin-transform-jsx-ref-to-function?
- wish I could have a `setRef` prop that just accepts a pointer to my instance field.
OK, fine, callback refs replaced string refs. This gives us more power! But, dang, this does add more boilerplate.
linkRef

[developit/linkref: Like Linked State, but for Refs. Works with Preact and React.](https://github.com/developit/linkref)


- [Jason Miller 🦊⚛ on Twitter: "@aweary This is one reason I recommend linkRef: https://t.co/dEpsDrOrIT https://t.co/euiZDxFF8W"](https://twitter.com/_developit/status/859384258498101250)
- [James Kyle on Twitter: "@_developit @aweary @preactjs Here you go https://t.co/QT6YrAaNBE"](https://twitter.com/thejameskyle/status/859420749844680708)

## refnode

## when is it set?

## called with null

## onLayout
props to Mike for the idea

- https://facebook.github.io/react/docs/refs-and-the-dom.html#caveats

https://github.com/necolas/react-native-web/issues/60

https://github.com/constelation/monorepo/issues/79
