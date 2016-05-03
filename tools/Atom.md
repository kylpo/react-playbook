```
*
  _____           _     
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

## Plugins - Generic
### [advanced-open-file](https://atom.io/packages/advanced-open-file)
Pretty crucial for me, coming from vim. This allows me to easily switch between open tabs without ctr-tabbing or clicking. Also has an omni search.

### [open-recent](https://atom.io/packages/open-recent)
Surprisingly, this is missing by default. Very helpful for if your flow is to open Atom, then open the project you'd like, instead of opening Atom from `cli` in the project you want.

### [clipboard-plus](https://atom.io/packages/clipboard-plus)
![](https://i.github-camo.com/3e515033ac645bfe2e94d5d073176934e5254f66/687474703a2f2f692e6779617a6f2e636f6d2f34386366633636633866386237363636656662373333346439323866316139652e676966)

### [quick-highlight](https://atom.io/packages/quick-highlight)
(Works nicely with vim-mode-plus, too)
![](https://i.github-camo.com/43ddccfcf8c24c01abd6d94439e3be7ca643d7e3/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f74396d642f74396d642f663531623865323131653965643865643435353035336265353264353530356461383736623239382f696d672f61746f6d2d717569636b2d686967686c696768742e676966)

### [todo-show](https://atom.io/packages/todo-show)
Finds all the TODOs, FIXMEs, HACKs, etc. in your project.

![](https://i.github-camo.com/f15ae70a5e4f1078889fba95bcfdfdec75306a28/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f6d726f64616c67616172642f61746f6d2d746f646f2d73686f772f6d61737465722f73637265656e73686f74732f707265766965772e706e67)

### [pigments](https://atom.io/packages/pigments)
A package to display colors in project and files.

![](https://i.github-camo.com/16c0c12a183c6f57fb33481984593a515777d5e8/68747470733a2f2f6769746875622e636f6d2f61626533332f61746f6d2d7069676d656e74732f626c6f622f6d61737465722f7265736f75726365732f7069676d656e74732e6769663f7261773d74727565)

### [file-icons](https://atom.io/packages/file-icons)
Assigns icons to file types (seen in tree view and tab title)


### [linter-eslint](https://atom.io/packages/linter-eslint)
If you use eslint. Also works with Nuclide's Diagnostics window.

---

## Plugins - git
### [git-control](https://atom.io/packages/git-control)
- A comprehensive GUI to interacting with git.
- git-flow button makes it easy

![](https://i.github-camo.com/e35620a073ae64498e9dbff837aa273d3dcb263d/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f6a61636f67722f61746f6d2d6769742d636f6e74726f6c2f6d61737465722f73637265656e73686f74732f6769742d30312e706e67)

### [git-blame](https://atom.io/packages/git-blame)
![](https://i.github-camo.com/48e49f0c648a225aa35bebaaff21d437b5b0461b/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f616c6578636f7272652f6769742d626c616d652f6d61737465722f696d616765732f73637265656e2d73686f742e706e67)

### [merge-conflicts](https://atom.io/packages/merge-conflicts)
![](https://i.github-camo.com/44ff44f156f274b8799022e44bcacb804fadc08a/68747470733a2f2f7261772e6769746875622e636f6d2f736d61736877696c736f6e2f6d657267652d636f6e666c696374732f6d61737465722f646f63732f636f6e666c6963742d7265736f6c7574696f6e2e676966)

---

## Plugins - React-specific
### [nuclide](https://atom.io/packages/nuclide)
Built-in tooling for React Native and Flow

Settings: Be sure to read through the `Use` section and disable what you don't need. Like `Hack` and `hg` features, for example.

### [file-explorer](https://atom.io/packages/file-explorer)
Your primary tool to navigate up and down a component tree. Better tool than the left Tree View that comes with Atom.

Settings: Disable the Exclude Active File to ensure `index.js` can be seen.

### [smart-tab-name](https://atom.io/packages/smart-tab-name)
Gives more context to the file you are looking at. Especially useful when viewing index.js.

![](https://i.github-camo.com/164d4406df3f766fdce0608217f98a1eaf153cd4/68747470733a2f2f6769746875622e636f6d2f4d6f4f782f61746f6d2d736d6172742d7461622d6e616d652f7261772f6d61737465722f73637265656e73686f74732f6f6e652d666f6c6465722e706e67)

### [atom-react-native-autocomplete](https://atom.io/packages/atom-react-native-autocomplete)
Add Intellisense completion for RN components (not for your custom ones though)

### [less-than-slash](https://atom.io/packages/less-than-slash)
Easily create the ending tag of a component in your jsx. Particularly helpful when you convert a self-closing tag into a wrapping one.

---

## Plugins - Vim
### [vim-mode-plus](https://atom.io/packages/vim-mode-plus)
- Cool feature demos: https://github.com/t9md/atom-vim-mode-plus/wiki/GIFs
- Example [keymap](https://github.com/t9md/atom-vim-mode-plus/wiki/Keymap-example) (from creator)
- All [commands](https://github.com/t9md/atom-vim-mode-plus/wiki/Commands)

### [vim-mode-plus-move-to-symbols](https://atom.io/packages/vim-mode-plus-move-to-symbols)

### [vim-mode-plus-project-find-from-search](https://atom.io/packages/vim-mode-plus-project-find-from-search)
![](https://i.github-camo.com/2c20ce2edd235c941fcfd02bb3505c9206e3972e/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f74396d642f74396d642f383430393230653531613931323736623630623232626533366663353966383733393765656330342f696d672f76696d2d6d6f64652d706c75732f766d702d70726f6a6563742d66696e642d66726f6d2d7365617263682e676966)

### [vim-mode-plus-subword-movement](https://atom.io/packages/vim-mode-plus-subword-movement)
![](https://i.github-camo.com/924801915b3f57d4a44b537a6a5b94756819c367/68747470733a2f2f676973742e6769746875622e636f6d2f63727368642f36663335393164636237336561383766656264302f7261772f316366306532643030636562363165326163663435623436633963653562666631303637333334392f4c4243757a4d593975542e676966)

### [cursor-history](https://atom.io/packages/cursor-history)

### [vim-mode-clipboard-plus](https://atom.io/packages/vim-mode-clipboard-plus)
(Requires clipboard-plus)
