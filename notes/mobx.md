```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# MobX
MobX is an app state manager (used in place of something like Redux)

## Getting Started
- [Video: 3 mins] [Mobx and React intro ](https://egghead.io/lessons/javascript-mobx-and-react-intro-syncing-the-ui-with-the-app-state-using-observable-and-observer?utm_content=buffer621b3&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)
- [MobX: Ten minute introduction to MobX and React](https://mobxjs.github.io/mobx/getting-started.html)
- Note: the [official docs](http://mobxjs.github.io/mobx/index.html) are also great! Thorough and digestible.

## Uhhh, isn't this just 2-way data binding?
Yes and no. In MobX, anything can `counter++` an observable to change it's state. This kept many people away from trying MobX - we like the predictability of one-way, top-down actions updating our state. Fortunately, MobX responded to this community ask by introducing `@action` and `mobx.useStrict()`. See [MobX 2.2: explicit actions, controlled mutations and improved DX](https://medium.com/@mweststrate/mobx-2-2-explicit-actions-controlled-mutations-and-improved-dx-45cdc73c7c8d#.6h94skvar) for more.

## Examples
- [mobx-examples: A collection of simple mobx examples](https://github.com/mattruby/mobx-examples)
  - Note, click on the links in the README to see the examples run in jsfiddle
- [A React-Native app to remote control Google Play Music Desktop](https://github.com/GPMDP/google-play-music-desktop-remote)


## Performance
- Any computation is deferred until needed by I/O. Huge performance benefits. Simplifies GC. - [@mwestrate](https://twitter.com/mweststrate/status/734001713800224768)
- Observables benchmarked faster than Immutable. [tweet](https://twitter.com/mweststrate/status/718444275239882753)
![](https://pbs.twimg.com/media/CfhtQs-WwAAzkkW.jpg:large)


## Server rendering
- [State Management & Hydration with MobX](https://medium.com/@foxhound87/state-management-hydration-with-mobx-we-must-react-ep-05-1922a72453c6#.ksm0szwm9)


# Best Practices
- __Stores__: Separate your ui-stores (forms, navigation, modals) and domain-stores (models). See [Defining data stores](http://mobxjs.github.io/mobx/best/store.html).


# Questions
### What are the pros/cons versus Redux (flux)?
- Redux uses __Pure Functions__.
- MobX uses __Reactive Programming__.
  - Reactive Programming is making your functions *react to events they are listening in to*, rather than making your buttons (components), http requests, and all other parts of your application *call* your functions. - [video](https://www.youtube.com/watch?v=31URmaeNHSs&feature=youtu.be&t=640)
- Flux (Redux) is designed to answer the question: when does a given slice of state change, and where does the data come from? It is not designed to be the most performant, or the most concise way of writing mutations. Its focus is on making the code predictable. - [Dan Abramov](https://twitter.com/dan_abramov/status/733742952657342464)
- __Redux con__: It is kinda strange in Redux to see `<Something/>` in a render with no props but then has some required propTypes and gets them. - [@ryanflorence](https://twitter.com/ryanflorence/status/736278249458835456)

### Should I pass the observable value all the way down to my children?
No, the components that care about the observable should just import it. This has an perf improvement, as well.
- See [Do child components need `@observer`? · Issue #101 · mobxjs/mobx](https://github.com/mobxjs/mobx/issues/101#issuecomment-220891704) thread

### Is there a router for MobX?
- Not really. See this thread for some responses: https://twitter.com/DonHansDampf/status/735339826984128513

### How to deal with Promises using `useStrict()`?
[Using the MobX @action decorator with async functions and .then](http://stackoverflow.com/questions/37554828/using-the-mobx-action-decorator-with-async-functions-and-then)

### Possible to use Hot Loader?
- I haven't tried this, but see https://gist.github.com/rej156/4524b59ea77a697fb62a39eada5b9bbe from [this](https://twitter.com/ericjuta/status/737312909521457152) tweet

## Web Setup/DevTools
from [mobx-react-boilerplate](https://github.com/mobxjs/mobx-react-boilerplate/blob/master/src/index.jsx)
```js
import React, {Component} from 'react';
import ReactDOM from 'react-dom';
import {observable} from 'mobx';
import {observer} from 'mobx-react';
import DevTools from 'mobx-react-devtools';

const appState =  new class AppState {
    @observable timer = 0;

    constructor() {
        setInterval(() => {
            appState.timer += 1;
        }, 1000);
    }

    resetTimer() {
        this.timer = 0;
    }
}();

@observer
class TimerView extends Component {
     render() {
        return (
            <div>
                <button onClick={this.onReset}>
                    Seconds passed: {this.props.appState.timer}
                </button>
                <DevTools />
            </div>
        );
     }

     onReset = () => {
     	this.props.appState.resetTimer();
     }
};

ReactDOM.render(<TimerView appState={appState} />, document.getElementById('root'));
```

## React Native Setup
Same as what is shown in Web's Debugger, but `import { observer } from 'mobx-react/native'` instead of just from `mobx-react`

### Dev Tools
There are no official native dev tools yet. You can, however, log each action with something like this in your `app.js`:
```js
// log all mobx actions when in development mode
if (__DEV__) {
  mobx.spy( ev => {
    if (ev.type === "action") {
      console.log( ev.name )
    }
  })
}
```

# Resources
- [Why we chose MobX over Redux for Spectacle Editor](http://formidable.com/blog/2016/06/02/why-we-chose-mobx-over-redux-for-spectacle-editor/)
