```
. ____                                             _        _             _     _ _            _                  
 / ___|___  _ __ ___  _ __   ___  _ __   ___ _ __ | |_     / \   _ __ ___| |__ (_) |_ ___  ___| |_ _   _ _ __ ___
| |   / _ \| '_ ` _ \| '_ \ / _ \| '_ \ / _ \ '_ \| __|   / _ \ | '__/ __| '_ \| | __/ _ \/ __| __| | | | '__/ _ \
| |__| (_) | | | | | | |_) | (_) | | | |  __/ | | | |_   / ___ \| | | (__| | | | | ||  __/ (__| |_| |_| | | |  __/
 \____\___/|_| |_| |_| .__/ \___/|_| |_|\___|_| |_|\__| /_/   \_\_|  \___|_| |_|_|\__\___|\___|\__|\__,_|_|  \___|
                      |_|                                                                                          
```
# Part 5: Example App Structure
For this example, we'll show a React Native app using Redux.

```
.
├── /android/                   # android-specific code (bootstrapped from `react-native init`)
├── /ios/                       # ios-specific code (bootstrapped from `react-native init`)
|
├── /assets                     # **Optional** if you'd like to keep them outside of your components
│   ├── /fonts/
│   ├── /images/
│   ├── /videos/
|
├── /app                        # Your app-specific source code of the application
│   ├── /flux/                  # Redux folder full of ducks-pattern files
│   ├── /scenes/                # Components that are handled by a Navigator. Same as pages, views, etc for the app.
│   |   ├── /App/               # Root scene
│   |       ├── /_/             # Subcomponents of App
│   |       ├── /App.js         # Component used in index.ios.js and android.ios.js
│   |       └── /index.js       
│   ├── /shared/                # Shared, app-dependent Components and functions
│   └── /types/                 # Types for the app-specific objects that would usually be a model
|                               # NOTE: if using something like Immutable.js to define Records, call this /models/ instead
|
├── /lib                        # Your non-app-specific Components and functions
|
├── /tools/                     # Build automation scripts and utilities
├── /node_modules/              # 3rd-party libraries and utilities
└── package.json                # The list of 3rd party libraries and utilities
```

## Next Up: [Future Tooling](https://github.com/kylpo/react-playbook/blob/master/component-architecture/6_Future-Tooling.md)
