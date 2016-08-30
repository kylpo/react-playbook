```
. ____                                             _        _             _     _ _            _                  
 / ___|___  _ __ ___  _ __   ___  _ __   ___ _ __ | |_     / \   _ __ ___| |__ (_) |_ ___  ___| |_ _   _ _ __ ___
| |   / _ \| '_ ` _ \| '_ \ / _ \| '_ \ / _ \ '_ \| __|   / _ \ | '__/ __| '_ \| | __/ _ \/ __| __| | | | '__/ _ \
| |__| (_) | | | | | | |_) | (_) | | | |  __/ | | | |_   / ___ \| | | (__| | | | | ||  __/ (__| |_| |_| | | |  __/
 \____\___/|_| |_| |_| .__/ \___/|_| |_|\___|_| |_|\__| /_/   \_\_|  \___|_| |_|_|\__\___|\___|\__|\__,_|_|  \___|
                      |_|                                                                                          
```
# Part 2: A Single Component
First, let's identify the primary types of any single component.

## Types
(aka separating concerns)

### Presentational
Presentational components are your __ui__ components. They should only react to `props` passed down (and __local__ `state` if absolutely necessary). They’re concerned about how it __looks__, and should hold little to no logic.

Example: The ui of a Button

[Read More](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.uy1ewt283)

```
Presentational

####################
####################
####################
####################
```

Note: This is a __component__

### Container
Container components are the inverse of presentational components: they only care about how it __works__. Instead of rendering multiple components in its render, it will typically render just one, and be sure to pass `props` to that component based on the logic it cares about (this means it will have __no stylesheets__, by the way).

Example: Wrapping a Button to specify the `onPress` functionality

[Read More](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.uy1ewt283)
```
Container

  --------------------
 |*                   |*
*|                   *|
 |*                   |*
*|                   *|
  --------------------
[*] represents communication inside and out
  ```

Note: This is a __component__

### Both
```

Presentational                  Container
                                 --------------------
####################            |*                   |*
####################  _ _ _ _  *|                   *|
####################     |      |*                   |*
####################     |     *|                   *|
                         |       --------------------
                         |
- - - - - - - - - - - - -|- - - - - - - - - - - - - - -
                         |
              Both       V
               --------------------
              |* ################  |*
             *|  ################ *|
              |* ################  |*
             *|  ################ *|
               --------------------
```

Note: This combination is also a __component__

A single component may share the responsibilities of a Presentational and Container, and is likely the way you currently build your components if you had not heard of the Container/Presentational pattern.

Good practice if the container and presentational are extracted into separate files (contained in a folder, called a __Folder Component__), but likely a code smell if they are combined into one big file. Having said that, we don't have all the time in the world, so sharing responsibilities in a single file is OK, but it should be small, focussed, and be refactored when it grows.

# File/Folder structure
Speaking of file and folder components, lets see exactly how they would be represented in the file system.

#### A file component
`/MyComponent.js` - Presentational

`/MyComponentContainer.js` - Container

#### A folder component
```
/MyComponent
  ├── /MyComponent.js
  ├── /index.js
```

Are you wondering why an `index.js` was added? Well, it is needed to keep the `import`s unchanged.

For example, let's import the file component:
```JavaScript
import MyComponent from './MyComponent'
```
Now, let's decide to convert the file component to a folder component. We create the folder, and move the .js file into it (note: no `index.js` currently).

``!ERROR! Can not find './MyComponent'.`

Oops, our iriginal path is no longer valid. To fix it, you can change your import to:
```JavaScript
import MyComponent from './MyComponent/MyComponent'
```

But do you really want to refactor all of your code imports every time you convert a file component to a folder component? And what about when you add a Container to that folder component? Will you want to change all imports from `'./MyComponent/MyComponent'` to `'./MyComponent/MyComponentContainer'`? Me neither. That's why we need an `index.js` like
```JavaScript
//index.js
import Root from './MyComponent'
export default Root
```
Now, your previous import of `import MyComponent from './MyComponent'` will work.

Aside: Eventually, I will likely build a tool to convert a file component to a folder component. It will auto-gen this `index.js` file, similar to [create-index](https://github.com/gajus/create-index).

#### Full folder structure
There are just a couple more optional pieces to add to a folder component: tests and assets.

##### Tests
When running tests, it is handy to keep them with the component being tested. This is why the community has adopted a convention of a `/__tests__/` folder right next to the component.

##### Assets
If a component is the only one using certain images, fonts, or videos, the component author might find it best to store it in an `/assets/` folder inside the folder component.

This is the most complicated a component can get (so far):
```
/MyComponent
  ├── /__tests__/
  ├── /assets/
  ├── /MyComponent.js
  ├── /MyComponentContainer.js
  ├── /index.js
```
Note: `__tests__` and `assets` should only exist if there is something in them. Do not create the empty folder by default.

## Summary
- A component can be Presentational, Container, or both.
- A component can exist on the file system as a single file or within its folder.

## Learn More
[Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.uy1ewt283)

## Next Up: [Multiple Components](https://github.com/kylpo/react-playbook/blob/master/component-architecture/3_Multiple-Components.md)
