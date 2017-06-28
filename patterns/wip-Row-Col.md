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

Hmmm, all of a sudden, when reading my JSX, I don't **know** that they'll render top-down. Either of these newly introduced styles may modify the `flexDirection`. So, I need to jump to the code that defines these styles to figure out how I should read the JSX. This jumping back and forth inhibits the skimability and efficient of your code. It'd be like reading a book where you frequently need to look up words in a dictionary.

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

Within `View`, since there isn't a `style` prop, I can read top-down. Then when I step in to `Row`, I can read left-right.

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
<Col style={styles.outer}>
  <Text>Hi</Text>
  <View style={styles.inner}>
    <Text>Hi again</Text>
    <Text>Bye</Text>
  </View>
</Col>
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
With [constelation](https://github.com/constelation/monorepo), a prototyping framework, we've taken `Row` and `Col` a bit further by adding `alignVertical: 'top' | 'center' | 'bottom'`, `alignHorizontal: 'left' | 'center' | 'right'`, `reverse` for `'column-reverse'` and `'row-reverse'`, and some other convenience props.

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

*This post has already gotten longer than I intended. If you have any more questions, please check out the full write-up in the react-playbook.*

---

React Native gives us a really smart set of primitives out of the box. `View` is one of these primitives, and it forces the use of flexbox, but also importantly defaults to `flexDirection: 'column'`.


In React Native, we are given View.

Made our own primitives, immediately built Row, Col, can now do convenient things like `alignHorizontal`.

`horizontal` prop explored for consistency with `ScrollView`

---

Had a mini breakthrough on our `<View horizontal >` discussion. I realized having `Row` is REALLY useful for reading jsx as a dev. Specifically, it tells to me “instead of reading components top-down, now read them left-right”. This of course is what `<View horizontal >` tells me, but it fails in with the ending tag. The skimmer can not easily see _when_ to stop reading left-right, especially if the children jsx height is tall enough to not see the opening and closing `View` tag on the screen. SO, I’m thinking about bring it back in some form. (edited)

Here’s a code sample with `Row` in it:

 ```jsx
     <Style_ borderTop={`1px solid ${COLOR.E5}`}>
        <View
          marginTop={32}
          paddingBottom={this.props.label === 'Color' ? 0 : 28}
        >
          <View
            // paddingTop={28}
            paddingHorizontal={this.props.label === 'Color' ? 15 : 30}
            // paddingHorizontal={30}
          >
            <View
              paddingTop={28}
              paddingBottom={this.props.label === 'Color' ? 10 : 18}
              paddingHorizontal={this.props.label === 'Color' ? 15 : 0}

              // paddingLeft={30}
              >
              <Text
                size={UIState.fontSize.body}
                bold
                >
                {this.props.label}
              </Text>
            </View>
            <Row
              wrap='wrap'
              marginTop='8px'
              height={this.props.contentHeight}
            >
              {Options.slice(0, this.props.visibleCount)}
              { (this.props.label === 'Size') && (
                <View
                  paddingTop={20}
                >
                  <Text
                    size={UIState.fontSize.body}
                    >
                    + Extended sizes
                  </Text>
                </View>
              )}
              {
                Options.slice(this.props.visibleCount, Options.length).length ?
                  <MoreOptions filters={Options.slice(4, Options.length)} /> : null
              }
            </Row>
          </View>
        </View>
      </Style_>
```

So easy to see when I am supposed to read top-down vs left-right.

This is a good reason to bring back something like `Flex`, too.


Does this mean we should also have a `ScrollRow` and `ScrollCol`? Well, yeah, actually. That would be nice.