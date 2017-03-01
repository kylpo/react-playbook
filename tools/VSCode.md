```
._____           _     
|_   _|__   ___ | |___
  | |/ _ \ / _ \| / __|
  | | (_) | (_) | \__ \
  |_|\___/ \___/|_|___/
```

# VS Code
## Tips
- see [Microsoft/vscode-tips-and-tricks: Collection of helpful tips and tricks for VS Code.](https://github.com/Microsoft/vscode-tips-and-tricks)
- [awesome-vscode: A curated list of delightful VS Code packages and resources.](https://github.com/viatsko/awesome-vscode)
- [Tasks in Visual Studio Code](https://code.visualstudio.com/docs/editor/tasks)


## Hotkeys/commands
- `cmd-shift-p` (Command Palette)
  - `"sort lines"`
- `cmd-shift-o` nav to symbol


## Settings Plugins
#### [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)
- This is the first thing you should install and set up - great for backing up and syncing the same config across machines.
- Follow steps in Sync readme to set up a Github Key.
- After install, be sure to `Sync: Advanced Options > Toggle Auto-Upload` and `Toggle Auto-Download` to force staying in sync.

#### [TSLint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) for linting `.ts` [rules](https://palantir.github.io/tslint/rules/)
- [buzinas/tslint-eslint-rules: Improve your TSLint with the missing ESLint rules](https://github.com/buzinas/tslint-eslint-rules)
- [palantir/tslint-react: Lint rules related to React & JSX for TSLint.](https://github.com/palantir/tslint-react)
- [Microsoft/tslint-microsoft-contrib: A set of TSLint rules used on some Microsoft projects.](https://github.com/Microsoft/tslint-microsoft-contrib)
- [vrsource/vrsource-tslint-rules: A extra set of tslint rules](https://github.com/vrsource/vrsource-tslint-rules)

Recommendation: start with
```js
"extends": ["tslint:latest", "tslint-react"]
```
and trim back rules as you see fit. For reference, `tslint:latest` is a combination of [latest](https://github.com/palantir/tslint/blob/master/src/configs/latest.ts) and [recommended](https://github.com/palantir/tslint/blob/master/src/configs/recommended.ts) rules.

#### [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)
![](https://shardulm94.gallerycdn.vsassets.io/extensions/shardulm94/trailing-spaces/0.2.11/1474455467376/Microsoft.VisualStudio.Services.Icons.Default)

#### [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
Everything you could need for git analysis (history, blame, diff). Not for commit and branch management - that is baked in by default.
![](https://raw.githubusercontent.com/eamodio/vscode-git-codelens/master/images/preview-gitlens.gif)

#### [File Navigator](https://marketplace.visualstudio.com/items?itemName=jakelucas.code-file-nav)
Closest thing to vinegar.vim

#### [Duplicate file](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-duplicate)
Add missing `Duplicate File` entry to File Explorer right-click menu.

OR just copy/paste, then rename the file without this extension

#### [Markdown Navigate](https://marketplace.visualstudio.com/items?itemName=jrieken.md-navigate)
![](https://raw.githubusercontent.com/jrieken/md-navigate/master/demo.gif)


## Editing Plugins
#### [Auto Close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)
Automatically closes tags while writing the first one, and can close tags after the fact with `cmd-alt-.`

#### [Auto Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)
Currently has a [bug](https://github.com/formulahendry/vscode-auto-rename-tag/issues/19) with multi-line tags though

![](https://raw.githubusercontent.com/formulahendry/vscode-auto-rename-tag/master/images/usage.gif)

#### [htmltagwrap](https://marketplace.visualstudio.com/items?itemName=bradgashler.htmltagwrap)
Select a chunk of code and press `alt-w` to wrap code in a tag


## Vim
#### [Vim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) and its [roadmap](https://github.com/VSCodeVim/Vim/blob/master/ROADMAP.md) of what is available today

You'll want to enable holding down `j` and `k` with
```bash
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false
```

consider using these settings:
```json5
"editor.lineNumbers": "relative"
"vim.leader": " ", // map leader to <space>
"vim.hlsearch": true,
"vim.useSystemClipboard": true,
"vim.easymotion": true, // press `<space><space> s` to jump to a character

// fixes c-d, c-u for visual select. see https://github.com/VSCodeVim/Vim/issues/907#issuecomment-264738452
"vim.otherModesKeyBindingsNonRecursive": [
    {
        "before": ["<C-u>"],
        "after": ["2", "5", "k"]
    },
    {
        "before": ["<C-d>"],
        "after": ["2", "5", "j"]
    },
    {
        "before": ["<C-f>"],
        "after": ["5", "0", "j"]
    },
    {
        "before": ["<C-b>"],
        "after": ["5", "0", "k"]
    }
],
```


## Emacs
#### [Emacs](https://marketplace.visualstudio.com/items?itemName=hiro-sun.vscode-emacs)
#### [keyboard-scroll](https://marketplace.visualstudio.com/items?itemName=selbh.keyboard-scroll) to enable `ctrl-l` functionality
#### [jumpy](https://marketplace.visualstudio.com/items?itemName=wmaurer.vscode-jumpy) (doesn't work with Emacs-plugin [yet](https://github.com/Microsoft/vscode/issues/13441)) for an Ace-Jump alternative
