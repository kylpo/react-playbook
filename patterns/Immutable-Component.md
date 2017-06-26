# A Standard and Naming Convention for Immutable Components
An "Immutable Component" is a component that, after rendering once, can **not** re-render (i.e. it has `shouldComponentUpdate() { return false }`).

*See [this](https://github.com/kylpo/react-playbook/blob/master/deep-dives/shouldComponentUpdate.md) writeup if you'd like more information on `shouldComponentUpdate`*

## When Are Immutable Components Useful?
Many of your components will need to live and respond to `prop` or `state` updates, but there are also likely components that are never updated. Perhaps it is a layout component used for spacing, or a header component with a fixed label. Why should these unchanging components re-`render()`, slowing down the javascript thread? They shouldn't! They should be immutable components.

## The Problem
React's mechanism to create immutable components is within a component's `shouldComponentUpdate()` method, but often this power exists at the wrong level. As a component, I don't always know that I should not re-render. As that component's **parent**, however, I might have more context to know if that component can safely be immutable.

Also, since immutable components prevent re-renders of their `children`, I should probably know how to identify them. Can you tell me which of these components do not re-render?

```jsx
<Col>
  <View padding={20}>
    <Text>Hi there</Text>
  </View>

  <Space size={20} />

  <Text style={styles.bye}>Bye bye now</Text>
</Col>
```

I can’t either. I need prior knowledge of the components, or I need to read through their implementation.

## Introducing the `<IMMUTABLE>` Standard and Naming Convention
The standard is to export a version of your component that sets `sCU => false`. This way, we're empowering the component's parent to make the performance optimization.

The convention is to call that immutable component something in all caps, like `IMMUTABLE`.

> Export an `UPPER_CASE` version of your component that sets `sCU => false`

```jsx
<Col>
  <VIEW padding={20}>
    <TEXT>Hi there</TEXT>
  </VIEW>

  <SPACE size={20} />

  <View padding={20}>
    <Text>{this.state.label}</Text>
  </View>

  <TEXT style={styles.bye}>Bye bye now</TEXT>
</Col>
```

Notice how much easier it is to identify the immutable components?

## It Gets Better
Can you spot the error(s) in this render?

```jsx
  <VIEW padding={20}>
    <Text>{this.state.label}</Text>
  </VIEW>
```
Yes! Because immutable components also prevent their `children` from re-rendering, they should probably not have non-immutable `children`.

## Even Better with Tooling
Naming conventions enable tooling. I've edited my vim color scheme to style immutable components the same as immutable values (e.g. `boolean`, `number`).

![](https://github.com/kylpo/react-playbook/blob/master/assets/IMMUTABLE.png?raw=true)

> more conventions => more helpful tooling

## Future
I would love for React to handle this standard automatically. Just like react handles *lowercase* components (e.g. `<div />`) differently from *PascalCase* composite components, it could handle *UPPER_CASE* components.

And how about going one step further in empowering a component's parent by specifying **per-prop immutability**?! Immutable props would be skipped from the `shouldComponentUpdate()` check. This would essentially give us **configuration** props!

```jsx
<View
  updatingProp={this.state.value}
  IMMUTABLE_PROP='fixed value'
/>
```

Perhaps a babel plugin could come along to handle immutable components and props in the meantime?

## Other naming conventions
- [<Injector_ > Components](https://github.com/kylpo/react-playbook/blob/master/patterns/Injector-Component.md)
- [<\_Null\_ /> Components](https://github.com/kylpo/react-playbook/blob/master/patterns/Null-Component.md)

---

# Notes to self
"Immutable Component" vs "Constant Component":

Chose immutable because immutable can not change after initialized. `const` is a one-time assignment, but it can mutate. `const` would make more sense if you declared the component outside of your class, and used that `const` inside your `render()`.

https://softwareengineering.stackexchange.com/questions/149555/difference-between-immutable-and-const