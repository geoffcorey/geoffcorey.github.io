---
title: NeoVim
tags: dotfiles, vim, neovim
toc: true
season : summer
---
Custom Vi setup

## Plugins

* Plug 'bling/vim-airline'
* Plug 'chrisbra/NrrwRgn'
* Plug 'christoomey/vim-tmux-navigator'
* Plug 'easymotion/vim-easymotion'
* Plug 'editorconfig/editorconfig-vim'
* Plug 'junegunn/fzf.vim'
* Plug 'junegunn/goyo.vim'
* Plug 'junegunn/limelight.vim'
* Plug 'rbgrouleff/bclose.vim'
* Plug 'scrooloose/nerdcommenter'
* Plug 'scrooloose/nerdtree'
* Plug 'ryanoasis/vim-devicons'
* Plug 'terryma/vim-multiple-cursors'
* Plug 'tpope/vim-fugitive'
* Plug 'tpope/vim-surround'
* Plug 'vimwiki/vimwiki'
* Plug 'neoclide/coc.nvim'
* Plug 'lifepillar/pgsql.vim'
* Plug 'tclh123/vim-thrift'
* Plug 'morhetz/gruvbox'

## Keymaps

### General

* , LEADER
* ,bg Toggle Colors

### Search

* ,c Clear search highlights

### Navigation

* \<S-Left\> Previous Buffer
* \<S-Right\> Next Buffer
* ,w buffer close
* \<c-h\> TMUX Left
* \<c-j\> TMUX Down
* \<c-k\> TMUX Up
* \<c-l\> TMUX Right
* \<c-\\> TMUX Previousq

### Files

* ,d Nerdtree toggle
* \<F2\> Nerdtree toggle

### Splits

* ,v Vertical Split
* ,h Horizontal Split

### Code

* gd definition
* gy type definition
* gi implimentation
* gr references
* \[g diag prev
* \]g diag next
* ,a Codeaction selected
* ,f Format selected code
* \<space\>a diag
* \<space\>e ext
* \<apace\>o outline
* \<space\>s symbols
* \<space\>j next
* \<space\>k prev
* \<space\>p list resume
* \<F3\> Tag bar toggle

### Commands

* :Format Formatter
* :Fold Fold function
* :OR Organize imports