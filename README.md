> **What is this repo?**
> `Highlight-undo.nvim` before its rewrite.
> The classic way is less likely to cause conflicts with other plugins, and it's config is considerably less boilerplate.
> So let's maintain it here.

# highlight-undo.nvim

Highlight changed text after Undo / Redo operations. Purely lua / nvim api implementation,
no external dependencies needed.

## In Action

![recording](https://github.com/tzachar/highlight-undo.nvim/assets/4946827/81b85a3b-b563-4e97-b4e1-7a48d0d2f912)

## Installation

Using Lazy:

```lua
  {
    'tzachar/highlight-undo.nvim',
    opts = {
      ...
    },
  },
```

## Setup

You can manually setup `highlight-undo` as follows:

```lua
require('highlight-undo').setup({
  duration = 300,
  undo = {
    hlgroup = 'HighlightUndo',
    mode = 'n',
    lhs = 'u',
    map = 'undo',
    opts = {}
  },
  redo = {
    hlgroup = 'HighlightRedo',
    mode = 'n',
    lhs = '<C-r>',
    map = 'redo',
    opts = {}
  },
  highlight_for_count = true,
})
```

## Keymaps

Specify which keymaps should trigger the beginning and end of tracking changes
([see here](#how-the-plugin-works)). By default, the plugin starts tracking
changes before an `undo` or a `redo`.

Keymaps are specified in the same format as `vim.keymap.set` accepts: mode, lhs,
rhs, opts. Maps are passed verbatim to `vim.keymap.set`.

## `hlgroup`

Specify the highlighting group to use.

By default, `highlight-undo` will use `HighlightUndo` for the undo action and
`HighlightRedo` for the redo action. Both of these groups, are defined when the
plugin is loaded. If the groups are already defined elsewhere in your config
then they will not be overwritten. You can also use others groups, if desired.

## `duration`

The duration (in milliseconds) to highlight changes. Default is 300.

## highlight_for_count

Enable support for highlighting when a `<count>` is provided before the key.
If set to `false` it will only highlight when the mapping is not prefixed with a
`<count>`.

Default: `true`

## How the Plugin Works

`highlight-undo` will remap the `u` and `<C-r>` keys (for undo and redo, by default) and
hijack the calls. By utilizing `nvim_buf_attach` to get indications for changes in the
buffer, the plugin can achieve very low overhead.
