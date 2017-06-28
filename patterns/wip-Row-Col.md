# The Case for `<Row>` (and `<Col>`)
React Native's base set of components are amazing! `<View>` enforcing `flexbox` and defaulting to `{flexDirection: 'column', position: 'relative'}`? *So* smart.

It is such a successful set of components and constraints to work with that web developers (like me) are bringing them to the web. Twitter Lite using react-native-for-web and Leland's react-primitives, for example.

I'd like to add a couple new members to this base set's layout components.


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