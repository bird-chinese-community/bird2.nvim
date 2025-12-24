# BIRD2.nvim

<div align="center">

**BIRD2 配置文件的 Neovim 插件**

Version: [English](README.md) | 简体中文

<!-- 徽章 -->

[![MPL-2.0 许可证](https://img.shields.io/badge/License-MPL--2.0-blue?style=flat-square)](LICENSE)
[![Neovim 0.9+](https://img.shields.io/badge/Neovim-0.9+-green?style=flat-square&logo=neovim)](https://neovim.io/)
[![Lua](https://img.shields.io/badge/Lua-5.1+-blue?style=flat-square&logo=lua)](https://www.lua.org/)
[![GitHub Stars](https://img.shields.io/github/stars/bird-chinese-community/BIRD2.nvim?style=flat-square&logo=github)](https://github.com/bird-chinese-community/BIRD2.nvim)
[![GitHub Issues](https://img.shields.io/github/issues/bird-chinese-community/BIRD2.nvim?style=flat-square&logo=github)](https://github.com/bird-chinese-community/BIRD2.nvim/issues)
[![维护状态](https://img.shields.io/badge/维护中-是-success?style=flat-square)](https://github.com/bird-chinese-community/BIRD2.nvim/graphs/commit-activity)

<!-- 预览图片 -->

![BIRD2.nvim 预览](https://raw.githubusercontent.com/bird-chinese-community/BIRD-tm-language-grammar/main/.github/assets/bird2-grammar-vim-preview.jpg)

</div>

---

## 目录

- [BIRD2.nvim](#bird2nvim)
  - [目录](#目录)
  - [概述](#概述)
  - [功能特性](#功能特性)
  - [安装](#安装)
    - [使用 lazy.nvim（推荐）](#使用-lazynvim推荐)
    - [使用 packer.nvim](#使用-packernvim)
    - [使用 vim-plug](#使用-vim-plug)
  - [配置](#配置)
    - [禁用启发式检测](#禁用启发式检测)
    - [自定义文件类型检测](#自定义文件类型检测)
  - [使用](#使用)
  - [API](#api)
  - [文档](#文档)
  - [贡献](#贡献)
  - [许可证](#许可证)
  - [相关项目](#相关项目)

---

## 概述

`BIRD2.nvim` 为 [BIRD2](https://bird.network.cz/) 配置文件提供 Neovim 支持，包括语法高亮、文件类型检测和文件类型插件功能。

这是 [BIRD 中文社区](https://github.com/bird-chinese-community) 的 [BIRD-tm-language-grammar](https://github.com/bird-chinese-community/bird-tm-language-grammar) 项目的 Neovim 插件组件。

---

## 功能特性

- :rainbow: **语法高亮** - 完整的 BIRD2 配置语法高亮
- :mag: **自动文件类型检测** - 支持 `.bird`, `.bird2`, `.bird3`, `.conf` 等扩展名
- :brain: **智能启发式检测** - 使用 Lua 模式匹配对通用 `.conf` 文件进行检测
- :wrench: **文件类型特定设置** - 注释、格式选项、匹配对等
- :heart_pulse: **内置 `:checkhealth` 集成**
- :zap: **现代 Lua API** - 支持扩展
- :rocket: **懒加载支持** - 优化性能

---

## 安装

<details>
<summary><b>:package: 快速安装</b></summary>

选择你喜欢的插件管理器：

### 使用 lazy.nvim（推荐）

```lua
{
  "bird-chinese-community/BIRD2.nvim",
  ft = "bird2",
  config = function()
    require("bird2").setup()
  end
}
```

### 使用 packer.nvim

```lua
use {
  "bird-chinese-community/BIRD2.nvim",
  config = function()
    require("bird2").setup()
  end
}
```

### 使用 vim-plug

```vim
Plug 'bird-chinese-community/BIRD2.nvim'
" 然后在 Lua 配置中：
lua require("bird2").setup()
```

</details>

<details>
<summary><b>:wrench: 手动安装</b></summary>

```bash
# 克隆仓库
git clone https://github.com/bird-chinese-community/BIRD2.nvim \
  ~/.local/share/nvim/site/pack/plugins/start/BIRD2.nvim
```

然后添加到你的配置：

```lua
require("bird2").setup()
```

</details>

---

## 配置

<details>
<summary><b>:gear: 默认配置</b></summary>

```lua
require("bird2").setup({
  enabled = true,
  heuristic_detect = true,
})
```

</details>

<details>
<summary><b>:gear: 高级选项</b></summary>

### 禁用启发式检测

```lua
require("bird2").setup({
  heuristic_detect = false,
})
```

### 自定义文件类型检测

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

## 使用

<details>
<summary><b>:terminal: 命令</b></summary>

| 命令             | 描述                 |
| ---------------- | -------------------- |
| `:Bird2 version` | 显示插件版本         |
| `:Bird2 check`   | 运行健康检查         |
| `:Bird2 enable`  | 为当前缓冲区启用插件 |
| `:Bird2 disable` | 为当前缓冲区禁用插件 |

</details>

<details>
<summary><b>:heart: 健康检查</b></summary>

```vim
:checkhealth bird2
```

或使用自定义命令：

```vim
:Bird2 check
```

</details>

<details>
<summary><b>:zap: 用户自动命令</b></summary>

监听 `Bird2File` 事件：

```lua
vim.api.nvim_create_autocmd("User", {
  pattern = "Bird2File",
  callback = function(args)
    local bufnr = args.data.buf
    -- 为 bird2 文件添加自定义设置
  end,
})
```

</details>

---

## API

<details>
<summary><b>:code: `setup(opts)`</b></summary>

使用选项初始化插件。

```lua
---@class bird2.Config
---@field enabled boolean 是否启用插件
---@field heuristic_detect boolean 为 .conf 文件启用启发式检测
require("bird2").setup({
  enabled = true,
  heuristic_detect = true,
})
```

</details>

<details>
<summary><b>:code: `on_attach(bufnr)`</b></summary>

手动触发缓冲区设置。

```lua
require("bird2").on_attach(0)
```

</details>

<details>
<summary><b>:code: `looks_like_bird2(bufnr)`</b></summary>

检查缓冲区是否看起来像 BIRD2 配置。

```lua
local is_bird2 = require("bird2").looks_like_bird2(0)
```

</details>

---

## 文档

安装后，可查看帮助文档：

```vim
:help bird2
```

---

## 贡献

欢迎贡献！请随时提交 Pull Request。

<details>
<summary><b>:test_tube: 运行测试</b></summary>

```bash
luarocks install --only-deps
luarocks test
```

</details>

---

## 许可证

- 插件文件：[Mozilla Public License 2.0](LICENSE)
- 版权所有 (c) BIRD 中文社区

---

## 相关项目

- :bookmark: [BIRD-tm-language-grammar](https://github.com/bird-chinese-community/bird-tm-language-grammar) - BIRD2 的 TextMate 语法
- :star: [bird2.vim](https://github.com/bird-chinese-community/bird2.vim) - Vim 插件
- :electric_plug: [vscode-bird2](https://github.com/bird-chinese-community/vscode-bird2-conf) - VS Code 扩展

---

<div align="center">

用 :heart: 维护，由 [BIRD 中文社区](https://github.com/bird-chinese-community) 呈现

</div>
