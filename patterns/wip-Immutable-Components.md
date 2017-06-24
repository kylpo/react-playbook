# A Standard and Naming Convention for Immutable Components
An "Immutable Component" is a component that, after rendering once, can not re-render. i.e. it has `shouldComponentUpdate() { return false } `

*See [this](https://github.com/kylpo/react-playbook/blob/master/deep-dives/shouldComponentUpdate.md) writeup if you'd like more information on `shouldComponentUpdate`*


## When Are Immutable Components Useful?

## Can You Spot Them?
Can you tell me which of these components do not re-render?

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

React's primary mechanism for tuning performance is with a component's `shouldComponentUpdate()` lifecycle. Unfortunately, the component itself needs to specify that it should not update, but often this is better known at its parent level, where the component is actually used. So, I propose we have a standard and convention for tuning performance at the parent level.

The standard is to always export a version of your component that sets `scu => false`. The convention is to call that Immutable Component like IMMUTABLE.

## Introducing the `<IMMUTABLE>` Component Naming Convention

`<SPACE size={40} />` means we can optimize `scu => false`

`<DIV className={} />` would also be useful

Would love for React to handle this automatically. Just like react auto-handles lowercase components, it could handle UPPER_CASE components and PROPS.

## It Gets Better

## Even Better with Tooling

## Other naming conventions
- [<Injector_ > Components](https://github.com/kylpo/react-playbook/blob/master/patterns/Injector-Component.md)
- [<\_Null\_ />](https://github.com/kylpo/react-playbook/blob/master/patterns/Null-Component.md) Components
---

# Notes to self
Immutable vs Constant:

Chose immutable because immutable can not change after initialized. `const` is a one-time assignment, but it can mutate. `const` would make more sense if you declared the component outside of your class, and used that `const` inside your `render()`.

https://softwareengineering.stackexchange.com/questions/149555/difference-between-immutable-and-const