# Neo-tree.nvim: Example source

This is just an example of how build a custom source for [Neo-tree](https://github.com/nvim-neo-tree/neo-tree.nvim)
as an external plugin.


## Quickstart

  
```lua
use {
  "nvim-neo-tree/neo-tree.nvim",
    branch = "external_sources_take_2",
    requires = { 
      "nvim-lua/plenary.nvim",
      "kyazdani42/nvim-web-devicons",
      "MunifTanjim/nui.nvim",
      "nvim-neo-tree/example-source" -- <-- THIS IS US! Add This line to your existing config.
    },
    config = function ()
      require("neo-tree").setup({
        sources = {
            "filesystem",
            "buffers",
            "git_status",
            "example" -- <-- external sources need to be a fully qualified path to the module
            --"my.name.example" <-- Feel free to add to your folder structure to create a namespace,
                                 -- The name of the source will be the last part, or whatever your module
                                 -- exports as the `name` field.
        },
        example = {
            -- The config for your source goes here. This is the same as any other source, plus whatever
            -- special config options you add.
            --window = {...}
            --renderers = { ..}
            --etc
        },
      })
    end
}
```

After installing, run:
```
:Neotree example
```

...to see the example source in all it's glory!


## Custom Stat Provider

A new feature just added is the concept of being able to provide a custom stat provider function to provide statistics to be used by the Size, Last Modified, and Created components and sort commands. If your nodes represent files and folders in the local filesystem and they have a `path` property, then you don't need to do anything because the default provider can handle those.

If your source provides something else, or even if just some of your nodes are something else, then you can handle that by doing one of the following:

### Lazy Loaded Stats

1. Create a function that takes a NuiNode object, which will contain copies of items you passed into the `showNodes` function, and returns a `stat` table in the same structure as `vim.loop.fs_stat`. [example](https://github.com/nvim-neo-tree/example-source/blob/75c482bb052594a3ba13084a1e4ead5c84f69a01/lua/example/init.lua#L42)
2. Register that function using `require("neo-tree.utils").register_stat_provider(name, func)` [example](https://github.com/nvim-neo-tree/example-source/blob/75c482bb052594a3ba13084a1e4ead5c84f69a01/lua/example/init.lua#L99)
3. On each item you provide to the `showNodes` function, add a string property called `stat_provider` which is the same name as what you used in your `register_stat_provider` call. [example](https://github.com/nvim-neo-tree/example-source/blob/75c482bb052594a3ba13084a1e4ead5c84f69a01/lua/example/init.lua#L60-L91)

The `stat_provider` function will only be called once per node and the result will be cached on the node object itself in a property called `stat`. That means the lifetime of that cache is the same as the lifetime of a node.

### Preloaded Stats

Alternatively, if it's not beneficial to lazy load these stats and you want to provide them premptively, you can just add them to each item that you send to `showNodes` as a property called `stat`. If you do this, there is no need to register a provider.

You could technically mix and match these strategies. Each node should have either a `stat` or a `stat_provider` property.
