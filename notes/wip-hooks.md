# Hooks

> Stop thinking in Lifecycles. **Think about synchronizing side effects to state 🔄**

from [React Hook Pitfalls \- Kent C\. Dodds \- React Rally 2019 \- YouTube](https://www.youtube.com/watch?v=VIRcX2X7EUk&t=517s)

## Rules

From [docs](https://reactjs.org/docs/hooks-overview.html#rules-of-hooks):

> Only call Hooks **at the top level**. Don’t call Hooks inside loops, conditions, or nested functions.
>
> Only call Hooks from React function components. Don’t call Hooks from regular JavaScript functions.

## Ref

Ref is like a class property. It does not affect (re-)rendering.

## Effect

From [docs](https://reactjs.org/docs/hooks-effect.html):

> By using this Hook, you tell React that your component needs to do something after render. React will remember the function you passed (we’ll refer to it as our “effect”), and call it later after performing the DOM updates.
>
> Instead of thinking in terms of “mounting” and “updating”, you might find it easier to think that effects happen **“after render”**.

> The question is not "when does this effect run" the question is "with which
> state does this effect synchronize with"
>
> ```
> useEffect(fn) // all state
> useEffect(fn, []) // no state
> useEffect(fn, [these, states])
> ```
>
> - [Ryan Florence](https://twitter.com/ryanflorence/status/1125041041063665666)

OR...

```js
useEffect(() => {
  // if (componentDidUpdate & (x or y changed))
  setMoveCount(moveCount + 1);
}, [x, y]);
```

`[]` -> first render

`none` -> every render

from https://medium.com/simars/react-hooks-manage-life-cycle-events-tricks-and-tips-7ed13f52ba12

---

`useEffect(yourCallback, [])` - will trigger the callback only after the first render.

useEffect runs by default after every render of the component (thus causing an effect).

When placing useEffect in your component you tell React you want to run the callback as an effect. React will run the effect after rendering and after performing the DOM updates.

If you pass only a callback - the callback will run after each render.

If passing a second argument (array), React will run the callback after the first render and every time one of the elements in the array is changed. for example when placing useEffect(() => console.log('hello'), [someVar, someOtherVar]) - the callback will run after the first render and after any render that one of someVar or someOtherVar are changed.

By passing the second argument an empty array, React will compare after each render the array and will see nothing was changed, thus calling the callback only after the first render.

https://stackoverflow.com/questions/53120972/how-to-call-loading-function-with-react-useeffect-only-once

# Resources

- [Hooks FAQ – React](https://reactjs.org/docs/hooks-faq.html?no-cache=1)
