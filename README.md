## Why the fork?

The fork is introduce for personal enchancements to the terminal. When I am in my JetBrains IDE, I can use Shift+j and Shift+k for switching between the open terminals.

I didn't find this feature in the toggleterm.lua, so I created a fork to fit my personal use case.

## Documentation

### Main

The main documentation can be read in the original repo [here](https://github.com/akinsho/toggleterm.nvim)

### Personal enchancements

The stuff that I enchanced are listed below:

- Support for switching between open terminals
- Open terminal to the last opened.

### How to use

Here is my configuration for toggleterm on one of my lua initialization scripts:

```lua
local status_ok, toggleterm = pcall(require, "toggleterm")
if not status_ok then
    return
end

local Terminal  = require('toggleterm.terminal').Terminal

toggleterm.setup({
	size = 20,
	open_mapping = [[<c-\>]],
	hide_numbers = true,
	shade_filetypes = {},
	shade_terminals = true,
	shading_factor = 2,
	start_in_insert = true,
	insert_mappings = true,
	persist_size = true,
	direction = "horizontal",
	close_on_exit = true,
	shell = vim.o.shell,
	float_opts = {
		border = "curved",
		winblend = 0,
		highlights = {
			border = "Normal",
			background = "Normal",
		},
	},
})

function _G.set_terminal_keymaps()
  local opts = {noremap = true}
  vim.api.nvim_buf_set_keymap(0, 't', '<S-j>', "<Cmd>lua require'toggleterm'.prev_terminal()<CR><Cmd>startinsert<CR>", opts)
  vim.api.nvim_buf_set_keymap(0, 't', '<S-k>', "<Cmd>lua require'toggleterm'.next_terminal()<CR><Cmd>startinsert<CR>", opts)
  vim.api.nvim_buf_set_keymap(0, 't', '<C-t>', "<Cmd>ToggleTerm<Cr><Cmd>lua require'toggleterm.terminal'.Terminal:new():toggle()<CR>i<BS>", opts)
end

-- if you only want these mappings for toggle term use term://*toggleterm#* instead
vim.cmd('autocmd! TermOpen term://*toggleterm#* lua set_terminal_keymaps()')
```

- I toggle the terminal with `<C+\>`. And create new ones with `<C+t>` while the terminal is open.
- I switch between the terminals with `<S-j>` for next terminal and `<S-k>` for previous one.
