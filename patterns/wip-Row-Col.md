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