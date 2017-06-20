# A Naming Convention for Null Components
A "Null Component" is a component that returns `null`, and likely uses lifecycle hooks to make side effects.

```jsx
class NullComponent extends React.Component {
  // Potentially perform side effect in a lifecycle method

  render() {
    // Potentially perform side effect

    return null
  }
}
```
*(I got the name from this oooold issue: https://github.com/facebook/react/issues/1058)*

## Can You Spot Them?
Can you tell me which of these components renders nothing?

```jsx
import { Redirect } from 'react-router'
import MyComponent from './my-component'

<View>
  <Redirect />
  <MyComponent />
</View>
```

I can’t either. I need prior knowledge of the components, or I need to read through their implementation.

If you are familiar with React Router V4, you may know that it doesn't render anything, but what if you could know without prior knowledge?

## Introducing the `<_NullComponent_ />` Naming Convention
Aligning closely with the [<Injector_ >](https://github.com/kylpo/react-playbook/blob/master/patterns/Injector-Component.md) naming convention, Null Components are denoted with a **prefix** and **postfix** `_` (e.g. `<_NullComponent_ />`). 

> Null Components are denoted with a prefix and postfix `_`

The idea is that the additional prefix `_` of a Null Component symbolizes that it adds even less to the DOM than an Injector. I picture the name falling through to become just the `_`, like the unused argument convention in javascript functions: `const handleEvent = (_, id) => { console.log(id) }`.

Cool, let's look at our example again with the naming convention applied. Which component(s) render nothing?

```jsx
import { Redirect as _Redirect_ } from 'react-router'
import MyComponent from './my-component'

<View>
  <_Redirect_ />
  <MyComponent />
</View>
```

`<_Redirect_ />` jumps out as a Null Component. I don't need to read its source or have prior knowledge of it's implementation to know this.

How about one more example, with an Injector Component added. How many components are rendered to the DOM?

```jsx
<View>
  <Style_>
    <Text />
  </Style_>

  <_Redirect_ />
</View>
```

By convention, we know that `Style_` and `_Redirect_` do not add anything to the DOM. So, we can mentally comment them out:

```jsx
<View>
  {/*<Style_>*/}
    <Text />
  {/*</Style_>*/}

  {/*<_Redirect_ />*/}
</View>
```

I count 2 in the above code.

## It Gets Better
Can you spot the error(s) in this render?

```jsx
<_MyComponent_>
  <Child />
</_MyComponent_>
```

Yes! Null Components should never have `children` - they'd never be rendered.

## Even Better with Tooling
Naming conventions enable tooling. I've edited my vim color scheme to style Null Components similarly to comments, which further reinforces their concept, and allows me to easily skim renders and identify or ignore them.

![](https://github.com/kylpo/react-playbook/blob/master/assets/NullComponent.png?raw=true)

In the future, we might also have an editor with a toggleable option to hide all components that do not add to the DOM directly.

> more conventions => more helpful tooling

## Other naming conventions
- [<Injector_ > Components](https://github.com/kylpo/react-playbook/blob/master/patterns/Injector-Component.md)
- [<IMMUTABLE /> Components]()