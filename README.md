# HumanLaTeX: a (n)vim+LaTeX setup for the humanities and social sciences.

This is for all those LaTeX users in the humanities and social sciences just trying to figure stuff out.
I am one of them.
Here I share what I've learned.
I do this both in the form of a 'tutorial' (scroll down) and a template + snippets (see above).

→ **NOTE:** if you're completely new to LaTeX, this is probably not the place to start.
You can get acquainted with LaTeX through [Overleaf's 30-minute introduction](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes), and check out [LaTeX-doc-ptr](https://mirrors.evoluso.com/CTAN/info/latex-doc-ptr/latex-doc-ptr.pdf) for a great list of general recommendations.

![Example of an neovim+LaTeX workflow](https://gitlab.com/teunphil/humanlatex/-/raw/main/example.gif)*What a simple neovim+LaTeX workflow could look like.*

## Contents

1. A simple [(n)vim config](#1-nvim-config)

2. A small collection of [useful (n)vim plugins](#2-nvim-plugins)

3. Some [(n)vim tips & tricks](#3-nvim-tips-tricks)

4. A collection of [useful LaTeX packages](#4-latex-packages)

5. Some [LaTeX tips & tricks](#5latex-tips-tricks)

## Attachments

- A basic LaTeX template called [sophia.tex](sophia.tex)

- [Snippets](snippets) for specific purposes

## 1. (n)vim config

I configure both vim and neovim in vimscript, because I share a single config file between the two editors.
Both editors require little extra config for comfy LaTeX editing, so the settings here are quite vanilla.

The following options are relevant to both vim and neovim:
(I apologise on behalf of Gitlab's vim syntax highlighting.)

```vim
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

```vim
set autoindent			" enable auto-indentation
set hlsearch			" enable search result highlighting
set backspace=indent,eol,start	" allow backspacing over everything in insert mode.
set ruler			" show the cursor position all the time
set incsearch			" enable incremental search

filetype plugin indent on	" enable file type detection
syntax on			" enable syntax highlighting

```

## 2. (n)vim plugins

### [vim-plug](https://github.com/junegunn/vim-plug)
You can manage your plugins [manually](https://gist.github.com/manasthakur/ab4cf8d32a28ea38271ac0d07373bb53#managing-plugins-natively-using-vim-8-packages), but for me, there's little reason not to use vim-plug.
It's incredibly simple and supports anything that can be found on GitHub.
Install it by following the instructions on the GitHub page.

Then, add the following to your (n)vim config:

```vim
call plug#begin()	

call plug#end()
```

In between these two lines, you can now start adding your plugins line-by-line.
You do this by starting a line with `Plug`, and then referring to the author name and plugin name on GitHub.
For example, if you want to add [Awesome Vim Color Schemes](https://github.com/rafi/awesome-vim-colorschemes), simply add the line

```vim
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

```vim
let g:vimtex_view_method = 'zathura'	" set VimTeX default pdf viewer
let g:vimtex_fold_enabled = 1 		" enable VimTeX folding
let g:vimtex_fold_manual = 1		" set VimTeX folding to manual
```

Now to some of its best features.

#### Auto-compilation
Perhaps the most convenient feature is auto-compilation.
To enable it, simply open a LaTeX document and hit `\ll`.

→ *Sidenote:* `\` is the default 'leader key' on (n)vim, which is meant to prevent custom keybindings from plugins or users to interfere with existing keybindings.
This is why a lot of plugins have keybindings that start with `\`.

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

```vim
autocmd! User GoyoEnter Limelight	" Limelight + Goyo integration
autocmd! User GoyoLeave Limelight!

noremap <leader>gg :Goyo<CR>|	 " hotkey for Goyo toggle
```

This way, `\gg` toggles Goyo and Limelight together.

### [Autolist](https://github.com/gaoDean/autolist.nvim)
→ **NOTE:** This is the only plugin here that requires neovim!

Automatically continues lists, so you don't have to type `\item` over and over and over.

The only config required is:

```vim
:lua require('autolist').setup({})
```

## 3. (n)vim tips & tricks

### Spellcheck

To enable basic spellchecking, install `hunspell` + all relevant Hunspell dictionaries to your system.
I added the following to my config:

```vim
map <F6> :setlocal spell! spelllang=en_UK<CR>|	" toggle UK_EN spellcheck
map <F7> :setlocal spell! spelllang=nl_NL<CR>|	" toggle NL spellcheck
```

This will give you a hotkey for every language.
(Vim is smart enough to detect and use Hunspell's dictionaries automatically.)

### Snippets

Personally, I don't find myself using snippets all that much.
There's something satisfying to typing out commands and environments on muscle memory, and I feel that snippets intrude on my muscle memory more than that they actually enhance the writing experience.

Still, for those that are interested, there's roughly two ways to use snippets: the lazy way and the manual way.

**The lazy way** is to use a plugin like [UltiSnips](https://github.com/SirVer/ultisnips).
You can get started by adding the following to your config

```vim
let g:UltiSnipsSnippetDirectories  = ['/.vim/UltiSnips']	" set UltiSnips directory
let g:UltiSnipsExpandTrigger       = '<Tab>'    		" use Tab to expand snippets
let g:UltiSnipsJumpForwardTrigger  = '<Tab>'    		" use Tab to move forward through tabstops
let g:UltiSnipsJumpBackwardTrigger = '<S-Tab>'  		" use Shift-Tab to move backward through tabstops
```

and using [this](https://github.com/honza/vim-snippets/blob/master/UltiSnips/tex.snippets) snippets file.

**The manual way** I learned from [Max Cantor](https://youtu.be/XA2WjJbmmoM?t=2303), and it looks like this:

```vim
nnoremap ,image :-1read $HOME/Documents/LaTeX\ Practice/templates/sophia/snippets/image.tex<CR>d/%%% Body %%%<CR>d2d/image.png<CR>gnc
nnoremap ,table :-1read $HOME/Documents/LaTeX\ Practice/templates/sophia/snippets/table.tex<CR>d/%%% Body %%%<CR>d2d/midrule<CR>j0c3c
```

To understand how this crazy-looking mapping works, do check out his video.
But basically, the string after `nnoremap` is what you type in normal mode to trigger the snippet, the string after `:-1read` is the path to the file you want to throw in your document, and the stuff after `<CR>` are the keystrokes you want to be pressed after the snippet is pasted.
Pretty simple!

## 4. LaTeX packages

### fontenc

```latex
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

```latex
\documentclass[dutch]{article}

% Sets document language based on the argument passed to the \documentclass variable
\usepackage{babel}
```

This way, any other packages that take a language as an argument also know that your document is in Dutch.

### biblatex-chicago

BibLaTeX manages your references and bibliographies.
This particular flavour does so in the [Chicago](https://www.chicagomanualofstyle.org/tools_citationguide.html) style, a popular style in the humanities and social sciences.

→ **NOTE:** If you require a different citation style, you should do some research to see whether you should use plain `biblatex` or a custom version of it.
It seems that for MLA you can use [biblatex-mla](https://ctan.org/pkg/biblatex-mla), and for APA there is [biblatex-apa](https://ctan.org/pkg/biblatex-apa) (both have been updated in 2022), but please do some [Ducking](https://duckduckgo.com/) before sticking to a method.

In any case, I invoke `biblatex-chicago` as follows (this should work with any biblatex package):

```latex
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

```latex
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

```latex
% Enables Cleveref, which partly automates internal references
% Instead of 'see paragraph \ref{sec:analysis}', write 'see \cref{sec:analysis}'
% Cleveref will automatically append the type of thing you're referencing (e.g. paragraph, figure, appendix, etc.) and do so in the right language
\usepackage{cleveref}
```

### booktabs

Booktabs makes it easy to create pretty and clear tables.
Specifically, the package follows [a set of rules](https://ftp.snt.utwente.nl/pub/software/tex/macros/latex/contrib/booktabs/booktabs.pdf) for making what they consider 'good tables'.
I agree with them, so I use it.

```latex
%%%%%%%%%%%%
% Preamble %
%%%%%%%%%%%%

% Making tables in LaTeX can be done without any extra packages
% However, booktabs makes tables simple and pretty
\usepackage{booktabs}

%%%%%%%%%%%%%
% Body text %
%%%%%%%%%%%%%

% This is a simple example table; feel free to modify it to your needs.
% By default, this table is put in a floating figure.
% (For more info on figures, see the `image' snippet)
% To put this table in-line instead, comment the lines \begin{figure}, \caption, \label and \end{figure}
\begingroup % A group is created to quarantine the distance parameters set below
% The two lines below increase the distance between columns (\tabcolsep) and between rows (\arraystretch).
\setlength{\tabcolsep}{9pt} % Default LaTeX value: 6pt – Default Sophia value: 9pt
\renewcommand{\arraystretch}{1.5} % Default LaTeX value: 1 – Default Sophia value: 1.5
\begin{figure}
% This table has two paragraphs that wrap their text automatically (hence the p's).
% Besides 'p', other options include c (center), l (left align) and r (right align).
\begin{tabular}{p{0.45\textwidth} p{0.45\textwidth}} \toprule
	\textbf{Header 1} & \textbf{Header 2} \\
	\midrule
	Some text on the left. & Some more text on the right. \\
	This is a somewhat longer though not unreasonably long piece of text. & This is short. \\
	Etc. etc. & You probably get the point by now. \\
		\bottomrule
\end{tabular}
\caption{A very simple example table.}
\label{fig:table}
\end{figure}
\endgroup
```

### pdfpages

pdfpages lets you include (parts of) a PDF file in your document.

```latex
%%%%%%%%%%%%
% Preamble %
%%%%%%%%%%%%

\usepackage{pdfpages}

%%%%%%%%%%%%%
% Body text %
%%%%%%%%%%%%%

% The file will be automatically be given its own page, no need to do a \pagebreak before or after
\begin{figure}[h]
    \centering
% If you only want to include certain pages from a document, write them in the first set of curly braces
% Write the filename without extension in the second set of curly brackets
    \includepdf[pages={-}]{frontpage}
\end{figure}
```

### microtype

Microtype makes a bunch of small typesetting changes that make a small but perceptible difference in how your document looks.
It is designed to work its magic out-of-the-box, and newbies (like me) are best off leaving it on its default behaviour.

```latex
% Microtype makes a bunch of small typesetting changes to make your document look better
% Comment to speed up compiling, or to see what the differences are
\usepackage{microtype}
```

### tgtermes (font)

Fonts are very subjective, but I like Tex Gyre Termes.
If you don't, look for other fonts [here](https://tug.org/FontCatalogue/seriffonts.html) (or hell, just leave it on the default, Computer Modern).

```latex
% I use TeX Gyre Termes because I think it's pretty
% Comment to restore to LaTeX default (Computer Modern)
\usepackage{tgtermes}
```

### geometry

Set the dimensions of your paper.
I personally don't know what 'letter paper' even is, so I set mine to A4.

```latex
% Options: a4paper, letterpaper (LaTeX default)
\usepackage[a4paper]{geometry}
```

### setspace

We all know that double spacing looks terrible, but sometimes an assignment requires you to use it.
Setspace has you covered.

```latex
\usepackage{setspace}
% Options: singlespacing (default), onehalfspacing, doublespacing
\onehalfspacing
```

### comment

This is just a convenience package, allowing you to comment out large portions of text with the `comment` environment.
Bonus: it works perfectly with VimTeX, which will ignore any commented blocks when counting words!

```latex
% Enables comment blocks
\usepackage{comment}

\begin{comment}
This is all commented out.
And so is this!
\end{comment}
```

## 5. LaTeX tips & tricks

### A properly formatted table of contents

If you simply plop down your table of contents, you might have a number of problems:

1) it's too widely or narrowly spaced;
2) it's completely red because of hyperref;
3) it includes all the subsubsections, which is too much.

Let's solve all these problems at once:

```latex
{ 
\onehalfspacing
\hypersetup{linkcolor=black} %To make the links in the ToC black instead of red
\setcounter{tocdepth}{2} % To include only sections (level 1) and subsections (level 2) in the ToC
\tableofcontents
}
```

### Abstract and keywords

LaTeX has a separate environment for abstracts, creatively named `abstract`.
Keywords can be formatted manually.

```latex
\begin{abstract}

\end{abstract}

\textit{\textbf{\small Keywords --- }\small first, second, third, etc.}
```

### Subtitles

Make a subtitle by making the title bold and adding a line break.

```latex
\title{\textbf{Title}\\Subtitle}
```

### Subheaders and table of contents

If you want to use the above trick with section headers, you run into the problem of having line breaks in your table of contents (since the table of contents literally copies the entire header).
The solution is to provide a separate ToC header in square brackets.
For example:

```latex
\section[Instrumental reason: Horkheimer's \textit{Eclipse of Reason}]{Instrumental reason\\ {\large Horkheimer's \textit{Eclipse of Reason}}}
```
