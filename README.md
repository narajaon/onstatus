*Disclaimer*: this is still a very early stage project, feel free to submit a PR if you want to contribute

# Motivation
OneStatus is an interface helps you interact with your tmux.<br>
One of my goal with it was to get rid of vim's redundant statusline and instead use tmux's.

Much better !

![onestatus](https://user-images.githubusercontent.com/26607946/90639803-7f947f00-e22f-11ea-863e-e347f9379dfe.png)

# Requirements
If you just want to quickly use the plugin :
 - a font that supports powerline
 - https://github.com/itchyny/vim-gitbranch installed
 - copy the `onestatus.json` example given below (this file must be present in your vim config directory)

If you want to play with the API :
 - make sure to disable the default config by setting
 `let g:onestatus_default_layout = 0`
 - create your own personal `onestatus.json`
  Have fun !

# Usage
Since v0.2.0 you can very easily customize your statusline via a `onestatus.json` file that you put in your **config folder ($HOME for vim and $HOME/.config/nvim for nvim)**.<br>
If you want another path you can override `g:onestatus_config_path`<br>
Here's an example of configuration file that you can copy

```json
{
  "status-right": [
    { "fg": "#ffd167", "bg": "default", "label": "" },
    { "fg": "#218380", "bg": "#ffd167", "labelFunc": "s:getCWD" },
    { "fg": "#218380", "bg": "#ffd167", "label": "" },
    { "fg": "#fcfcfc", "bg": "#218380", "labelFunc": "s:getHead" }
  ],
  "status-left": [
    { "fg": "default", "bg": "default", "labelFunc": "s:getFileName" }
  ],
  "status-style": "s:getDefaultColor",
  "window-status-style": [
    { "fg": "#6c757d", "bg": "default", "isStyleOnly": true }
  ],
  "window-status-current-style": [
    { "fg": "#ffd167", "bg": "default", "isStyleOnly": true }
  ]
}
```

which will give your current git head and the name of the focused file just like shown in the screenshot

Then you can use the full power of onestatus like in this example
```
au BufEnter * :OneStatus
set noshowmode noruler
set laststatus = 0
```
you can of course not use `BufEnter` and use `WinEnter` or some other events but I found it sufficient for my everyday use.

## The api
You must have noticed that the json file has these types of attributes
- tmux option: an option that will be sent to tmux, you can learn more about them in `man tmux` 
- tmux color: can be any color forma supported by tmux (ex: #ffd167)
- onestatus' builtin function: they begin with `s:` like `s:getFileName`

and has this form
```
{
  option: attributes
}
```

In order to give a maximum amount of flexibility, attributes can either be an array of attribute of this shape:
```
{
  "fg": optional tmux color (will default to tmux's default),
  "bg": optional tmux color (will default to tmux's default),
  "label": the text to be displayed if your option takes a string as an argument (ex: status-left ),
  "labelFunc": you can dynamicaly display labels by using one of the functions exposed by onestatus (will take precedance over "label"),
  "isStyleOnly": a boolean that has to be set to true if your option does not display a label
}
```

You can also use also use a onestatus function to dynamicaly generate your attributes.
Currently only `s:getDefaultColor` is supported.

## Exposed functions
labelFunc:
- `s:getCWD`
- `s:getFileName`
- `s:getHead`

attributes:
- `s:getDefaultColor` (it will make your statusline's background match with your vim's theme)

## Exposed global variables
- `g:onestatus_default_layout`: can be `0` or `1`. Defaults to `1`
 Setting it to 0 prevents onestatus from applying some arbitrary layout style. Usefull when you want to fully customize your statusline.
- `g:onestatus_config_path`: a path string
 It contains the default path where onestatus will look for a `onestatus.json`. You can override it if you want to use a custom path.

## For even more customization
OneStatus also provides a helper to send more straightforward commands to tmux

```vim
  call onestatus#build([
        \{'command' : 'set-option status-justify centre'},
        \{'command': 'set-option status-right-length 30'},
        \{'command': 'set-option status-left-length 50'},
        \])
```

# TODO
- Write a proper `:h`
- Add more default templates
- Improve default template theme integration
- Add more builtin functions

# License
Distributed under the same terms as Vim itself. See the vim license.
