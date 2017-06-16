_Work In Progress_

## Keep Stateful Components Shallow
# Reduce re-renders by keeping stateful components shallow
Spawned from "One of the best tactics that you can apply early on is to try to keep components shallow. It will help you see which props components actually use and cut an extra re-render computation." - comment of https://marmelab.com/blog/2017/02/06/react-is-slow-react-is-fast.html

Would it be best to keep components as small as possible. No letting the depth increase by more than, say 3 components and height be no more than 3 sibling components?

That way a when a single component in a render needs to be updated, only a few other things will re-render, not many unrelated components.

How would this apply to Page level components?

"By removing the draft Tweet state from updating the main Redux state on every keypress and keeping things local in the React component’s state, we were able to reduce the overhead by over 50% (above, right)." - https://medium.com/@paularmstrong/twitter-lite-and-high-performance-react-progressive-web-apps-at-scale-d28a00e780a3

- https://twitter.com/_developit/status/872068281628282880
- ![](https://pbs.twimg.com/media/DBo1eKqV0AEms32.jpg)
- "Ok, that make sense. The more depth your tree has the more time the diffing will take."

---
Connect components that are as deep in the tree as possible. Changes affect fewer components for updates. Vs changing something at the root.

Thought about while reading https://dev.bleacherreport.com/3-things-i-learned-about-working-with-data-in-redux-5fa0d5f89c8b#.ij978jnby
---

[Optimizing Performance - React](https://facebook.github.io/react/docs/optimizing-performance.html)
