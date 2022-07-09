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
