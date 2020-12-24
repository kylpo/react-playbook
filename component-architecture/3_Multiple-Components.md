```
. ____                                             _        _             _     _ _            _                  
 / ___|___  _ __ ___  _ __   ___  _ __   ___ _ __ | |_     / \   _ __ ___| |__ (_) |_ ___  ___| |_ _   _ _ __ ___
| |   / _ \| '_ ` _ \| '_ \ / _ \| '_ \ / _ \ '_ \| __|   / _ \ | '__/ __| '_ \| | __/ _ \/ __| __| | | | '__/ _ \
| |__| (_) | | | | | | |_) | (_) | | | |  __/ | | | |_   / ___ \| | | (__| | | | | ||  __/ (__| |_| |_| | | |  __/
 \____\___/|_| |_| |_| .__/ \___/|_| |_|\___|_| |_|\__| /_/   \_\_|  \___|_| |_|_|\__\___|\___|\__|\__,_|_|  \___|
                      |_|                                                                                          
```
# Part 3: Multiple Components

In part 2, we identified 3 types of components, with 2 different file representations that could form a single, basic component like a Button. This is how it might look in the render tree:

```
<other components>
       |
       |
       O <--- our component (file or folder)
```

Cool, now let's make a more interesting one. Let's say it is a component that renders a Button and a Label.
```
<other components>
       |
       |
       O <--- our component (folder)
       |
     -----
    |     |
L-> O     O <-Button
```
Looking at the render tree, we see that our new component is composed of __subcomponents__! A subcomponent can either be a presentational, a container, or a node_module.

Since this Label and Button component are only used by our component, we could say that they are *scoped* to our component. Since this whole series is about describing and structuring our components, how would we best describe that Label and Button are subcomponents, and scoped only to our component?

Easy, just nest them in our component folder! Our component folder will now look something like this:
```
/MyComponent
  |
  ├── /subcomponents/
  |     ├── Button.js
  |     └── Label.js
  |
  ├── /MyComponent.js
  ├── /MyComponentContainer.js
  └── /index.js
```

This is so helpful. We are structuring our apps the way the render tree will render them!

---

__FAQ: Why use nesting over a big, flat `components/` folder?__

A single, flat `components/` folder works really well in small, example projects and tutorials, but just doesn't scale. As your project grows in complexity and size, your `components/` folder bloats with namespaced components and/or generic component names that have too many concerns or too specific of concerns.

And as this folder grows, the activation energy to refactor and simplify components becomes greater. To prevent bloating it further, you might choose to break up a complicated render into inline subcomponents via `render___()` or defining stateless functional components in your file. Why is this a problem? Well, what happens when this subcomponent gains complexity? Do you need to refactor it out into a new file? Do you make more inline subcomponents and bloat the current file? It doesn't matter! You shouldn't need to think this much.

These problems are avoided if you use the nested components structure from the get-go. The nesting is mindlessly repeatable, and encourages you to do the right thing. Refactor away! No activation energy required!

---

For completeness, lets see what the folder structure would look like if Button was a folder component with subcomponents:
```
/MyComponent
  |
  ├── /subcomponents/
  |     ├── /Button
  |     |   ├── /subcomponents/
  |     |   |   ├── /Box.js
  |     |   |   └── /Text.js
  |     |   ├── /Button.js
  |     |   └── /index.js
  |     └── Label.js
  |
  ├── /MyComponent.js
  ├── /MyComponentContainer.js
  └── /index.js
```

Structurally, this looks great, but there is definitely an eye sore with the name */subcomponents/*. Let's quickly image that we are using our text editor's Fuzzy Search to open a file. We press something like CMD-P, and start typing in Box.js to open it. This is what we'll see:

`/MyComponent/subcomponents/Button/subcomponents/Box.js`

Yikes! And this is only 2 levels deep! `subcomponents` is a perfectly descriptive term, but dirties the fuzzy search. We could change it to something like `parts`, `sub`, etc, but really, any *word* will remain dirty. This is why a symbol will be needed here.

##### New Convention: `subcomponent` directories will be labeled with an `_`

Example:
```
/MyComponent
  |
  ├── /_/
  |     ├── /Button
  |     |   ├── /_/
  |     |   |   ├── /Box.js
  |     |   |   └── /Text.js
  |     |   ├── /Button.js
  |     |   └── /index.js
  |     └── Label.js
  |
  ├── /MyComponent.js
  ├── /MyComponentContainer.js
  └── /index.js
```
Now when we fuzzy search for Box.js, we'll see:

`/MyComponent/_/Button/_/Box.js`

Notice how easily the nesting is to see and read? "`MyComponent` composes `Button`, which composes `Box`".

## Scenes
This component nesting will continue on until a __Scene__ is reached. A __Scene__ is any (folder) component that can be routed to, and is essentially the largest a component can get.

Note: we chose `Scene` over `Page`, `Screen`, `Route`, `View` because it is an established convention for Mobile dev. Since web doesn't have one established name for this, the Mobile convention wins.

```
/app
 ├── /scenes
 |    ├── /Login/
 |    |    ├── ...
 |    ├── /Home/
 |    |    ├── /_/
 |    |    |   ├── /MyComponent/
 |    |    |   |    ├── ...
 |    |    ├── Home.js
 |    |    └── /index.js
```

^ shows a `Home` scene that uses `MyComponent`

##### What about nested scenes?
You can nest scenes by simply adding a `/scenes/` folder in one of your scenes: e.g. `/scenes/Home/scenes/Dashboard`. For smaller apps, it can be nice to just use one, flat `scenes` folder though.

## Sharing Components
So far, our components have conveniently been the only users of their subcomponents. Now, this must change.

What if `MyComponent` and `Button` used the same `Text` component? Where should `Text` go?

Keeping with the nesting theme and writing components that contain their dependencies, the best place would be in a `/shared/` directory at the highest node in the render tree of the two components using it. In this case, it'd be at the `MyComponent` level, like

```
/MyComponent
  |
  ├── /_/
  |    └── /Button
  |         ├── /Button.js       // import Text from 'shared/Text'
  |         └── /index.js
  ├── /shared/
  |    └── /Text.js
  |
  ├── /MyComponent.js            // import Text from 'shared/Text'
  └── /index.js
```

We, again, are able to see the scope of this shared `Text` component because of where it is located.

For the web, this structure is possible using Webpack's [resolve.modulesDirectories](https://webpack.github.io/docs/configuration.html#resolve-modulesdirectories) as explained in [this gist](https://gist.github.com/ryanflorence/daafb1e3cb8ad740b346#shared-module-resolution).

For mobile, however, we are not using Webpack, so this structure is not yet possible.

### Sharing Components in React Native
Since we can not resolved multiple `/shared/` as though they were a `/node_modules/` folder in React Native, we are forced to only have one, top-level `/shared/` directory.

##### New Convention: Any shared app components live in the top-level `/shared/` folder and are imported from `'shared/<<Component>>'`

```
/app
 ├── /scenes
 |    ├── ...
 ├── /shared
 |    ├── /FolderComponent/
 |    |    ├── ...
 |    └── /Text.js
 ...
```

Note: Eventually, this `/shared/` folder might contain too many components to remain flat. If it needs to be broken up into subfolders, try to organize it by __scene__.

## Summary
- A component can contain __subcomponents__ in its `/_/` folder.
- A __Scene__ is a top-level, routed-to component and exists in the `/scenes/` folder
- __Shared__ app components exist in the `/shared/` folder

Nested folders show dependence and scope. Need to use this code in another place, too? Put it in their shared ancestor. Simple.

Next up, we'll explore app-dependent and independent components!

## Next Up: [Component Dependencies](https://github.com/kylpo/react-playbook/blob/master/component-architecture/4_Component-Dependencies.md)
