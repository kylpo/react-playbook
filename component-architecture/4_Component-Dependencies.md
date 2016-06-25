```
. ____                                             _        _             _     _ _            _                  
 / ___|___  _ __ ___  _ __   ___  _ __   ___ _ __ | |_     / \   _ __ ___| |__ (_) |_ ___  ___| |_ _   _ _ __ ___
| |   / _ \| '_ ` _ \| '_ \ / _ \| '_ \ / _ \ '_ \| __|   / _ \ | '__/ __| '_ \| | __/ _ \/ __| __| | | | '__/ _ \
| |__| (_) | | | | | | |_) | (_) | | | |  __/ | | | |_   / ___ \| | | (__| | | | | ||  __/ (__| |_| |_| | | |  __/
 \____\___/|_| |_| |_| .__/ \___/|_| |_|\___|_| |_|\__| /_/   \_\_|  \___|_| |_|_|\__\___|\___|\__|\__,_|_|  \___|
                      |_|                                                                                          
```
# Part 4: Component Dependencies
Ah, we're here! The crucial part of our component architecture: defining app-dependent and independent components.

## Independent
As javascript developers, we write many functions. Some functions are app specific by, for example, relying on a model's instance being passed in or cause some app-level side effect like an api call. And some are completely generic and app independent by relying on simple primitives for its input and having no app-level side effects. We certainly benefit from high level app-dependent functions, but the real gems are those independent ones. Independent functions are less susceptible to change when a feature, datastructure, etc changes, and they can be composed together for truly powerful results. This is why functional programming is all the rage these days, and why [Lodash](https://lodash.com/) is the most depended on npm module.

Just like independent functions, __independent components__ are the real gems! Some examples:
- presentational components that map to your style guide
- presentational components that wrap animations
- presentational components of generic layouts
- any component generic enough to make public

But where should these independent components live? Remember that a goal of our component architecture is to help identify the type of components based on where they live in the file structure.

What about `/app/independent`?

Well, having app-independent code in scope of `app` doesn't really make much sense. So it should be outside of `/app`.

What about in `/lib`?

`lib` was a common folder name for 3rd party libraries before the npm and bundler era. It was a naming convention that clearly denoted independent, generic code. This is where we'll place our independent components and functions! It is outside of `/app`, and anyone modifying or creating within it will know what criteria it needs to meet. For more on the `lib` name, see [this tweet](https://twitter.com/housecor/status/745962536751632385).

#### What is the criteria for `lib` components and functions?
- no imports of app code
- no knowledge of the app's state or models
  - does not enforce PropTypes that reflect an app's model
- can only depend on others in lib or node_modules
- must broadcast its own constants/utils if they do not already exist in lib
- uppercase means Component, lower means function (a util or helper)
- file/function names as generic as possible (less brittle to change)
  - e.g. Button, InlineButton, ListItem, Input
- a certain amount of *pride* in its implementation

Tip: don't shy away from building a lot of small, single-focussed components and functions. //TODO: see perf section for JIT inlines and babel inline transforms

## App-Dependent
If independent components live in `/lib`, then app-dependent components must live in `/app`. Simple. They will be *scenes*, *shared*, and *subcomponents*.

Note: this means you will have presentational components that are apps-specific! This is a good thing - just like app-specific functions.

Some app-dependent component examples:
- containers interacting with redux/relay
- presentational that rely on an app model as a proptype
- presentational that compose app-dependent components (container or presentational)

## Summary
- You will have many app-dependent components. This is OK!
- You should strive to have as many app-independent components as possible, just as you are already writing your best practice functions.
  - app-independent components live in the `/lib` root directory

## Next Up: [Example App Structure](https://github.com/kylpo/react-playbook/blob/master/component-architecture/5_Example-App-Structure.md)
