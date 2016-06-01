```
.____            _     ____                 _   _               
| __ )  ___  ___| |_  |  _ \ _ __ __ _  ___| |_(_) ___ ___  ___
|  _ \ / _ \/ __| __| | |_) | '__/ _` |/ __| __| |/ __/ _ \/ __|
| |_) |  __/\__ \ |_  |  __/| | | (_| | (__| |_| | (_|  __/\__ \
|____/ \___||___/\__| |_|   |_|  \__,_|\___|\__|_|\___\___||___/
```

# Redux
[Redux](https://github.com/reactjs/redux) holds your App's state. It is predictable and has great tooling, but might be a bit slower than something like [MobX](https://github.com/mobxjs/mobx) ([Abramov's tweet](https://twitter.com/dan_abramov/status/733705049902329856)).

React makes your UI reactive (hence its name) by treating data from your store as a __fact__. So you don’t have to tell your view ‘how to update the UI.’ In the same way, I also believe Redux/Flux makes your data model reactive, by treating actions as a fact, so you don’t have to tell your data model how to update themselves. - [from @dtinth](https://github.com/reactjs/redux/issues/1171#issuecomment-167714850)

## Rules
* Your __reducers__ must be pure (__deterministic__).
* Any logic with side effects (__non-deterministic__) (external services, async code) belong in an action (via something like [redux-thunk](https://github.com/gaearon/redux-thunk) and/or [redux-saga](https://github.com/yelouafi/redux-saga))
  * For more about the deterministic vs non-deterministic, see [this](https://github.com/reactjs/redux/issues/1171#issuecomment-205888533) Github Issue response.
* Containers read a store's data through selectors. Selectors are your "reading API" and should be __co-located__ with their reducers.
  * See [So you’ve screwed up your Redux store — or, why Redux makes refactoring easy](https://blog.boldlisting.com/so-youve-screwed-up-your-redux-store-or-why-redux-makes-refactoring-easy-400e19606c71#.rho2ned2d)
  * and [Computing Derived Data | Redux](http://redux.js.org/docs/recipes/ComputingDerivedData.html)
* __Use selectors everywhere__. Even for the most trivial ones.
* Redux should store the minimal possible state, allowing Selectors to compute derived data.
* Use [Reselect](https://github.com/reactjs/reselect) for selectors that need to be memoized (like derived data).
* Selectors can be composed of other selectors
* Normalize your data for better reducer composition
  * see the output of [normalizr](https://github.com/paularmstrong/normalizr) for an example

### Reducer composition
* Redux houses your models and __collections__ (of normalized models). It is better to think of it this way, instead of just one big blob of app state.
* Just like Selectors and Components are composable, so too are Reducers!
  * Think of Redux as a tree of your reducers (like react's render tree of components). Both are just compositions of functions.
  * Totally OK to have many, many reducers.
* Dan's videos
  * [Redux: Reducer Composition with Arrays](https://egghead.io/lessons/javascript-redux-reducer-composition-with-arrays)
  * [Redux: Reducer Composition with Objects](https://egghead.io/lessons/javascript-redux-reducer-composition-with-objects?series=getting-started-with-redux)
  * [Redux: Reducer Composition with combineReducers()](https://egghead.io/lessons/javascript-redux-reducer-composition-with-combinereducers?series=getting-started-with-redux)
    * Explains the point of combineReducers: to create the root reducer which branches off into all of your other reducers (which might branch off into children reducers of their own, and so on).

## Structure and Naming
* Use [Ducks](https://github.com/erikras/ducks-modular-redux)
* Use [redux-actions](https://github.com/acdlite/redux-actions)
* action name: `<NOUN>_<VERB>`
* action creator name: `<verb><Noun>`
* selector name: `get<Noun>`

### Actions
* __DO__ name each action (constant) as `<NOUN>_<VERB>` with the present tense
  * e.g. `TODO_ADD`
  * __Why?__ For namespacing and sorting your reducers


* __DO__ build your action creators using [redux-actions](https://github.com/acdlite/redux-actions)' `createActions()`
  * e.g. `createAction( 'TODO_ADD' )`
  * __Why?__ To reduce boilerplate and enforce [FSA](https://github.com/acdlite/flux-standard-action)-compliant actions


* __DO__ name each action creator as `<verb><Noun>`
  * e.g. `const addTodo = createAction( 'TODO_ADD' )`
  * __Why?__ As a convention to clearly identify what type of function it is

### Selectors
* __DO__ name each selector as `get<Noun>`
  * e.g. `const getTodo = (state) => state`
  * __Why?__ As a convention to clearly identify what type of function it is

### Reducers
* __DO__ build your reducers using [redux-actions](https://github.com/acdlite/redux-actions)'s `handleAction()`
  * e.g.

  ```js
  export default handleActions({

    TODO_ADD: (state, action) => ([
      ...state,
      action.payload,
    ]),

    // Other reducers
    // ...
    //

  }, initialState )
  ```
  * __Why__ this instead of the documented `switch` statement? Primarily because it keeps a clean switch-like syntax, while adding block scoping to the cases. This means you can reuse variable of the names in each "case". With `switch`, your `case`s are scoped to the `switch`, so you are forced to use `var` or unique names.

### Files
* __DO__ structure your Redux files (typically in the `/flux` folder) with the [Ducks](https://github.com/erikras/ducks-modular-redux) pattern
  * Here's another helpful [link](https://medium.com/@scbarrus/the-ducks-file-structure-for-redux-d63c41b7035c#.iw6yey65h)
* Example reducer:

```js
/* /flux/todos.js */
import { createAction, handleActions } from 'redux-actions';

//- Actions
export const addTodo = createAction( 'TODO_ADD' )

//- State
const initialState = []

//- Reducers
export default handleActions({

  TODO_ADD: (state, action) => ([
    ...state,
    action.payload,
  ]),

  // Other reducers
  // ...
  //

}, initialState )

//- Selectors
export const getTodos = (state) => state

```

## Types
* If you are typing your code with Flow or Typescript, be sure to type your redux code as well!
* Flow: [Using Redux with Flow](http://frantic.im/using-redux-with-flow) is a nice write-up

## Resources
* [Recommendations for best practices regarding action-creators, reducers, and selectors](https://github.com/reactjs/redux/issues/1171)
  * Really good thread for aligning on fat vs skinny actions
* [Neos CMS Goes for a Full UI Rewrite with React and Redux](http://dimaip.github.io/2016/03/13/neos-react-redux-rewrite/)
* [Redux reducers.. Am I missing something? : javascript](https://www.reddit.com/r/javascript/comments/40n5u3/redux_reducers_am_i_missing_something/)
  * Discussion on what gets its own reducer
