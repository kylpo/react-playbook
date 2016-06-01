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

# General
- "Limiting yourself to pure functions as much as possible just makes complex logic *so* much easier to express" - [Henrik Joreteg](https://twitter.com/HenrikJoreteg/status/722654861280550913)

### Many more to come...
