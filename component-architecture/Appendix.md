```
*
   ____                                             _        _             _     _ _            _                  
  / ___|___  _ __ ___  _ __   ___  _ __   ___ _ __ | |_     / \   _ __ ___| |__ (_) |_ ___  ___| |_ _   _ _ __ ___
 | |   / _ \| '_ ` _ \| '_ \ / _ \| '_ \ / _ \ '_ \| __|   / _ \ | '__/ __| '_ \| | __/ _ \/ __| __| | | | '__/ _ \
 | |__| (_) | | | | | | |_) | (_) | | | |  __/ | | | |_   / ___ \| | | (__| | | | | ||  __/ (__| |_| |_| | | |  __/
  \____\___/|_| |_| |_| .__/ \___/|_| |_|\___|_| |_|\__| /_/   \_\_|  \___|_| |_|_|\__\___|\___|\__|\__,_|_|  \___|
                      |_|                                                                                          
```
# Part 6: Future Tooling
With conventions outline in this component architecture, we will be able to easily build tooling and automation around it!

#### Some ideas not yet implemented
- extract current file's jsx out into subcomponent
  - highlight a section of your jsx, press some hotkey, you are prompted for a name, then the component is generated and stored in the current component's `_` folder
- hotkeys to jump to/from a component's container, presentational, and tests
- convert component file to component folder
  - sets up index.js as well as the folder
- convert subcomponent to shared component
- Component Guide service, like a designer's Style Guide, could scrape through `/lib`, and generate a visual display of your component kit

These tools would only work because of the conventions established - just like Rails!
