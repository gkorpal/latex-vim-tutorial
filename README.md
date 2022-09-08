# HumanLaTeX: a (n)vim setup for LaTeX for the humanities.

## Contents

√ A simple (n)vim config
√ A small collection of useful (n)vim plugins
√ A basic LaTeX template to get started
√ Snippets for specific purposes

## Setting up (n)vim

I configure both vim and neovim in vimscript, because I share a single config file between the two editors.
Both editors require little extra config for comfy LaTeX editing, so the settings here are quire vanilla.

The following options are relevant to both vim and neovim:

```
set number			" show number line
set ignorecase			" ignore case when searching
set smartcase			" ignore case only with lowercase letters
set wrap			" enable soft wrapping at the edge of the screen
set linebreak 			" don't wrap in the middle of a word
set mouse=a			" enable mouse in all modes

autocmd VimEnter * set cul	" add cursorline when not in insert mode
autocmd InsertLeave * set cul
autocmd InsertEnter * set nocul
```

The following options are relevant only to vim (since neovim already sets these by default):

```
set autoindent			" enable auto-indentation
set hlsearch			" enable search result highlighting
set backspace=indent,eol,start	" allow backspacing over everything in insert mode.
set ruler			" show the cursor position all the time
set incsearch			" enable incremental search

filetype plugin indent on	" enable file type detection
syntax on			" enable syntax highlighting

```

## (n)vim plugins

### [vim-plug](https://github.com/junegunn/vim-plug)
You can manage your plugins [manually](https://gist.github.com/manasthakur/ab4cf8d32a28ea38271ac0d07373bb53#managing-plugins-natively-using-vim-8-packages), but for me, there's little reason not to use vim-plug.
It's incredibly simple and supports anything that can be found on GitHub.
Install it by following the instructions on the GitHub page.

Then, add the following to your (n)vim config:

```
call plug#begin()	

call plug#end()
```

In between these two lines, you can now start adding your plugins line-by-line.
You do this by starting a line with `Plug`, and then referring to the author name and plugin name on GitHub.
For example, if you want to add [Awesome Vim Color Schemes](https://github.com/rafi/awesome-vim-colorschemes), simply add the line

```
Plug 'rafi/awesome-vim-colorschemes'	" color schemes

```

You can (and should) read more about how vim-plug works on the GitHub page, but the basic operations are:

`PlugInstall`	Install any new plugins from the config
`PlugUpdate`	Update all plugins
`PlugUpgrade`	Upgrade vim-plug itself

### [VimTeX](https://github.com/lervag/vimtex)

If you're going to install just one LaTeX-related plugin, make it this one.
VimTeX is a very complete filetype plugin for LaTeX files.
It has many config options, but I have only set these three:

```
let g:vimtex_view_method = 'zathura'	" set VimTeX default pdf viewer
let g:vimtex_fold_enabled = 1 		" enable VimTeX folding
let g:vimtex_fold_manual = 1		" set VimTeX folding to manual
```

#### Auto-compilation
Perhaps the most convenient feature is auto-compilation.
To enable it, simply open a LaTeX document and hit `\ll`.
(`\` is the default 'leader key' on (n)vim, which is meant to prevent custom keybindings from plugins or users themselves to interfere with existing keybindings.
This is why a lot of plugins have keybindings that start with `\`.)

#### Forward search
Hit `\lv` in a LaTeX document to jump to the corresponding place in the pdf.

#### Table of contents
Hit `\lt` to toggle a table of contents.

#### Folding
`za`	toggle current fold
`zA`	toggle current fold + all underlying folds
`zR`	open all folds (Reduce)
`zM`	close all folds	(More)

#### Moving between sections
use `]]` and `[[` to quickly jump between sections.

### [Limelight](https://github.com/junegunn/limelight.vim) and [Goyo](https://github.com/junegunn/goyo.vim)
These two plugins combined add a pleasant distraction-free writing mode.
I have them set up as follows:

```
autocmd! User GoyoEnter Limelight	" Limelight + Goyo integration
autocmd! User GoyoLeave Limelight!

noremap <leader>gg :Goyo<CR>|	 " hotkey for Goyo toggle
```

This way, `\gg` toggles Goyo and Limelight together.

### [Autolist](https://github.com/gaoDean/autolist.nvim)
Automatically continues lists, so you don't have to type `\item` over and over and over.

The only config required is:

```
:lua require('autolist').setup({})
```
