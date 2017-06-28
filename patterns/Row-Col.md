# The Case for `<Row>` (and `<Col>`)
React Native's base set of components are amazing! `<View>` enforcing `flexbox` and defaulting to `{flexDirection: 'column', position: 'relative'}`? **So** smart.

It is such a successful set of components and constraints that web developers (like me) are bringing them to the web. See [react-native-web](https://github.com/necolas/react-native-web) and [react-primitives](https://github.com/lelandrichardson/react-primitives) for two great examples of this.

Now, this is getting a bit nitpicky, but I believe there is a good case for adding just a couple more layout components to your toolkit. Whether these components should exist at the framework, primitive level or in user-space is up for debate.

## The Problem
Given what we know about `View`, how do you expect the code below to render in your app?

```jsx
<View>
  <Text>Hi</Text>
  <Text>Hi again</Text>
  <Text>Bye</Text>
</View>
```

Well, because `View` defaults to `flexDirection: 'column'`, and there are no style overrides, I expect the text to render as I read it: top-down.

What if I add another `View` into the code?

```jsx
<View>
  <Text>Hi</Text>
  <View>
    <Text>Hi again</Text>
    <Text>Bye</Text>
  </View>
</View>
```

No change here. The `direction` of each is still `column`, so I can read it top-down.

Now let's add some styles to these `View`s. How will they render?

```jsx
<View style={styles.outer}>
  <Text>Hi</Text>
  <View style={styles.inner}>
    <Text>Hi again</Text>
    <Text>Bye</Text>
  </View>
</View>
```

Hmmm, all of a sudden, when reading my JSX, I don't **know** that they'll render top-down. Either of these newly introduced styles may modify the `flexDirection`. So, I need to jump to the code that defines these styles to figure out how I should read the JSX. This jumping back and forth inhibits the skimability and efficiency of your code. It is like reading a technical book, where you frequently need to look up words in a dictionary.

## The Solution
Let's add `Row` into the mix. `Row`, for now, just **forces** its `flexDirection` to `'row'`. How do you expect this to render?

```jsx
<Row>
  <Text>Hi</Text>
  <Text>Hi again</Text>
  <Text>Bye</Text>
</Row>
```

As I read top-down within `Row`, I **know** it'll render left-right.

Another example:

```jsx
<View>
  <Text>Hi</Text>
  <Row>
    <Text>Hi again</Text>
    <Text>Bye</Text>
  </Row>
</View>
```

Within `View`, since there isn't a `style` prop, I can read top-down. Then when I step in to `Row`, it tells me I can read left-right.

Now, we could just stop here with adding `Row` to our toolkit, but `View` will often have a `style` prop, so lets formalize things with adding `Col` as well. `Col`, for now, just **forces** its `flexDirection` to `'column'`.

```jsx
<Col style={styles.outer}>
  <Text>Hi</Text>
  <Row style={styles.inner}>
    <Text>Hi again</Text>
    <Text>Bye</Text>
  </Row>
</Col>
```

Cool, now I **know** that I can read top-down within `Col`, despite it having a `style` prop. And I **know** that I can read left-right within `Row`, despite it also having a `style` prop.

Hot dang! That's some nice and easy reading.

## When To Use `View`
In this new system, `View` is still necessary for two use cases:

1) A variable `flexDirection`

```jsx
<View style={style}>
  <Text>Hi again</Text>
  <Text>Bye</Text>
</View>
```

`View` signals to the reader that they can't just keep skimming here. They need more context to know if they should read top-down, left-right, bottom-up, or right-left.

2) It only has one child

```jsx
<View>
  <Text>Hi</Text>
</View>
```

With one child, the direction does not matter, so we signal that to the reader by using `View`.

## Going Further
With [constelation](https://github.com/constelation/monorepo), the prototyping framework my team uses, we've taken `Row` and `Col` a bit further by adding `alignVertical: 'top' | 'center' | 'bottom'`, `alignHorizontal: 'left' | 'center' | 'right'`, `reverse` for `'column-reverse'` and `'row-reverse'`, and some other convenience props.

```jsx
<Row
  reverse
  alignHorizontal='right'
  alignVertical='center'
>
  <Text>Hi</Text>
  <Text>Bye</Text>
</Row>
```

## Some Questions You May Have

#### Why not follow `ScrollView`'s lead of using a `horizontal` prop?
We actually went down this road! We first started with `Row` and `Col`, then switched to just `View` with a `horizontal` prop for the consistency and ease of teaching. Shortly after we made the switch, however, we missed the clarity of having `Row` and `Col`. The explicitness helps, but also, the **closing tag** remains descriptive!

Compare this partial JSX

```jsx
    <Something />
    <OtherThing />
  </View>
  <Text>Hi</Text>
  <Text>Bye</Text>
</View>
```

with this

```jsx
    <Something />
    <OtherThing />
  </Col>
  <Text>Hi</Text>
  <Text>Bye</Text>
</Row>
```

In both cases, they render the same thing, but the JSX of the `Col` and `Row` is instantly understood. No scrolling up needed.

#### Does this mean we should have `ScrollRow` and `ScrollCol`?
Well yeah, actually, that would be nice. We have not yet done this, but I don't see how it could be a bad thing.

#### Why not replace `View` with something more descriptive, like `Flex`?
I would like to do this. Or maybe just add a `Flex` for when `flexDirection` is variable, and leave `View` for the case that there is only one child. My team is not yet unified on this, however, so it has not landed in [constelation](https://github.com/constelation/monorepo).