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
- WHY? (from [abramov](https://news.ycombinator.com/edit?id=12093234)
  - String refs are not composable. A wrapping component can’t “snoop” on a ref to a child if it already has an existing string ref. On the other hand, callback refs don’t have a single owner, so you can always compose them.
  - String refs don’t work with static analysis like Flow
  - The owner for a string ref is determined by the currently executing component. This means that with a common “render callback” pattern (e.g. `<DataTable renderRow={this.renderRow} />`), the wrong component will own the ref (it will end up on `DataTable` instead of your component defining `renderRow`).
- DO NOT use `findDOMNode()`
  - use callback refs instead [abramov tweet](https://twitter.com/dan_abramov/status/752936646602031104)

## Context
- Always make sure context is shallowly immutable, avoids issues with PureRenderMixin components not propagating context changes
  - [mweststrate tweet](https://twitter.com/mweststrate/status/764182336888078336)
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
- "Don’t stress over binding in render() too much. In my experience it makes a real difference in maybe 10% of cases."
  - [abramov tweet](https://twitter.com/dan_abramov/status/760199672824815616)
- "Don‘t use PureComponent everywhere. Measure."
  - [abramov tweet](https://twitter.com/dan_abramov/status/759383530120110080)
- "What are the performance implications of wrapping most #React components in HoC? It can double the # of lifecycle methods called."
  - [brehm tweet](https://twitter.com/spikebrehm/status/760593924184432640)
- Higher order components should be wrapped before render()
  - [abramov](https://github.com/krasimir/react-in-patterns/issues/12)

```js
var OriginalComponent = () => <p>Hello world.</p>;
var EnhancedComponent = enhanceComponent(OriginalComponent);

class App extends React.Component {
  render() {
    return React.createElement(EnhancedComponent);
  }
};
```

# General
- "If a parent needs to know about it, the parent owns it"
- "Limiting yourself to pure functions as much as possible just makes complex logic *so* much easier to express" - [Henrik Joreteg](https://twitter.com/HenrikJoreteg/status/722654861280550913)
- "Multiple simple components are better than one highly customisable one" from [11 lessons learned as a React contractor](https://medium.com/@jolyon_russ/11-lessons-learned-as-a-react-contractor-f515cd0491cf#.liewu56oc)

## Testing
Write component tests that accomplish the following goals (from [Getting Started with TDD in React](https://semaphoreci.com/community/tutorials/getting-started-with-tdd-in-react?utm_source=javascriptweekly&utm_medium=email)):
* it renders
* it renders the correct thing
  * default props
  * varied props
* it renders the different states
* test events
* test edge cases
  * e.g. something that uses an array should be thrown an empty array

"Testing exact render is bad, but testing props have correct impact in render is big. Also events and lifecycle." - [@FwardPhoenix](https://twitter.com/FwardPhoenix/status/757591796641914880)

"Jest snapshot testing + inline style + css-layout = auto layout regression prevention." - [cheng lou](https://twitter.com/_chenglou/status/758461301307748353)

### Many more to come...
