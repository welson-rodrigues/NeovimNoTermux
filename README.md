# Neovim com Tema no Termux

Este guia mostra como instalar o Neovim no Termux e configurar um tema moderno (Tokyonight) com o gerenciador de plugins Lazy.nvim. No final da leitura você vai encontrar o link do vídeo onde explico com detalhes como usar o Neovim no celular.

---

## Requisitos

- Termux atualizado
- Acesso à internet
- Arquitetura ARM64 (funciona na maioria dos celulares)

---

## 1. Instale o Neovim

```bash
pkg update && pkg upgrade
```
```bash
pkg install neovim
```

## Verifique se foi instalado:
```bash
nvim --version
```

# 2. Configure o tema Tokyonight com Lazy.nvim
## Crie os diretórios de configuração:
```bash
mkdir -p ~/.config/nvim
```
```bash
nvim ~/.config/nvim/init.lua
```

## Cole este conteúdo no init.lua:

```bash
-- Instalar lazy.nvim automaticamente
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git",
    "clone",
    "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable",
    lazypath,
  })
end
vim.opt.rtp:prepend(lazypath)

-- Plugins
require("lazy").setup({
  "folke/tokyonight.nvim", -- Tema

  -- File explorer (Ctrl+b para abrir/fechar)
  {
    "nvim-tree/nvim-tree.lua",
    dependencies = { "nvim-tree/nvim-web-devicons" },
    config = function()
      require("nvim-tree").setup({})
      vim.keymap.set("n", "<C-b>", ":NvimTreeToggle<CR>", { noremap = true, silent = true })
    end
  }
})

-- Ativar tema
vim.cmd[[colorscheme tokyonight]]

-- Linhas numeradas
vim.opt.number = true
vim.opt.relativenumber = true
```

# 3. Salve e saia do Neovim
- Aperte ESQ
- Digite :wq e dê Enter

# 4. Execute o Neovim

```bash
nvim
```

## Na primeira execução, o Lazy.nvim instalará o tema automaticamente.
