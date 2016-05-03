```
*
   ____                                             _        _             _     _ _            _                  
  / ___|___  _ __ ___  _ __   ___  _ __   ___ _ __ | |_     / \   _ __ ___| |__ (_) |_ ___  ___| |_ _   _ _ __ ___
 | |   / _ \| '_ ` _ \| '_ \ / _ \| '_ \ / _ \ '_ \| __|   / _ \ | '__/ __| '_ \| | __/ _ \/ __| __| | | | '__/ _ \
 | |__| (_) | | | | | | |_) | (_) | | | |  __/ | | | |_   / ___ \| | | (__| | | | | ||  __/ (__| |_| |_| | | |  __/
  \____\___/|_| |_| |_| .__/ \___/|_| |_|\___|_| |_|\__| /_/   \_\_|  \___|_| |_|_|\__\___|\___|\__|\__,_|_|  \___|
                      |_|                                                                                          
```
# Part 4: Component Dependencies
Ah, we're here! The crucial part of our component architecture: defining app-dependent and independent components.

# !NOT YET WRITTEN! Below are just notes

## App-Dependent
Presentational
PropType using an App model's type

Container
Button example: (w/o app state: Alert, w/ app state: change theme)

## Independent
Lib components (and utils) are independent of the app. They should not know anything about the app’s state or models.

May compose: Presentational and Container ONLY in /lib, node_modules

Lib criteria:
- generic (independent)
- sufficiently generic ~~and used enough times or is sufficiently complex to warrant~~
- if specific to scene/parent, leave it in Scene
- can only depend on others in lib or node_modules
- a certain amount of pride
- uppercase means Component, lower means util
- must broadcast its own constants/utils, not depend on anything in App/

File/function names as generic as possible (less brittle to change)
e.g. Button, InlineButton, ListItem, Input

?Question? Are there independent containers?
yes

With utils, don't shy away from many modular functions. The JIT with inline them, so no perf issues.

## Going further
This app-dependent vs independent distinction is import for ALL functions, not just components! Strive to make your functions as generic as reasonable - you will notice the same scaling, maintainable benefits as components.

## Learn More

## Next Up: [Example App Structure](https://github.com/kylpo/react-playbook/blob/master/component-architecture/5_Example-App-Structure.md)
