+++
date = '2026-03-09T19:02:25+03:00'
draft = false
title = 'Best Neovim Setup'
+++

# Best Neovim Setup

Hello! Today, we'll create the best Neovim setup in this world.

Neovim is a text editor but with steroids. You can customize EVERYTHING with Lua configurations unlike Vimscript in pure Vim.

Difference between Vim and Neovim is, Vim uses Vimscript which is confusing and slow but Neovim uses Lua which is faster and easier.

If you learned what is Neovim, let's get started.

## Options
```lua
-- Line numbers {{{
vim.opt.number = true
vim.opt.relativenumber = true
-- }}}

-- Tabs & indentation {{{
vim.opt.tabstop = 2
vim.opt.shiftwidth = 2
vim.opt.expandtab = true
vim.opt.smartindent = true
-- }}}

-- UI improvements {{{
vim.opt.termguicolors = true
vim.opt.cursorline = true
vim.opt.signcolumn = "yes"
-- }}}

-- Search {{{
vim.opt.ignorecase = true
vim.opt.smartcase = true
vim.opt.hlsearch = false
vim.opt.incsearch = true
-- }}}

-- Scrolling {{{
vim.opt.scrolloff = 8
vim.opt.sidescrolloff = 8
-- }}}

-- Performance {{{
vim.opt.updatetime = 250
vim.opt.timeoutlen = 400
-- }}}

-- Clipboard {{{
vim.opt.clipboard = "unnamedplus"
-- }}}

-- Folds {{{
vim.opt.foldmethod = "marker"
vim.opt.foldmarker = "{{{,}}}"
-- }}}
```
## Plugins
### conform.nvim
You can format your code automatically when you save it. With this plugin, you always have a good code.

```lua
return {
	{
		"stevearc/conform.nvim",
		opts = {},
		config = function()
			require("conform").setup({
				formatters_by_ft = {
					lua = { "stylua" },
					python = { "black" },
				},
				format_on_save = {
					lsp_format = "fallback",
					timeout_ms = 500,
				},
			})
		end,
	},
}
```
### bufferline.nvim
You can create tab bar on top.
```lua
return {
	{
		"akinsho/bufferline.nvim",
		version = "*",
		dependencies = "nvim-tree/nvim-web-devicons",
		opts = {},
	},
}
```
### blink.cmp
You can use autocompletion on your code with blink.cmp and friendly-snippets!
```lua
return {
	{
		"saghen/blink.cmp",
		dependencies = { "rafamadriz/friendly-snippets" },
		version = "1.*",
		---@module 'blink.cmp'
		---@type blink.cmp.Config
		opts = {
			keymap = { preset = "super-tab" },
			appearance = {
				nerd_font_variant = "mono", --mono or normal
			},
			completion = { documentation = { auto_show = false } },
			sources = {
				default = { "lsp", "path", "snippets", "buffer" },
			},
			fuzzy = { implementation = "prefer_rust_with_warning" },
		},
		opts_extend = { "sources.default" },
	},
}
```
### nvim-lspconfig and mason.nvim
You can install language servers with mason.nvim and use them with lspconfig.
```lua
return {
	{
		"neovim/nvim-lspconfig",
	},
	{
		"mason-org/mason.nvim",
		opts = {},
	},
	{
		"mason-org/mason-lspconfig.nvim",
		opts = {},
		config = function()
			require("mason-lspconfig").setup({
				ensure_installed = { "lua_ls" }, -- you can install others by :Mason command
			})
		end,
	},
}
```
### lualine.nvim
You can create custom statuslines with lualine.nvim.
```lua
return {
	{
		"nvim-lualine/lualine.nvim",
		dependencies = { "nvim-tree/nvim-web-devicons" },
		opts = {},
		config = function()
			require("lualine").setup({
				sections = {
					lualine_a = { "mode" },
					lualine_b = { "branch", "diff", "diagnostics" },
					lualine_c = { "filename" },
					lualine_x = { "encoding", "filetype" },
					lualine_y = { "" },
					lualine_z = { "location" },
				},
			})
		end,
	},
}
```
### noice.nvim
New-gen command prompt for Neovim!
```lua
return {
	{
		"folke/noice.nvim",
		event = "VeryLazy",
		opts = {},
		dependencies = {
			"MunifTanjim/nui.nvim",
			"rcarriga/nvim-notify",
		},
	},
}
```
### telescope.nvim
Fuzzy-finder! We'll create keymaps for it later.
```lua
return {
	{
		"nvim-telescope/telescope.nvim",
		version = "*",
		dependencies = {
			"nvim-lua/plenary.nvim",
			{ "nvim-telescope/telescope-fzf-native.nvim", build = "make" },
		},
	},
}
```
### toggleterm.nvim
You can toggle terminal with a keymap. Great for running code.
```lua
return {
	{
		{
			"akinsho/toggleterm.nvim",
			version = "*",
			config = true,
			opts = {},
		},
	},
}
```
### which-key.nvim
You can see hints when doing keymaps with this plugin.
```lua
return {
	{
		"folke/which-key.nvim",
		event = "VeryLazy",
		opts = {},
		keys = {
			{
				"<leader>?",
				function()
					require("which-key").show({ global = false })
				end,
				desc = "Buffer Local Keymaps (which-key)",
			},
		},
	},
}
```
## Keymaps
### telescope.nvim
I recommend these keymaps for telescope.nvim.
```lua
vim.keymap.set("n", "<leader><leader>", ":Telescope find_files <CR>", { desc = "Find files", silent = true })
vim.keymap.set("n", "<leader>cs", ":Telescope colorscheme <CR>", { desc = "Colorschemes", silent = true })
vim.keymap.set("n", "<leader>bb", ":Telescope buffers <CR>", { desc = "List buffers", silent = true })
```
### toggleterm.nvim
I recommend to use these with toggleterm.nvim to toggle terminal.
```lua
vim.keymap.set("n", "<C-/>", ":ToggleTerm direction=float<CR>")
vim.keymap.set("t", "<C-/>", "<cmd>ToggleTerm direction=float<CR>")
```

## Folds
### Keymaps
To use folds, you can use these keymaps. These are essentials, you can find all of them with a Google search.

| Key | Action           |
| --- | ---------------- |
| za  | Toggle fold      |
| zM  | Close all folds  |
| zR  | Open all folds   |
| zA  | Toggle all folds |
| zf  | Create fold      |
| zd  | Delete fold      |
| zE  | Delete all folds |
### Creating and Deleting Folds Manually
You can add `{{{` and `}}}` as comment lines like:
```lua
-- Fold Name {{{
print("Fold is this! You can fold this with za")
-- }}}
```
## Folder Structure
This is my recommended configuration.

| Path         | Description         |
| ------------ | ------------------- |
| /lua/config  | Configuration files |
| /lua/plugins | Plugin files        |
| /snippets    | Custom snippets     |
## My Own Dotfiles
https://github.com/UmutKzl/dots.git
