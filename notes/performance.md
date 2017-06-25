```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# React Performance

## Official Docs
[Optimizing Performance - React](https://facebook.github.io/react/docs/optimizing-performance.html) docs have dramatically improved. Be sure to read through it!

## Keep stateful components shallow
Reduce re-renders by keeping stateful components shallow. Given a render like:

```jsx
<Col
  center
  grow
>
  <Text size={20}>Home</Text>

  <Link to='other'>Other</Link>

  <Event_ onClick={() => this.setState({label: 'changed'})}>
    <Style_ border='1px solid #111'>
      <View padding={16}>
        <Text size={16}>{this.state.label}</Text>
      </View>
    </Style_>
  </Event_>
</Col>
```

When `this.state.label` changes, **all** of these subcomponents will re-render, not just the relevant `Text`. So, if possible, keep your stateful components as shallow as possible. In this case, we can extract out the stateful component:

```jsx
<Event_ onClick={() => this.setState({label: 'changed'})}>
  <Style_ border='1px solid #111'>
    <View padding={16}>
      <Text size={16}>{this.state.label}</Text>
    </View>
  </Style_>
</Event_>
```

Shallow stateful components also play nicely with the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle).

#### Reference
- "One of the best tactics that you can apply early on is to try to keep components shallow. It will help you see which props components actually use and cut an extra re-render computation." - comment of [this post](https://marmelab.com/blog/2017/02/06/react-is-slow-react-is-fast.html)
- "By removing the draft Tweet state from updating the main Redux state on every keypress and keeping things local in the React component’s state, we were able to reduce the overhead by over 50% (above, right)." - [source]() https://medium.com/@paularmstrong/twitter-lite-and-high-performance-react-progressive-web-apps-at-scale-d28a00e780a3)

## The deeper your render tree, the longer diffing will take
![](https://pbs.twimg.com/media/DBo1eKqV0AEms32.jpg)

#### Reference
- "yup. that increases recursion in the diff, which is a nontrivial cost." - [@_developit](https://twitter.com/_developit/status/872068281628282880)

## Place connected container components as deep in the tree as possible
The higher your connected container components, the more subcomponents will re-render on app store changes. So, try to place them as deep as possible. It is totally OK to have multiple connected components to the same store.

#### Reference
Thought about while reading https://dev.bleacherreport.com/3-things-i-learned-about-working-with-data-in-redux-5fa0d5f89c8b#.ij978jnby