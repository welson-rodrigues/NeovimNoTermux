# Neovim com Tema no Termux

Este guia mostra como instalar o Neovim no Termux e configurar um tema moderno (Tokyonight) com o gerenciador de plugins Lazy.nvim.

---

## 🧰 Requisitos

- Termux atualizado
- Acesso à internet
- Arquitetura ARM64 (funciona na maioria dos celulares)

---

## 1. Instale o Neovim

```bash
pkg update && pkg upgrade
pkg install neovim git curl
```

# Verifique se foi instalado:
```bash
nvim --version
```

## 2. Configure o tema Tokyonight com Lazy.nvim
# Crie os diretórios de configuração:
```bash
mkdir -p ~/.config/nvim
nvim ~/.config/nvim/init.lua
```

# Cole este conteúdo no init.lua:

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
})

-- Ativar tema
vim.cmd[[colorscheme tokyonight]]
```

# Clique em ESQ se estiver no modo de edição, e salve com :wq

## 3. Execute o Neovim

```bash
nvim
```

# Na primeira execução, o Lazy.nvim instalará o tema automaticamente.
