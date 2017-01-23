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

## Hotkeys/commands
### cmd-shift-p (Command Palette)
`sort lines`

## Plugins
### [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)
- This is the first thing you should install and set up - great for backing up and syncing the same config across machines.
- After install, be sure to `Sync: Advanced Options > Toggle Auto-Upload` and `Toggle Auto-Download` to force staying in sync.

### [Vim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)
You'll want to enable holding down `j` and `k` with
```bash
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false
```

consider using these settings:
```js
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

### [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)
![](https://shardulm94.gallerycdn.vsassets.io/extensions/shardulm94/trailing-spaces/0.2.11/1474455467376/Microsoft.VisualStudio.Services.Icons.Default)

### [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
Everything you could need for git analysis (history, blame, diff). Not for commit and branch management - that is baked in by default.
![](https://raw.githubusercontent.com/eamodio/vscode-git-codelens/master/images/preview-gitlens.gif)

### [File Navigator](https://marketplace.visualstudio.com/items?itemName=jakelucas.code-file-nav)
Closest thing to vinegar.vim

### [Duplicate file](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-duplicate)
Add missing `Duplicate File` entry to File Explorer right-click menu.

OR just copy/paste, then rename the file without this extension

### [Markdown Navigate](https://marketplace.visualstudio.com/items?itemName=jrieken.md-navigate)
![](https://raw.githubusercontent.com/jrieken/md-navigate/master/demo.gif)
