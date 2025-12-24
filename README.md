# BIRD2.nvim

<div align="center">

**Neovim plugin for BIRD2 configuration files**

Version: English | [简体中文](README.zh-CN.md)

<!-- Badges -->

[![MPL-2.0 License](https://img.shields.io/badge/License-MPL--2.0-blue?style=flat-square)](LICENSE)
[![Neovim 0.9+](https://img.shields.io/badge/Neovim-0.9+-green?style=flat-square&logo=neovim)](https://neovim.io/)
[![Lua](https://img.shields.io/badge/Lua-5.1+-blue?style=flat-square&logo=lua)](https://www.lua.org/)
[![GitHub Stars](https://img.shields.io/github/stars/bird-chinese-community/BIRD2.nvim?style=flat-square&logo=github)](https://github.com/bird-chinese-community/BIRD2.nvim)
[![GitHub Issues](https://img.shields.io/github/issues/bird-chinese-community/BIRD2.nvim?style=flat-square&logo=github)](https://github.com/bird-chinese-community/BIRD2.nvim/issues)
[![Maintenance](https://img.shields.io/badge/Maintained-Yes-success?style=flat-square)](https://github.com/bird-chinese-community/BIRD2.nvim/graphs/commit-activity)

<!-- Preview Image -->

![BIRD2.nvim Preview](https://raw.githubusercontent.com/bird-chinese-community/BIRD-tm-language-grammar/main/.github/assets/bird2-grammar-vim-preview.jpg)

</div>

---

## Table of Contents

- [BIRD2.nvim](#bird2nvim)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Features](#features)
  - [Installation](#installation)
    - [Using lazy.nvim (Recommended)](#using-lazynvim-recommended)
    - [Using packer.nvim](#using-packernvim)
    - [Using vim-plug](#using-vim-plug)
  - [Configuration](#configuration)
    - [Disable Heuristic Detection](#disable-heuristic-detection)
    - [Custom Filetype Detection](#custom-filetype-detection)
  - [Usage](#usage)
  - [API](#api)
  - [Documentation](#documentation)
  - [Contributing](#contributing)
  - [License](#license)
  - [Related Projects](#related-projects)

---

## Overview

`BIRD2.nvim` provides Neovim support for [BIRD2](https://bird.network.cz/) configuration files, including syntax highlighting, filetype detection, and filetype plugin features.

This is the Neovim plugin component of the [BIRD-tm-language-grammar](https://github.com/bird-chinese-community/bird-tm-language-grammar) project by the **BIRD Chinese Community**.

---

## Features

- :rainbow: **Syntax highlighting** for all BIRD2 configuration elements
- :mag: **Automatic filetype detection** for `.bird`, `.bird2`, `.bird3`, and `.conf` files
- :brain: **Smart heuristic detection** for generic `.conf` files using Lua patterns
- :wrench: **Filetype-specific settings** (comments, format options, matchpairs)
- :heart_pulse: **Built-in `:checkhealth` integration**
- :zap: **Modern Lua API** for extensibility
- :rocket: **Lazy-loading support** for optimal performance

---

## Installation

<details>
<summary><b>:package: Quick Installation</b></summary>

Choose your preferred plugin manager:

### Using lazy.nvim (Recommended)

```lua
{
  "bird-chinese-community/BIRD2.nvim",
  ft = "bird2",
  config = function()
    require("bird2").setup()
  end
}
```

### Using packer.nvim

```lua
use {
  "bird-chinese-community/BIRD2.nvim",
  config = function()
    require("bird2").setup()
  end
}
```

### Using vim-plug

```vim
Plug 'bird-chinese-community/BIRD2.nvim'
" Then in your Lua config:
lua require("bird2").setup()
```

</details>

<details>
<summary><b>:wrench: Manual Installation</b></summary>

```bash
# Clone the repository
git clone https://github.com/bird-chinese-community/BIRD2.nvim \
  ~/.local/share/nvim/site/pack/plugins/start/BIRD2.nvim
```

Then add to your config:

```lua
require("bird2").setup()
```

</details>

---

## Configuration

<details>
<summary><b>:gear: Default Configuration</b></summary>

```lua
require("bird2").setup({
  enabled = true,
  heuristic_detect = true,
})
```

</details>

<details>
<summary><b>:gear: Advanced Options</b></summary>

### Disable Heuristic Detection

```lua
require("bird2").setup({
  heuristic_detect = false,
})
```

### Custom Filetype Detection

```lua
vim.filetype.add({
  extension = {
    myext = "bird2",
  },
  pattern = {
    [".*%.myconf"] = "bird2",
  },
})
```

</details>

---

## Usage

<details>
<summary><b>:terminal: Commands</b></summary>

| Command          | Description                       |
| ---------------- | --------------------------------- |
| `:Bird2 version` | Show plugin version               |
| `:Bird2 check`   | Run health checks                 |
| `:Bird2 enable`  | Enable plugin for current buffer  |
| `:Bird2 disable` | Disable plugin for current buffer |

</details>

<details>
<summary><b>:heart: Health Check</b></summary>

```vim
:checkhealth bird2
```

Or use the custom command:

```vim
:Bird2 check
```

</details>

<details>
<summary><b>:zap: User Autocommand</b></summary>

Listen to the `Bird2File` event:

```lua
vim.api.nvim_create_autocmd("User", {
  pattern = "Bird2File",
  callback = function(args)
    local bufnr = args.data.buf
    -- Your custom setup for bird2 files
  end,
})
```

</details>

---

## API

<details>
<summary><b>:code: `setup(opts)`</b></summary>

Initialize the plugin with options.

```lua
---@class bird2.Config
---@field enabled boolean Whether to enable the plugin
---@field heuristic_detect boolean Enable heuristic detection for .conf files
require("bird2").setup({
  enabled = true,
  heuristic_detect = true,
})
```

</details>

<details>
<summary><b>:code: `on_attach(bufnr)`</b></summary>

Manually trigger buffer setup.

```lua
require("bird2").on_attach(0)
```

</details>

<details>
<summary><b>:code: `looks_like_bird2(bufnr)`</b></summary>

Check if a buffer looks like BIRD2 configuration.

```lua
local is_bird2 = require("bird2").looks_like_bird2(0)
```

</details>

---

## Documentation

After installation, view the help documentation:

```vim
:help bird2
```

---

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

<details>
<summary><b>:test_tube: Running Tests</b></summary>

```bash
luarocks install --only-deps
luarocks test
```

</details>

---

## License

- Plugin files: [Mozilla Public License 2.0](LICENSE)
- Copyright (c) BIRD Chinese Community

---

## Related Projects

- :bookmark: [BIRD-tm-language-grammar](https://github.com/bird-chinese-community/bird-tm-language-grammar) - TextMate grammar for BIRD2
- :star: [bird2.vim](https://github.com/bird-chinese-community/bird2.vim) - Vim plugin
- :electric_plug: [vscode-bird2](https://github.com/bird-chinese-community/vscode-bird2-conf) - VS Code extension

---

<div align="center">

Maintained with :heart: by the [BIRD Chinese Community](https://github.com/bird-chinese-community)

</div>
