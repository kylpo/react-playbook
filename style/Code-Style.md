I am a web and mobile developer, using React and React Native. As such, my style has evolved to work more seamlessly with React, and may not work for other frameworks. Also, I LOVE Ruby’s (and CoffeeScript’s) syntax. It taught me the value of code that reads more like english and how aesthetics of a code base truly will affect your and others excitement to maintain/contribute to it.

# Code Style

## Semicolons
#### No

```javascript
// Bad
import Foo from 'foo';
let foo;

// Good
import Foo from 'foo'
let foo
```

#### Why

I don’t have any real beef with them, but my code looks better, is more consistent, and faster to write.
* Looks better is subjective, so I won't go into it. Just look at examples or try it before you decide, please.
* More consistent because my js looks as my js looks in jsx.
  * For example:
  * `<div>{someJavascriptCode()}</div>`
  * vs
  * `<div>{someJavascriptCode();}</div>`
  * Which are you used to seeing in your renders?
* Faster to code. More than just not hitting the `;`, also the time saved from the linter error telling you to add it. You all know you’re guilty of the linter error from time to time.

## Quotes
#### Single
```javascript
// Bad
const double = "double"

// Good
const single = 'single'
```
#### Why?
I used to prefer double because it is the only valid json quote, and it is used in html. Now, with the rise of transpilers, and me using less .json, I prefer the lazy approach of the `non-shift-'`. It is faster to type, and more importantly, faster to edit. For example, in vim, `ci'` is honestly way less taxing than `ci<shift>'`.

## Spacing
The following spacing rules are used to satisfy the "reads more like english" request. In general, pad spaces are used around parameters and arguments, and not used around types.

### Paren spacing in function args and params?
#### Yes
```javascript
// Bad
function myFunc(one, two, three) { ... }

myFunc(a, b, c)

// Good
function myFunc( one, two, three ) { ... }

myFunc( a, b, c )
```
#### Except: desctructured params
```javascript
// Good
function myFunc({ one, two, three }) { ... }
```
#### Except: arrow params
```javascript
// Good
const arrow = (one, two, three) => { ... }

// Good
array.sort( (item, index) => ... )
```
#### Why
With arrow params, the added spacing is redundant since it is already padded with spacing outside of the parens.

### Paren spacing in statement expressions (like if, switch, logic)?
#### No
```javascript
// Bad
if ( x > y ) { ... }

// Good
if (x > y) { ... }
```
#### Why
Here, the added spacing in the `if` expression is redundant since it is already padded with spacing outside of the parens.

### Curly spacing in objects
#### No
```javascript
// Bad
const obj = { a: 'a', b: 'b' }

// Good
const obj = {a: 'a', b: 'b'}
```
#### Why
The lack of spacing clearly denotes that this is a type, and is not destructured.

### Curly spacing in destructuring assignments?
#### Yes
```javascript
// Bad
import {MyComponent} from 'components'
const {user} = this.props

// Good
import { MyComponent } from 'components'
const { user } = this.props
```
#### Why
The spacing here is redundant, but also really useful for skimming code and distinguishing destructuring from objects.

### Bracket spacing in array inits
#### No
```javascript
// Bad
const obj = [ 1, 2, 3 ]

// Good
const obj = [1, 2, 3]
```
#### Why
The lack of spacing clearly denotes that this is a type, and is not destructured.

### Bracket spacing in object reference and destructuring
#### Yes
```javascript
// Bad
const [a, b] = someArray
const a = someArray[0]

// Good
const [ a, b ] = someArray
const a = someArray[ 0 ]
```
#### Why
Destructuring for reasons above. Space in reference for "reading like english", same as function args/params.

## if/else
### spacing
```javascript
// Bad
if (isCondition1) {
} else if (isCondition2) {
} else {
}

// Good
if (isCondition1) {
}
else if (isCondition2) {
}
else {
}
```
#### Why
All revolves around vertical spacing. The more popular style of housing `else` on the closing `}` just clutters up the content of condition1 too much. Also, using the more spaced out version that I prefer has revealed refactoring opportunities for more complex nested if statements. Win win. See [this](https://twitter.com/jlongster/status/834407714965057540) twitter thread for other reasons.

# Code Conventions

## if/else commenting
```javascript
// Whole if/else comment (likely not needed though)
if (isCondition1) {
  // if condition 1, do...
}
else if (isCondition2) {
  // if condition 2, use...
}
else {
  // otherwise, pass off...
}
```

Comment on first line of `if` block IF a comment is necessary. Also using the `if _condition_, <verb>` format is useful for the reader skimming just the comments.

## Function approach
You want to get right to the meat of your function’s purpose. Do not defensively code! This should be done in the function’s parameters.
```javascript
// Bad
function doSomeCoolLogic( name, names ) {
  if (typeOf name === 'string' && Array.isArray( names )) {
   return names.includes( name )
  }
}

// Good
function doSomeCoolLogic( name = 'default', names = [] ) {
  return names.includes( name )
}
```

## Handler function naming
Passing handlers down as props is common in React.
* Use __handles__EventName if it is the originating handler for an event
* Use __on__EventName if it is a function that is calling an onEventName that was passed down to it

## Sort function naming
sort functions called **by**__.
```javascript
function byNameAscending( a, b ) { ... }

array.sort( byNameAscending() )
```

## Use `get` functions for derived and computed props/state
_EXPERIMENTAL_
```jsx
get fullName() {
  return `${this.props.firstName} ${this.props.lastName}`
}

render() {
  return (
    <Text>{this.fullName}</Text>
  )
}
```
### Reference
[react-bits/03.computed-props-state.jsx](https://github.com/vasanthk/react-bits/blob/master/conventions/03.computed-props-state.jsx)

# React-specific conventions
In general, keep your components as small and focussed as possible (and reasonable). The conventions below will help with this.

## Imports order
Structure your imports like: functions, constants, __vertical space__, then components. Each block should be ordered as external, then local.
```javascript
// Good
import React from 'react'                                 // external
import { connect } from 'react-redux'                     // external
import { ANIMATION_TIMING } from 'constants'              // local

import {
  View,
  Text,
  TouchableOpacity,
} from 'react-native'                                     // external
import NavBar form 'react-native-navbar'                  // external
import MyAnimatedComponent from './MyAnimatedComponent'   // local

class MyComponent extends React.Component { ... }
```

#### Why
This allows you to more easily skim and parse a component's dependencies.

## Stateless functional components
Gone are the days of renderSection(). Now you should be using a stateless functional component. Note, any exported component should probably still be the class style so that you have access to shouldComponentUpdate and such. Otherwise, consider Recompose.
```javascript
// Bad
class MyComponent extends React.Component {
 renderItem() {
  return <div/>
 }

 render() {
  return (
   <div>
    {this.renderItem()}
   </div>
  )
 }
}

// Good
const Item = props => <div/>

class MyComponent extends React.Component {
 render() {
  return (
   <div>
    <Item />
   </div>
  )
 }
}
```

#### Why
Why not use render___() in components? They spiral out of control. You should be building subcomponents instead!

## Extract non-this functions out of component
Utility functions that do not actually use any instance properties should not exist within the component definition. They should be above the component definition, or in some separate, imported helpers file.
```javascript
// Bad
class MyComponent extends React.Component {
 getNamesFromItems( items = [] ) {
  return items.find( item => item.name )
 }

 render() {
  const names = this.getNamesFromItems( this.props.items )

  return ( ... )
 }
}

// Good
function getNamesFromItems( items = [] ) {
  return items.find( item => item.name )
}

class MyComponent extends React.Component {
 render() {
  const names = getNamesFromItems( this.props.items )

  return ( ... )
 }
}
```

## inline styles
Inline styles place at the bottom of the file for RN (and web React if using something like Radium).

## _privateMethod vs publicMethod
Since ~99% of components will use props, and not be accessed via public methods, prefer the `publicMethod` format for all functions, and clearly comment when one is truly public (in that a component will use `this.refs.myComponent.publicMethod()`).
```javascript
// Bad
class MyComponent extends React.Component {
 ...

 _resetScrollValue() {
  this.setState({ scrollValue: 0 })
 }

 scrollTo( scrollValue = 0 ) {
  this.setState({ scrollValue })
 }
 ...
}

// Good
class MyComponent extends React.Component {
 ...

 resetScrollValue() {
  this.setState({ scrollValue: 0 })
 }

 /* PUBLIC */
 scrollTo( scrollValue = 0 ) {
  this.setState({ scrollValue })
 }
 ...
}
```

# Some good resources
[9 things every React.js beginner should know - Cam Jackson](https://camjackson.net/post/9-things-every-reactjs-beginner-should-know)
