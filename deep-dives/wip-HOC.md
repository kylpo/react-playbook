# Conventions
- see [Official Docs](https://facebook.github.io/react/docs/higher-order-components.html#convention-pass-unrelated-props-through-to-the-wrapped-component)
- `passProps` that aren't related to HOC
- hoist statics
- debug label w/ `displayName`

# Considerations
- `ref`s aren't passed through
  - use `refNode` pattern
  - TODO: link to my article
