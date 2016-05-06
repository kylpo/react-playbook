```
*
   ____                                             _        _             _     _ _            _                  
  / ___|___  _ __ ___  _ __   ___  _ __   ___ _ __ | |_     / \   _ __ ___| |__ (_) |_ ___  ___| |_ _   _ _ __ ___
 | |   / _ \| '_ ` _ \| '_ \ / _ \| '_ \ / _ \ '_ \| __|   / _ \ | '__/ __| '_ \| | __/ _ \/ __| __| | | | '__/ _ \
 | |__| (_) | | | | | | |_) | (_) | | | |  __/ | | | |_   / ___ \| | | (__| | | | | ||  __/ (__| |_| |_| | | |  __/
  \____\___/|_| |_| |_| .__/ \___/|_| |_|\___|_| |_|\__| /_/   \_\_|  \___|_| |_|_|\__\___|\___|\__|\__,_|_|  \___|
                      |_|                                                                                          
```
# Part 1: The "Component" problem
If I said something was a "file", what would that mean to you? Likely not much - you’d have to ask follow-up questions. Is it physical or digital? Is it text? Is it an image? Excel? Word?

What about a "module"? Same problem.

And a "component"? Sure, you’d know that it is an abstraction of ui and/or logic, built in a modular way to facilitate composing it with other components, but is that really descriptive enough? If talking to non-developers, yes. But when talking to other developers, hell NO!

This is the "Component" problem: the React community is throwing around the word "component" too loosely. For developers new to React, they read articles and watch talks about building these perfect, modular components. "Simply drop the tag with the right props in your render". But when they go off to build their own apps and components, they struggle with why their components are so tightly coupled to the app's features. THIS IS OK! You will have components that should and can never be modular enough for re-use in other apps or components. You will also be able to build more generic ones that can be used in multiple places in your app - maybe even generic enough to be used by other people's apps. Just like how you've been building your Functions.

This Component Architecture series is an attempt to align us on some terms, patterns, rules, and folder structures (collectively called Component Architecture) to help us manage the complexities of building REAL apps with components.

## Goals
- File structure should help identify the type of components and functions
- Easy enough to be automated - no activation energy, mindlessly repeatable
- Scalable and maintainable
  - Doesn't require significant refactors to scale or improve
- Doesn't feel dirty
  - (just didn't feel right calling something like this 'elegant')
- ... More?

#### Conventions are a win for everyone

## Next Up: [A Single Component](https://github.com/kylpo/react-playbook/blob/master/component-architecture/2_A-Component.md)
