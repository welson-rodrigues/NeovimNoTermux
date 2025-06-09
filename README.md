# Neovim com Tema no Termux

Este guia mostra como instalar o Neovim no Termux e configurar um tema moderno (Tokyonight) com o gerenciador de plugins Lazy.nvim. Também vamos adicionar explorador de arquivos, barra de status, autocompletar e suporte a LSP — tudo isso direto no celular. No final da leitura você encontra o link do vídeo onde explico tudo passo a passo.

---

## Requisitos

- Termux atualizado
- Acesso à internet
- Arquitetura ARM64 (funciona na maioria dos celulares)

---

## 1. Instale o Neovim
- Atualize os pacotes:
```bash
pkg update && pkg upgrade
```
- Instale o Neovim:
```bash
pkg install neovim
```

- Verifique se foi instalado:
```bash
nvim --version
```

# 2. Configure o Neovim com Tema e Plugins
- Crie as pastas de configuração:
```bash
mkdir -p ~/.config/nvim
```
```bash
nvim ~/.config/nvim/init.lua
```

- Cole este conteúdo no init.lua:
```bash
-- Tecla líder
vim.g.mapleader = " "

-- Aparência e número de linhas
vim.opt.number = true
vim.opt.relativenumber = true
vim.opt.termguicolors = true

-- Lazy.nvim (gerenciador de plugins)
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git", "clone", "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable", lazypath,
  })
end
vim.opt.rtp:prepend(lazypath)

-- Plugins
require("lazy").setup({
  "folke/tokyonight.nvim",

  -- Nvim Tree (explorador de arquivos)
  {
    "nvim-tree/nvim-tree.lua",
    dependencies = { "nvim-tree/nvim-web-devicons" },
    config = function()
      require("nvim-tree").setup()
      vim.keymap.set("n", "<C-b>", ":NvimTreeToggle<CR>", { noremap = true, silent = true })
    end,
  },

  -- Barra de status
  {
    "nvim-lualine/lualine.nvim",
    dependencies = { "nvim-tree/nvim-web-devicons" },
    config = function()
      require("lualine").setup()
    end,
  },

  -- Mason (gerenciador de LSPs e ferramentas)
  {
    "williamboman/mason.nvim",
    config = function()
      require("mason").setup()
    end,
  },

  -- LSP config
  "neovim/nvim-lspconfig",
  "williamboman/mason-lspconfig.nvim",

  -- Autocompletar
  "hrsh7th/nvim-cmp",
  "hrsh7th/cmp-nvim-lsp",
  "hrsh7th/cmp-buffer",
  "hrsh7th/cmp-path",
  "hrsh7th/cmp-cmdline",
  "L3MON4D3/LuaSnip",
  "saadparwaiz1/cmp_luasnip",
})

-- Ativar tema
vim.cmd[[colorscheme tokyonight]]

-- LSP + Autocomplete
require("mason-lspconfig").setup({
  ensure_installed = { "lua_ls", "pyright" },
})

local lspconfig = require("lspconfig")
local capabilities = require("cmp_nvim_lsp").default_capabilities()

-- Lua LSP
lspconfig.lua_ls.setup {
  capabilities = capabilities,
}

-- Python LSP
lspconfig.pyright.setup {
  capabilities = capabilities,
}

-- Configurar autocompletar
local cmp = require("cmp")
local luasnip = require("luasnip")

cmp.setup({
  snippet = {
    expand = function(args)
      luasnip.lsp_expand(args.body)
    end,
  },
  mapping = cmp.mapping.preset.insert({
    ['<Tab>'] = cmp.mapping.select_next_item(),
    ['<S-Tab>'] = cmp.mapping.select_prev_item(),
    ['<CR>'] = cmp.mapping.confirm({ select = true }),
  }),
  sources = cmp.config.sources({
    { name = "nvim_lsp" },
    { name = "luasnip" },
  }, {
    { name = "buffer" },
  })
})
```

# 3. Salve e saia do Neovim
- Aperte ESQ
- Digite :wq e pressione Enter

# 4. Execute o Neovim
```bash
nvim
```

# Vídeo Tutorial

