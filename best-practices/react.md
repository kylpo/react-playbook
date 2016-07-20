```
.____            _     ____                 _   _               
| __ )  ___  ___| |_  |  _ \ _ __ __ _  ___| |_(_) ___ ___  ___
|  _ \ / _ \/ __| __| | |_) | '__/ _` |/ __| __| |/ __/ _ \/ __|
| |_) |  __/\__ \ |_  |  __/| | | (_| | (__| |_| | (_|  __/\__ \
|____/ \___||___/\__| |_|   |_|  \__,_|\___|\__|_|\___\___||___/
```

# React
## PropTypes
- Any props that are not required should have a defaultProp
  - [tweet](https://twitter.com/sebmarkbage/status/722128823706193920) from @sebmarkbage

## Refs
- Avoid using `refs` as much as possible
  - Why? Using refs makes the implementation more brittle. The `ref`d component can not be wrapped without custom logic.
  - Having a nested ref means that section of your render can not easily be refactored out into its own component.
- If you must use one (to get the dom node, for example), it must be to the top component in your render. __Don't ever use refs of a nested component.__ If you need the ref of a child, extract it out to its own component, then pass a prop/callback to it.
- DO NOT use string refs. All refs should use the callback style.
- DO NOT use `findDOMNode()`
  - use callback refs instead [abramov tweet](https://twitter.com/dan_abramov/status/752936646602031104)

## APIs
- Generic components that need broad customization should provide and act on a `render___` prop to override their default implementation.
  - e.g. Navbar's `title` (string) prop vs `renderTitle` (function (Component)) prop
- Only use `setState` iff it affects something that should be rerendered and it can not be computed from props.
  - from [Dan](https://twitter.com/dan_abramov/status/749710501916139520):
![](https://pbs.twimg.com/media/CmeBsGzW8AQp_av.jpg)

## Perf
- Do not pass in Array or Object literals to subcomponents. If you do, PureRenderMixin will not work, since `['hi', 'bye'] !== ['hi', 'bye']`. Instead, move that array creation to an instance field or completely outside of the component. This means you should also define inline styles outside of your `render()`.
  - [Performance Engineering with React](http://benchling.engineering/performance-engineering-with-react/)
  - [React.js pure render performance anti-pattern — Medium](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.y7zpsjsu6)

# General
- "Limiting yourself to pure functions as much as possible just makes complex logic *so* much easier to express" - [Henrik Joreteg](https://twitter.com/HenrikJoreteg/status/722654861280550913)

### Many more to come...
