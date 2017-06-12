# All About Refs
# Refs: Beyond the Basics
`ref` is one of React's magical props, in that it is used by React itself, not by the component that attach the `ref` to. If it is placed on a primitive like `<div ref={...} />`, it will give you the DOM node instance. If it is placed on a composite component like `<MyComponent ref={...} />`, it will give you the component instance.

Cool, you likely already knew that, but did you know:

If you haven't read the official docs or haven't read it in a year or so, I recommend you read it. It even covers functional components not being `ref`-able.

I do have a few things to add to the topic that justify this post though.

https://github.com/kylpo/react-playbook/blob/master/best-practices/react.md#refs

https://facebook.github.io/react/docs/refs-and-the-dom.html#caveats

## When to use ref

## no ref for functional components
- You may not use the ref attribute on functional components because they don't have instances. https://facebook.github.io/react/docs/refs-and-the-dom.html#refs-and-functional-components

## ref callbacks
- callbacks, how to avoid boilerplate? called on when unmounted (passes null)?
- how to reduce boilerplate with something like https://github.com/fresk-nc/babel-plugin-transform-jsx-ref-to-function?
- wish I could have a `setRef` prop that just accepts a pointer to my instance field.
OK, fine, callback refs replaced string refs. This gives us more power! But, dang, this does add more boilerplate.
linkRef

[developit/linkref: Like Linked State, but for Refs. Works with Preact and React.](https://github.com/developit/linkref)


- [Jason Miller 🦊⚛ on Twitter: "@aweary This is one reason I recommend linkRef: https://t.co/dEpsDrOrIT https://t.co/euiZDxFF8W"](https://twitter.com/_developit/status/859384258498101250)
- [James Kyle on Twitter: "@_developit @aweary @preactjs Here you go https://t.co/QT6YrAaNBE"](https://twitter.com/thejameskyle/status/859420749844680708)

## refnode

## when is it set?

## called with null
- [Dan Abramov on Twitter: "Ever wondered why callback refs get called with null during the updates? I wrote a bit about this: https://t.co/42kdCy0eKF"](https://twitter.com/dan_abramov/status/859159065498406913)
- [Glen Mailer on Twitter: "@dan_abramov That's a very long way to say "null means unmount, use it as a signal to perform cleanup" 😉😊"](https://twitter.com/glenathan/status/859161300668166146)

## onLayout
- https://facebook.github.io/react/docs/refs-and-the-dom.html#caveats

https://github.com/necolas/react-native-web/issues/60

https://github.com/constelation/monorepo/issues/79
