```
._____           _     
|_   _|__   ___ | |___
  | |/ _ \ / _ \| / __|
  | | (_) | (_) | \__ \
  |_|\___/ \___/|_|___/
```

# Atom
## Tips
- Want to open multiple projects in one Atom session?
  - `atom -a ~/otherProject`
  - Especially useful when editing two React Native roots
  - ![](http://blog.atom.io/img/posts/atom-add-command-line.gif)
- Follow Atom's [blog](http://blog.atom.io/)
- "preserve last search" option in Command Palette package
- cmd-shift-b to see git modified buffers
- Fuzzy Finder - ensure Search All Panes is not toggled
- toggle Use Custom Titlebar in Core Settings
- cmd-k cmd-w to close pane

## Other Links
[The Ultimate Atom Editor Setup (+for JS/React) – Productivity Freak – Medium](https://medium.com/productivity-freak/my-atom-editor-setup-for-js-react-9726cd69ad20)

## Plugins - Generic
### [advanced-open-file](https://atom.io/packages/advanced-open-file)
Pretty crucial for me, coming from vim. This allows me to easily switch between open tabs without ctr-tabbing or clicking. Also has an omni search.

### [open-recent](https://atom.io/packages/open-recent)
Surprisingly, this is missing by default. Very helpful for if your flow is to open Atom, then open the project you'd like, instead of opening Atom from `cli` in the project you want.

### [clipboard-plus](https://atom.io/packages/clipboard-plus)
![](http://i.gyazo.com/48cfc66c8f8b7666efb7334d928f1a9e.gif)

### [quick-highlight](https://atom.io/packages/quick-highlight)
(Works nicely with vim-mode-plus, too)
![](https://i.github-camo.com/43ddccfcf8c24c01abd6d94439e3be7ca643d7e3/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f74396d642f74396d642f663531623865323131653965643865643435353035336265353264353530356461383736623239382f696d672f61746f6d2d717569636b2d686967686c696768742e676966)

### [todo-show](https://atom.io/packages/todo-show)
Finds all the TODOs, FIXMEs, HACKs, etc. in your project.

![](https://raw.githubusercontent.com/mrodalgaard/atom-todo-show/master/screenshots/preview.png)

### [pigments](https://atom.io/packages/pigments)
A package to display colors in project and files.

![](https://raw.githubusercontent.com/abe33/atom-pigments/master/resources/pigments.gif)

### [file-icons](https://atom.io/packages/file-icons)
Assigns icons to file types (seen in tree view and tab title)

### [linter-eslint](https://atom.io/packages/linter-eslint)
If you use eslint. Also works with Nuclide's Diagnostics window.

### [atom-ternjs](https://atom.io/packages/atom-ternjs)
Intellisense for js

### [terminal-plus](https://atom.io/packages/terminal-plus)
Terminals in your Atom editor

![](https://raw.githubusercontent.com/jeremyramin/terminal-plus/master/resources/demo.gif)

### [smalls](https://atom.io/packages/smalls)
Rapid cursor positioning across any visible chars with search and jump. SO GOOD!

__Settings:__ Jump Trigger Input Length: 2

---

## Plugins - git
### [git-control](https://atom.io/packages/git-control)
- A comprehensive GUI to interacting with git.
- git-flow button makes it easy

![](https://raw.githubusercontent.com/jacogr/atom-git-control/master/screenshots/git-01.png)

### [git-blame](https://atom.io/packages/git-blame)
![](https://raw.githubusercontent.com/alexcorre/git-blame/master/images/screen-shot.png)

### [merge-conflicts](https://atom.io/packages/merge-conflicts)
![](https://raw.github.com/smashwilson/merge-conflicts/master/docs/conflict-resolution.gif)

---

## Plugins - React-specific
### [nuclide](https://atom.io/packages/nuclide)
Built-in tooling for React Native and Flow

__Settings__: Be sure to read through the `Use` section and disable what you don't need. Like `Hack` and `hg` features, for example.

### [file-explorer](https://atom.io/packages/file-explorer)
Your primary tool to navigate up and down a component tree. Better tool than the left Tree View that comes with Atom.

__Settings__: Disable the Exclude Active File to ensure `index.js` can be seen.

### [smart-tab-name](https://atom.io/packages/smart-tab-name)
Gives more context to the file you are looking at. Especially useful when viewing index.js.

![](https://raw.githubusercontent.com/MoOx/atom-smart-tab-name/master/screenshots/one-folder.png)

### [atom-react-native-autocomplete](https://atom.io/packages/atom-react-native-autocomplete)
Add Intellisense completion for RN components (not for your custom ones though)

### [less-than-slash](https://atom.io/packages/less-than-slash)
Easily create the ending tag of a component in your jsx. Particularly helpful when you convert a self-closing tag into a wrapping one.

### [atom-wrap-in-tag](https://atom.io/packages/atom-wrap-in-tag)
![](https://i.github-camo.com/07b67f4500cbf448ab0455a4586260fd042659af/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f73616e75736172742f61746f6d2d777261702d696e2d7461672f6d61737465722f696d616765732f73637265656e73686f742e676966)

---

## Plugins - Vim
### [vim-mode-plus](https://atom.io/packages/vim-mode-plus)
- Cool feature demos: https://github.com/t9md/atom-vim-mode-plus/wiki/GIFs
- Example [keymap](https://github.com/t9md/atom-vim-mode-plus/wiki/Keymap-example) (from creator)
- All [commands](https://github.com/t9md/atom-vim-mode-plus/wiki/Commands)
- [TIPS](https://github.com/t9md/atom-vim-mode-plus/wiki/TIPS#use-system-clipboard-only-when-you-use-space-as-leaderkey)
- [YouDontKnowVimModePlus](https://github.com/t9md/atom-vim-mode-plus/wiki/YouDontKnowVimModePlus)
- [AdvancedTopicTutorial](https://github.com/t9md/atom-vim-mode-plus/wiki/AdvancedTopicTutorial)

### [vim-mode-plus-move-to-symbols](https://atom.io/packages/vim-mode-plus-move-to-symbols)

### [vim-mode-plus-project-find-from-search](https://atom.io/packages/vim-mode-plus-project-find-from-search)
![](https://raw.githubusercontent.com/t9md/t9md/840920e51a91276b60b22be36fc59f87397eec04/img/vim-mode-plus/vmp-project-find-from-search.gif)

### [vim-mode-plus-subword-movement](https://atom.io/packages/vim-mode-plus-subword-movement)
![](https://gist.github.com/crshd/6f3591dcb73ea87febd0/raw/1cf0e2d00ceb61e2acf45b46c9ce5bff10673349/LBCuzMY9uT.gif)

### [cursor-history](https://atom.io/packages/cursor-history)

### [vim-mode-clipboard-plus](https://atom.io/packages/vim-mode-clipboard-plus)
(Requires clipboard-plus)

### [relative-numbers](https://atom.io/packages/relative-numbers)
![](https://raw.githubusercontent.com/justmoon/relative-numbers/master/screencast.gif)
