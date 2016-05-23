
```
*
  ____            _     ____                 _   _               
 | __ )  ___  ___| |_  |  _ \ _ __ __ _  ___| |_(_) ___ ___  ___
 |  _ \ / _ \/ __| __| | |_) | '__/ _` |/ __| __| |/ __/ _ \/ __|
 | |_) |  __/\__ \ |_  |  __/| | | (_| | (__| |_| | (_|  __/\__ \
 |____/ \___||___/\__| |_|   |_|  \__,_|\___|\__|_|\___\___||___/

```

# Redux

*!!Actively being written!!*

* Redux holds the app's state
  * The reducer is built using the handy [redux-actions](https://github.com/acdlite/redux-actions) lib
  * It is organized with the [Ducks](https://github.com/erikras/ducks-modular-redux) pattern
  * Here's another helpful [link](https://medium.com/@scbarrus/the-ducks-file-structure-for-redux-d63c41b7035c#.iw6yey65h)
* Containers read a store's data through selectors. These selectors should be co-located with their reducers.
  * See [So you’ve screwed up your Redux store — or, why Redux makes refactoring easy](https://blog.boldlisting.com/so-youve-screwed-up-your-redux-store-or-why-redux-makes-refactoring-easy-400e19606c71#.rho2ned2d)
  * and [Computing Derived Data | Redux](http://redux.js.org/docs/recipes/ComputingDerivedData.html)
* Just like react's render tree of components, think of Redux as a tree of your reducers. (both are just compositions of functions)
* Redux is not just your app state. It is your collections (of normalized models). See normalizr for an example.
* Reselect to combine into derived.
  * Allows redux to store minimum viable state
  * Selectors are your reading API for your components to access your store.
* Selectors are composable

## Reducer composition
* Dan's videos
  * [Redux: Reducer Composition with Arrays](https://egghead.io/lessons/javascript-redux-reducer-composition-with-arrays)
  * [Redux: Reducer Composition with Objects](https://egghead.io/lessons/javascript-redux-reducer-composition-with-objects?series=getting-started-with-redux)
  * [Redux: Reducer Composition with combineReducers()](https://egghead.io/lessons/javascript-redux-reducer-composition-with-combinereducers?series=getting-started-with-redux)
    * Explains the point of combineReducers: to create the root reducer which branches off into all of your other reducers (which might branch off into children reducers of their own, and so on).
* [Redux reducers.. Am I missing something? : javascript](https://www.reddit.com/r/javascript/comments/40n5u3/redux_reducers_am_i_missing_something/)
  * Discussion on what gets its own reducer
