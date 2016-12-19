```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```


# React Native
## Updating/Upgrading
Consult [Breaking Changes](https://github.com/facebook/react-native/wiki/Breaking-Changes) before updating react-native

## How does it work?
Javacript Core runs in a separate thread and talks to native iOS modules via RCTBridge. Native iOS modules must use certain annotations to recieve and send to RN components.

## Guides
[Introduction · Rangle.io: React Native Workshop](https://rangle-io.gitbooks.io/react-native-workshop/content/)

## Animations
### Performance
- [Tal Kol - Introduction to React Native Performance - YouTube](https://www.youtube.com/watch?v=9VqVv_sVgv0&index=56&list=WL) is a must-watch!
- [React Native Internals: A Wider Picture (Part 1: MessageQueue & JS Thread)](https://medium.com/@rotemmiz/react-native-internals-a-wider-picture-part-1-messagequeue-js-thread-7894a7cba868#.qtq5sohfe)

## Debugging
- [rn-snoopy: Snoopy is a profiling tool for React Native, that lets you snoop on the React Native Bridge.](https://github.com/jondot/rn-snoopy)
- [react-native-debugger: The standalone app based on official debugger of React Native, with React Inspector / Redux DevTools](https://github.com/jhen0409/react-native-debugger)
- [Debugging React Native Applications – React Native Academy – Medium](https://medium.com/reactnativeacademy/debugging-react-native-applications-6bff3f28c375#.z8ln1acta)

## Tricks/Hacks
Need something accessible from every file? Add it to `global` in your `index.{ios|android}.js`.
- Found this in [initializeCore.js](https://github.com/facebook/react-native/blob/master/Libraries/Core/InitializeCore.js) from [package defaults](https://github.com/facebook/react-native/blob/master/packager/defaults.js)
