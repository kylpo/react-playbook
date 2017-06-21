```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# Flow
- [Flow type cheat sheet - SaltyCrane Blog](http://www.saltycrane.com/blog/2016/06/flow-type-cheat-sheet/)
- Pro-tip: Put your @flowtype libdefs in a dir called "flow-typed" in the same dir as your .flowconfig. No need for any [libs] configuration! - [tweet](https://twitter.com/lbljeffmo/status/752705329180409856)
- `console` flow-type: https://github.com/jeffmo/jest/blob/fef19c897fcf646e66a7cd6120b4b743846159b5/flow-typed/console.js
- [Did you know you can use capture groups in #flowtype's name_mapper option? E.g. 'mylib/\(.*\)' -> '<PROJECT_ROOT>/dist/lib/\1' .. NEAT!](https://twitter.com/ryyppy/status/787979980714209280)
- How to read error messages: "Basically every error has two parts. They are related. Since Flow is about asserting how types "flow" into each other, an error may happen on one of two sides." - [abramov](https://twitter.com/dan_abramov/status/813794628575068160)

# Tools
- [thejameskyle/babel-plugin-react-flow-props-to-prop-types: Convert Flow React props annotation to PropTypes](https://github.com/thejameskyle/babel-plugin-react-flow-props-to-prop-types). See
[this](https://twitter.com/thejameskyle/status/870762618599817216) for the announcement.
![](https://pbs.twimg.com/media/DBWR8agUQAApEmb.jpg)

# Still some rough patches
- [$Keys types values missing in autocomplete](https://github.com/facebook/flow/issues/4215)
  - Example of editor tooling not being caught up with the actual type checking
- No `@decorator` support

# Style
## `Array<Foo>`, not `Foo[]`
### Why?
- Because when you inevitably need to make it a union, `Array<Foo | Bar>` works but `Foo | Bar[]` doesn't
- Also, what if you had a set? `Set<Foo>`
- See https://twitter.com/spikebrehm/status/832675779070750720

# FAQ
## Children
```jsx
import React from 'react';
import type { Children } from 'react';
type Props = {
  children?: Children,
};
const SampleText = (props: Props) => (
  <div>{props.children}</div>
);
```
from https://stackoverflow.com/questions/40651126/flow-type-annotation-for-children-react-elements

## binding methods in class gives me some "Covariant" errors
Must type them as `Function` in class:
```jsx
class HeaderContainer extends React.Component {

  handleNavigate: Function;
  toggleMenu: Function;

  constructor(props: Object) {
    super(props);
    this.handleNavigate = this.handleNavigate.bind(this);
    this.toggleMenu = this.toggleMenu.bind(this);
  }

  handleNavigate() {
    console.log('hey');
  }

  toggleMenu() {
    console.log('ho');
  }

  render() {
    return (
      <div />
    );
  }
}
```
from https://github.com/ryyppy/flow-guide/issues/6

## DefaultProps get nullable type errors
"You don’t have to mark your defaultProps as optional properties in your props. Flow knows how to handle them properly."
from [official docs](https://flow.org/en/docs/frameworks/react/#toc-adding-types-for-react-component-props)
