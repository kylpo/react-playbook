```
._   _       _            
| \ | | ___ | |_ ___  ___
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/

```

# Redux
Redux is a state manager. It is a small library (2kB) that relies on you to follow certain rules to operate as expected.

Characteristics of Redux:
- state is represented as one, big, read-only object in the Redux store
- state may only be changed by emitting (dispatching) an action
- pure reducers specify how state is transformed by an action
- middleware extension points between actions and reducers enable things like logging, side effects, etc.

Particularly useful with React, where `UI = fn(state)`. Some applications go as far to not allowing any local component state, only allowing global, Redux state so that stat history may be kept or serialized for debugging and caching.

## Terms

> A **store** is a state (object) container

> A **provider** is used with React to pass your store down the component tree via React's Context

> An **action** is a plain object that describes what happened. It usually has a `type` property.

> An **action creator** is a function that returns an action

> A **reducer** transforms the current state into the next state

> A **selector** is a function that *reads* from the store's state

## Rules

- Only one Redux store per app
- Do not mutate state directly
- Reducers must be *pure* (have no side effects)

## Recommendations

- [Normalize](https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape) the data in your store
- Store the minimal amount of data needed, compute derived data with selectors

## Examples

Mostly from [Core Concepts \| Redux](https://redux.js.org/introduction/core-concepts)

```js
// app state
{
  todos: [{
    text: 'Eat food',
    completed: true
  }, {
    text: 'Exercise',
    completed: false
  }],
  visibilityFilter: 'SHOW_COMPLETED'
}

// actions
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }

// action creator
function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

// reducers
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter
  } else {
    return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([{ text: action.text, completed: false }])
    case 'TOGGLE_TODO':
      return state.map((todo, index) =>
        action.index === index
          ? { text: todo.text, completed: !todo.completed }
          : todo
      )
    default:
      return state
  }
}

// combined reducers
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}

// selectors
const getTodos = state => state.todos
```

## Pitfalls and Solutions

### Selectors are rerun/computed every time a component updates

Every time the state tree updates, the selector is recalculated. This may be fine for simple getters, but if it is computing a value, like the sum of all items in an array, then it may cause performance problems.

**Solution**: memoize selectors with [reselect](https://github.com/reduxjs/reselect) to only recompute the selector when a value it uses changes. Else it returns the cached value.

### Reducers can get complicated

Reducers are simple in theory, but can quickly get out of hand and complicated. [immer](https://github.com/immerjs/immer) provides a nicer API for producing next states, and is only 3kB in size.

> Create the next immutable state tree by simply modifying the current tree

```js
// immer intro
import produce from "immer"

const baseState = [
    {
        todo: "Learn typescript",
        done: true
    },
    {
        todo: "Try immer",
        done: false
    }
]

const nextState = produce(baseState, draftState => {
    draftState.push({todo: "Tweet about it"})
    draftState[1].done = true
})
```

```js
// Redux reducer
const byId = (state, action) => {
    switch (action.type) {
        case RECEIVE_PRODUCTS:
            return {
                ...state,
                ...action.products.reduce((obj, product) => {
                    obj[product.id] = product
                    return obj
                }, {})
            }
        default:
            return state
    }
}

// Using immer
import produce from "immer"

const byId = produce((draft, action) => {
    switch (action.type) {
        case RECEIVE_PRODUCTS:
            action.products.forEach(product => {
                draft[product.id] = product
            })
    }
})
```

### Async Operations and Side Effects

Remember, reducers are not allowed to have side effects. When you want to do asynchrnous things like data fetching, you'll need to add middleware to respond to new types of actions. There are some established libraries to accomplish this:

#### Redux Thunk (functions)

[redux-thunk](https://github.com/reduxjs/redux-thunk) ("thunk" is a function that wraps an expression to delay its evalution) allows action creators to return a function instead of an action object. The inner function receives the store method's `dispatch` and `getState`.

```js
const INCREMENT_COUNTER = 'INCREMENT_COUNTER';

function increment() {
  return {
    type: INCREMENT_COUNTER,
  };
}

function incrementAsync() {
  return (dispatch) => {
    setTimeout(() => {
      // Yay! Can invoke sync or async actions with `dispatch`
      dispatch(increment());
    }, 1000);
  };
}

function incrementIfOdd() {
  return (dispatch, getState) => {
    const { counter } = getState();

    if (counter % 2 === 0) {
      return;
    }

    dispatch(increment());
  };
}
```

#### Redux Pack (Promises)
> [redux-pack](https://github.com/lelandrichardson/redux-pack) is a library that introduces promise-based middleware that allows async actions based on the lifecycle of a promise to be declarative
>
> Async actions in redux are often done using redux-thunk or other middlewares. The problem with this approach is that it makes it too easy to use dispatch sequentially, and dispatch multiple "actions" as the result of the same interaction/event, where they probably should have just been a single action dispatch.
>
> This can be problematic because we are treating several dispatches as all part of a single transaction, but in reality, each dispatch causes a separate rerender of the entire component tree, where we not only pay a huge performance penalty, but also risk the redux store being in an inconsistent state.
>
> `redux-pack` helps prevent us from making these mistakes, as it doesn't give us the power of a `dispatch` function, but allows us to do all of the things we were doing before.

```js
// types.js
export const LOAD_FOO = 'LOAD_FOO';

// actions.js
export function loadFoo(id) {
  return {
    type: LOAD_FOO,
    promise: Api.getFoo(id),
  };
}

// reducer.js
import { handle } from 'redux-pack';

// handle the action with redux-pack's handle function, where you can specify several smaller "reducer" functions for each lifecycle
export function fooReducer(state = initialState, action) {
  const { type, payload } = action;
  switch (type) {
    case LOAD_FOO:

      return handle(state, action, {
        start: prevState => ({
          ...prevState,
          isLoading: true,
          fooError: null
        }),
        finish: prevState => ({ ...prevState, isLoading: false }),
        failure: prevState => ({ ...prevState, fooError: payload }),
        success: prevState => ({ ...prevState, foo: payload }),
      });
    default:
      return state;
  }
}
```

#### Redux Saga (Generators)

A Generator is a special type of function that returns an iterator for `.next()`ing through the evaluation of the function to the next `yield` or `return`. The `next()` method returns an object with a `value` property containing the yielded value and a `done` property which indicates whether the generator has yielded its last value.

[Redux-Saga](https://github.com/redux-saga/redux-saga/) uses generators for asynchronous flows.

> The mental model is that a saga is like a separate thread in your application that's solely responsible for side effects. redux-saga is a redux middleware, which means this thread can be started, paused and cancelled from the main application with normal redux actions, it has access to the full redux application state and it can dispatch redux actions as well.

```js
import { call, put, takeEvery, takeLatest } from 'redux-saga/effects'
import Api from '...'

// worker Saga: will be fired on USER_FETCH_REQUESTED actions
function* fetchUser(action) {
   try {
      const user = yield call(Api.fetchUser, action.payload.userId);
      yield put({type: "USER_FETCH_SUCCEEDED", user: user});
   } catch (e) {
      yield put({type: "USER_FETCH_FAILED", message: e.message});
   }
}

/*
  Starts fetchUser on each dispatched `USER_FETCH_REQUESTED` action.
  Allows concurrent fetches of user.
*/
function* mySaga() {
  yield takeEvery("USER_FETCH_REQUESTED", fetchUser);
}

/*
  Alternatively you may use takeLatest.

  Does not allow concurrent fetches of user. If "USER_FETCH_REQUESTED" gets
  dispatched while a fetch is already pending, that pending fetch is cancelled
  and only the latest one will be run.
*/
function* mySaga() {
  yield takeLatest("USER_FETCH_REQUESTED", fetchUser);
}

export default mySaga;
```

Common effects: (`yield ___()`)
- `call` - call any function and get the return value. Supports promises.
- `select` - call a selector
- `put` - `dispatch` an action
- `cancel` - abort the execution of a forked task

## Dev tools

One of the best reasons to use Redux. See all of the amazing tools at [Ecosystem \| Redux](https://redux.js.org/introduction/ecosystem#dev-tools).

## Resources