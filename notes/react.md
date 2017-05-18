```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# React Core understanding
jsx converts
```js
<div height={5} >
  <Child />
</div>
```
into
```js
React.createElement('div', {height:5},
  React.createElement(Child, {})
)
```
`React.createElement()` returns a `reactElement`, which could be a single node, or tree of nodes.

Note: this means even if the parent div did not render Child, React will still create the Element. This is an argument for using Child Functions for conditionally rendering components. See the Usage section of [react-media](https://github.com/ReactTraining/react-media#usage) for more.

# Patterns
## RefNode
```js
refNode={node => this.node = node}
ref={this.props.refNode}
```

## HOCs
Basically a HOC enhances a component with new capabilities. This way you can abstract common behaviors into reusable pieces.

## Types of HOCs
Wrappers -> wrap children and introduce something to dom

Manipulators (or Injectors) -> pass props (injects props into child(ren))

## Function as Children
[Ryan Florence on Twitter: "@wincent @timbucki @sebmck I think we're talking about <Thing>{() => ()}</Thing> v. <Thing render={() => ()}/> In which case, I, too, prefer the "attribute" form."](https://twitter.com/ryanflorence/status/865302840696225792)

# Security
- For SSR, be sure to sanitize your stores that you serialize (for later hydration) to prevent XSS
  - [The Most Common XSS Vulnerability in React.js Applications](https://medium.com/node-security/the-most-common-xss-vulnerability-in-react-js-applications-2bdffbcc1fa0#.vlnhi9gcv)

# Interesting ideas
- [You might not need React Router — Free Code Camp](https://medium.freecodecamp.com/you-might-not-need-react-router-38673620f3d#.z27bsomqb)
