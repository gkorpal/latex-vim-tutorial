# HumanLaTeX: a (n)vim setup for LaTeX for the humanities.

## Contents

√ A simple (n)vim config

√ A small collection of useful (n)vim plugins

√ A collection of useful LaTeX packages

√ A basic LaTeX template to get started

√ Snippets for specific purposes

## Setting up (n)vim

I configure both vim and neovim in vimscript, because I share a single config file between the two editors.
Both editors require little extra config for comfy LaTeX editing, so the settings here are quite vanilla.

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

Now to some of its best features.

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
Use `]]` and `[[` to quickly jump between sections.

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

## LaTeX packages

### fontenc

```
% Set font encoding to T1
% Comment to restore to LaTeX default (OT1)
% If you have no idea what this is, it's best to leave it
\usepackage[T1]{fontenc}
```

This is really just a commonsense setting to bring LaTeX font encoding into the 21st century.
You can read more about it [here](https://tex.stackexchange.com/questions/664/why-should-i-use-usepackaget1fontenc).

### babel

If you're not writing in American English, Babel takes care of pretty much all language-related settings for you.
You can give it the language directly by invoking `\usepackage[dutch]{babel}`, but I prefer doing it as follows:

```
\documentclass[dutch]{article}

% Sets document language based on the argument passed to the \documentclass variable
\usepackage{babel}
```

This way, any other packages that take a language as an argument also know that your document is in Dutch.

### biblatex-chicago

BibLaTeX manages your references and bibliographies.
This particular flavour does so in the [Chicago](https://www.chicagomanualofstyle.org/tools_citationguide.html) style, a popular style in the humanities and social sciences.
If you require a different citation style, check out the resources to see whether you should use plain `biblatex`, or a custom version of it.

In any case, I invoke biblatex-chicago as follows (this should work with any biblatex package):

```
% Enables BibLaTeX-Chicago in the author-date format, with Biber as backend, and suppressing all doi's and isbn's
% Biber needs to be installed on your machine for this to work!
% Change 'authordate' to 'notes' to use footnotes instead
\usepackage[
authordate,
backend=biber,
doi=false,
isbn=false]
{biblatex-chicago}

% Sets the bibliography source file
% Takes both absolute and relative paths, so all of the following are valid:
%	/home/name/Documents/article/sources.bib
%	sources.bib
%	../sources/sources.bib
\addbibresource{filename.bib}

% Enables csquotes, which is recommended for BibLaTeX-Chicago
\usepackage[autostyle=true]{csquotes}
```

Here, I tell it to use the author-date style, to use Biber as its back-end, and to leave out any DOI's and ISBN's in the bibliography.
Then, I point the package to my `.bib` file, which can be produced with a reference manager like [Zotero](https://www.zotero.org/).
And lastly, I enable csquotes, which is recommended by BibLaTeX.

### hyperref

Hyperref enables hyperlinks in your documents.

```
\usepackage{hyperref}
% Set custom colors for urls, links and citations
\hypersetup{
  colorlinks   = true, %Colours links instead of ugly boxes
  urlcolor     = blue, %Colour for external hyperlinks
  linkcolor    = red, %Colour of internal links
  citecolor   = blue %Colour of citations
}

% The 'caption' package enables the [hypcap=true] option (enabled by default), which lets hyperref refer to the figure itself instead of the caption
\usepackage{caption}
```

The only configuration I do here is some visual tweaks, because the default look of links is… not great.
The `caption` package only fixes an issue where clicking a link to a figure jumps the pdf to the caption instead of the figure itself.

### cleveref

Cleveref is a way to make internal references a bit easier.
In particular, Cleveref 'knows' what kind of thing you're referring to (a figure, a section, a page), so that it's easier to fit internal references into a sentence.

```
% Enables Cleveref, which partly automates internal references
% Instead of 'see paragraph \ref{sec:analysis}', write 'see \cref{sec:analysis}'
% Cleveref will automatically append the type of thing you're referencing (e.g. paragraph, figure, appendix, etc.) and do so in the right language
\usepackage{cleveref}
```

### microtype

Microtype makes a bunch of small typesetting changes that make a small but perceptible difference in how your document looks.
It is designed to work its magic out-of-the-box, and newbies (like me) are best off leaving it on its default behaviour.

```
% Microtype makes a bunch of small typesetting changes to make your document look better
% Comment to speed up compiling, or to see what the differences are
\usepackage{microtype}
```

### tgtermes (font)

Fonts are very subjective, but I like Tex Gyre Termes.
If you don't, look for other fonts [here](https://tug.org/FontCatalogue/seriffonts.html) (or hell, just leave it on the default, Computer Modern).

```
% I use TeX Gyre Termes because I think it's pretty
% Comment to restore to LaTeX default (Computer Modern)
\usepackage{tgtermes}
```

### geometry

Set the dimensions of your paper.
I personally don't know what 'letter paper' even is, so I set mine to A4.

```
% Options: a4paper, letterpaper (LaTeX default)
\usepackage[a4paper]{geometry}
```

### setspace

We all know that double spacing looks terrible, but sometimes an assignment requires you to use it.
Setspace has you covered.

```
\usepackage{setspace}
% Options: singlespacing (default), onehalfspacing, doublespacing
\onehalfspacing
```

### comment

This is just a convenience package, allowing you to comment out large portions of text with the `comment` environment.
Bonus: it works perfectly with VimTeX, which will ignore any commented blocks when counting words!

```
% Enables comment blocks
\usepackage{comment}
```

## LaTeX tips & tricks

### a properly formatted table of contents

If you simply plop down your table of contents, you might have a number of problems:

1) it's too widely or narrowly spaced;
2) it's completely red because of hyperref;
3) it includes all the subsubsections, which is too much.

Let's solve all these problems at once:

```
{ 
\onehalfspacing
\hypersetup{linkcolor=black} %To make the links in the ToC black instead of red
\setcounter{tocdepth}{2} % To include only sections (level 1) and subsections (level 2) in the ToC
\tableofcontents
}
```

### abstract and keywords

LaTeX has a separate environment for abstracts, creatively named `abstract`.
Keywords can be formatted manually.

```
\begin{abstract}

\end{abstract}

\textit{\textbf{\small Keywords --- }\small first, second, third, etc.}
```

### subtitles

Make a subtitle by making the title bold and adding a line break.

```
\title{\textbf{Title}\\Subtitle}
```

### subheaders and table of contents

If you want to use the above trick with section headers, you run into the problem of having line breaks in your table of contents (since the table of contents literally copies the entire header).
The solution is to provide a separate ToC header in square brackets.
For example:

```
\section[Instrumental reason: Horkheimer's \textit{Eclipse of Reason}]{Instrumental reason\\ {\large Horkheimer's \textit{Eclipse of Reason}}}
```