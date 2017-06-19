```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# MobX
MobX is an app state manager (used in place of something like Redux)

---

**UPDATE:** Below are fairly old notes by now. Honestly, just read [Mobdux: Combining the good parts of MobX and Redux](https://medium.com/@cameronfletcher92/mobdux-combining-the-good-parts-of-mobx-and-redux-61bac90ee448). It is the best MobX article I've read!

---

## Getting Started
- [Video: 3 mins] [Mobx and React intro ](https://egghead.io/lessons/javascript-mobx-and-react-intro-syncing-the-ui-with-the-app-state-using-observable-and-observer?utm_content=buffer621b3&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)
- [MobX: Ten minute introduction to MobX and React](https://mobxjs.github.io/mobx/getting-started.html)
- Note: the [official docs](http://mobxjs.github.io/mobx/index.html) are also great! Thorough and digestible.

## Uhhh, isn't this just 2-way data binding?
Yes and no. In MobX, anything can `counter++` an observable to change it's state. This kept many people away from trying MobX - we like the predictability of one-way, top-down actions updating our state. Fortunately, MobX responded to this community ask by introducing `@action` and `mobx.useStrict()`. See [MobX 2.2: explicit actions, controlled mutations and improved DX](https://medium.com/@mweststrate/mobx-2-2-explicit-actions-controlled-mutations-and-improved-dx-45cdc73c7c8d#.6h94skvar) for more.

## Learning More
- [MobX tutorial #1 - MobX + React is AWESOME - YouTube](https://www.youtube.com/watch?v=_q50BXqkAfI)
- [MobX tutorial #2 - Computed Values and Nested/Referenced Observables - YouTube](https://www.youtube.com/watch?v=nYvNqKrl69s)
- [Becoming fully reactive: an in-depth explanation of MobX — Medium](https://medium.com/@mweststrate/becoming-fully-reactive-an-in-depth-explanation-of-mobservable-55995262a254#.hlnc7wmuk)
- "mobx observable is really like "thing that notifies observers when it changes". You don't subscribe to them" - [@15lettermax](https://twitter.com/15lettermax/status/763228391377997824)
- [A MobX introduction and case study - We Are Wizards Blog](https://blog.wearewizards.io/a-mobx-introduction-and-case-study)
  - Goes in to Typescript and mobx vs redux/immutable
- [Michel Weststrate on Twitter: "Bunch of excellent tips on how to scale MobX applications: https://t.co/0imjjYENUo by @KatSickk"](https://twitter.com/mweststrate/status/869183911070498817)

## Some Main concepts
- Actions, observable state, computed property values, reactions
- Strive for the smallest possible state. Derive the rest.
- Computed: like formulas in spreadsheets
  - Not allowed to produce side effects
- types of reactions: `observer`, `when`, `autorun`
  - these have side effects
- With an observable array of objects, for example, MobX will automatically make the objects observables. This means child components of the list may need to be Observers to react to the object changes.

## Examples
- [mobx-examples: A collection of simple mobx examples](https://github.com/mattruby/mobx-examples)
  - Note, click on the links in the README to see the examples run in jsfiddle
- [Mobx + Form Validation](https://jsfiddle.net/royriojas/qp5p33cn/)
- [A React-Native app to remote control Google Play Music Desktop](https://github.com/GPMDP/google-play-music-desktop-remote)
- [RFX Stack - Universal App featuring: React + Feathers + MobX](https://github.com/foxhound87/rfx-stack)
- [google-play-music-desktop-remote: A React-Native app to remote control Google Play Music Desktop](https://github.com/GPMDP/google-play-music-desktop-remote)
- [mobx-ssr-example: Server-side rendering with mobx and react-router](https://github.com/kuuup/mobx-ssr-example)
- [MobX React: Refactor your application from Redux to MobX - RWieruch](http://www.robinwieruch.de/mobx-react/)

## Performance
- Any computation is deferred until needed by I/O. Huge performance benefits. Simplifies GC. - [@mwestrate](https://twitter.com/mweststrate/status/734001713800224768)
- Observables benchmarked faster than Immutable. [tweet](https://twitter.com/mweststrate/status/718444275239882753)
![](https://pbs.twimg.com/media/CfhtQs-WwAAzkkW.jpg:large)
- [mobxjs/react-particles-experiment: An experiment to see if you can make a particle generator in redux+react+d3.](https://github.com/mobxjs/react-particles-experiment)

# Best Practices
- __Stores__: Separate your ui-stores (forms, navigation, modals) and domain-stores (models). See [Defining data stores](http://mobxjs.github.io/mobx/best/store.html).
- "If you want to update some observables automatically (e.g. in autorun), you probably should use computed instead" - [mweststrate](https://twitter.com/mweststrate/status/738955213009096704)
- If already using mobX for app state, consider using it in place of React' `setState()`: see [3 Reasons why I stopped using React.setState — Medium](https://medium.com/@mweststrate/3-reasons-why-i-stopped-using-react-setstate-ab73fc67a42e#.802uvhns0)
- To enforce this, use the [no-set-state](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-set-state.md) eslint rule

# Structure
![](https://pbs.twimg.com/media/CnbC3auXEAE4UdZ.jpg:large)
- from [@jay_soo](https://twitter.com/jay_soo/status/754004481524793344)

# Patterns
- [Lazy Loading observables](https://github.com/mobxjs/mobx/issues/307)
- [Server Rendering - Hydration with MobX](https://medium.com/@foxhound87/state-management-hydration-with-mobx-we-must-react-ep-05-1922a72453c6#.ksm0szwm9)

# Questions
### What are the pros/cons versus Redux (flux)?
- Redux uses __Pure Functions__.
- MobX uses __Reactive Programming__.
  - Reactive Programming is making your functions *react to events they are listening in to*, rather than making your buttons (components), http requests, and all other parts of your application *call* your functions. - [video](https://www.youtube.com/watch?v=31URmaeNHSs&feature=youtu.be&t=640)
- Flux (Redux) is designed to answer the question: when does a given slice of state change, and where does the data come from? It is not designed to be the most performant, or the most concise way of writing mutations. Its focus is on making the code predictable. - [Dan Abramov](https://twitter.com/dan_abramov/status/733742952657342464)
- __Redux con__: It is kinda strange in Redux to see `<Something/>` in a render with no props but then has some required propTypes and gets them. - [@ryanflorence](https://twitter.com/ryanflorence/status/736278249458835456)

### Is there a router for MobX?
- Not really. See this thread for some responses: https://twitter.com/DonHansDampf/status/735339826984128513

### How to deal with Promises using `useStrict()`?
[Using the MobX @action decorator with async functions and .then](http://stackoverflow.com/questions/37554828/using-the-mobx-action-decorator-with-async-functions-and-then)

### Possible to use Hot Loader?
- I haven't tried this, but see https://gist.github.com/rej156/4524b59ea77a697fb62a39eada5b9bbe from [this](https://twitter.com/ericjuta/status/737312909521457152) tweet

## Example store from [Janiczek](https://gist.github.com/Janiczek/fdee27d10e933f1d4f948807559c8255):
```js
class Store {

    /**********************************
     * BASE VALUES
     **********************************/

    @observable bpm = 120;
    @observable playing = false;

    /**********************************
     * COMPUTED VALUES
     **********************************/

    @computed get msPerClick() {
        return 60000 / this.bpm;
    }

    @computed get playButtonIcon() {
        return this.playing ? 'stop' : 'play';
    }

    @computed get bpmString() {
        return (this.bpm) ? this.bpm.toString() : '';
    }

    /**********************************
     * ACTIONS
     **********************************/

    @action togglePlaying() {
        this.playing = !this.playing;
    }

    @action setBpmFromString(bpmString) {
        this.bpm = parseFloat(bpmString) || null;
    }

    @action clampBpm() {
        this.bpm = clamp(this.bpm, 1, 999.999);
    }
}
```
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
