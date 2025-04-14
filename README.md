# tidy.nvim 🧹

**tidy.nvim** removes trailing white space and empty lines at EOF on save.

![tidy](https://github.com/mcauley-penney/tidy.nvim/assets/59481467/0c9c43c8-891a-40d4-9d54-b4bd5010be86)


## Requirements

- Neovim >= 0.9.0

It may work on lower versions, but is tested and updated using nightly.

## Installation

Most basic configuration using lazy.nvim:

```lua
{
    "mcauley-penney/tidy.nvim",
    config = true,
}
```

A more full example configuration for lazy.nvim would be:

```lua
{
    "mcauley-penney/tidy.nvim",
    opts = {
        enabled_on_save = false,
        filetype_exclude = { "markdown", "diff" }
    },
    init = function()
        vim.keymap.set('n', "<leader>tt", require("tidy").toggle, {})
        vim.keymap.set('n', "<leader>tr", require("tidy").run, {})
    end
}
```

## Configuration

### Options and Defaults

```lua
{
  enabled_on_save = true,
  filetype_exclude = {}  -- Tidy will not be enabled for any filetype, e.g. "markdown", in this table
  provide_undefined_editorconfig_behavior = false,  -- Set to true to trim whitespace even if .editorconfig omits the setting
}
```

#### EditorConfig Integration

If your project uses `.editorconfig`, `tidy.nvim` will respect the `trim_trailing_whitespace` setting. When this option is explicitly set, the plugin will defer to it and skip trimming.

However, if `.editorconfig` is present but the option is *not defined*, `tidy.nvim` will **not** trim whitespace by default. This is done because we expect that you do not want trailing whitespace trimmed if you have not defined this setting in your existing `.editorconfig` file. **This can make it seem like the plugin has stopped working, but it has not**. To override this behavior, set `provide_undefined_editorconfig_behavior = true` in your config. This setting will make tidy trim whitespace even if `.editorconfig` exists and does not hae the `trim_trailing_whitespace` setting.


### Functions

tidy.nvim also comes with the following functions, which may be mapped:

| Lua                        | Description                                                      |
| -------------------------- | ---------------------------------------------------------------- |
| `require("tidy").toggle()` | Turn tidy.nvim off for the current buffer                        |
| `require("tidy").run()`    | Run the formatting functionality of tidy.nvim off without saving |

```lua
init = function()
     vim.keymap.set('n', "<leader>tt", require("tidy").toggle, {})
     vim.keymap.set('n', "<leader>tr", require("tidy").run, {})
end
```


## About and Credits

I originally wrote this as a wrapper around a couple of vim regex commands used for formatting files before I began using formatters. These commands are not mine, please see the sources below. Even with real formatters in my setup now, I still like and use this because I like these specific formats to be applied to every buffer and don't want to have a formatting tool installed for them.

- [Vim Tips Wiki entry for removing unwanted spaces](https://vim.fandom.com/wiki/Remove_unwanted_spaces#Automatically_removing_all_trailing_whitespace)

- `ib.`, the author of [this Stack Overflow answer](https://stackoverflow.com/a/7501902)

- [This line](https://github.com/gpanders/editorconfig.nvim/blob/ae3586771996b2fb1662eb0c17f5d1f4f5759bb7/lua/editorconfig.lua#L180)
  in [gpanders/editorconfig.nvim](https://github.com/gpanders/editorconfig.nvim) for exposing me to the `keepjumps`
  and `keeppatterns` modifiers
