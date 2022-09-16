# HumanLaTeX: a (n)vim+LaTeX setup for the humanities and social sciences.

This is for all those LaTeX users in the humanities and social sciences just trying to figure stuff out.
I am one of them.
Here I share what I've learned.
I do this both in the form of general recommendations (scroll down) and a template + snippets (see above).

![Example of an neovim+LaTeX workflow](https://gitlab.com/teunphil/humanlatex/-/raw/main/example.gif)*What a simple neovim+LaTeX workflow could look like.*

---

## Contents

1. [Reasons for using LaTeX](#reasons-for-using-latex)

2. [Reasons for using (n)vim](#reasons-for-using-nvim)

3. [A simple (n)vim config](#a-simple-nvim-config)

4. [Useful (n)vim plugins](#useful-nvim-plugins)

5. [(n)vim tips & tricks](#nvim-tips-tricks)

6. [Useful LaTeX packages](#useful-latex-packages)

7. [LaTeX tips & tricks](#latex-tips-tricks)

8. [Further resources](#further-resources)

## Attachments

- A basic LaTeX template called [sophia.tex](sophia.tex)

- [Snippets](snippets) for specific purposes

---

## Reasons for using LaTeX

LaTeX is different from your average word processor such as Word or Writer.
We call these [What You See Is What You Get (WYSIWYG)](https://en.wikipedia.org/wiki/WYSIWYG) editors, because, well, they let you visually edit your documents in real-time.
In contrast, LaTeX is a *language* in which you can specify the content and form of your document.
This has a few advantages:

**Precision and consistency.**
In LaTeX, what happens to your document is only what you tell it to.
This means that Word annoyances such as random typographic changes or a vague blue background behind pasted text are unthinkable.
LaTeX feels very robust, because you first specify what you want your document to look like (in the 'preamble'), and then you type out your document which will be formatted in that exact way.
This is what people mean when they say that LaTeX "separates form and content".
I'm personally a big fan, because while I'm writing I don't care where exactly the line breaks or how the figure ends up on the page or how that footnote looks.
I trust that these things will sort themselves out (as they almost always do), and I can focus on writing instead.

In Word or Writer, formatting information is often 'hidden': "why is this paragraph all underlined in red?
Ah, it's because the program thinks it's in a different language for some reason."
In LaTeX, every local change is declared explicitly.
So instead of applying a hidden option to a certain chunk of text, you write it out:

```latex
As Descartes said: ``\foreignlanguage{latin}{cogito, ergo sum}''.
```

The same goes for bold and italics, font size, spacing, colour, links, capitalisation, footnotes, all the way up to tables and bibliographies: everything is explicitly declared.
This makes unpleasant surprises almost impossible.

**Ａ Ｅ Ｓ Ｔ Ｈ Ｅ Ｔ Ｉ Ｃ Ｓ.**
If you make a new document in Word or Writer, use all the defaults and write a few sentences with a header, the result is… not dashing.
If you make a new LaTeX document and add the bare minimum, the result looks awesome and professional.
Beautiful, fully justified text in a classy font and with lovely titles and headers (and no blue!).
And with some small tweaks you can make it look even better!

**Referencing.**
In my experience, referencing to sources in Word is okay, but it feels a bit like a hack.
In contrast, referencing to sources in LaTeX is buttery smooth.
You tell it to use a certain style and all formatting happens automatically, including in the bibliography.
The bibliography is automatically populated with all referenced sources.

**Version control.**
`thesis.docx` `thesisfinal.docx` `thesisfinalFINAL.docx` `thesisREALLYFINALFORREAL.docx` `final_thesisREALLYFINALFORREAL.docx`

LaTeX allows you to completely avoid the above situation and streamline the version control process.
This is because LaTeX files are just plaintext files, and plaintext files are manageable using `git`.
If you've never used Git, it is another one of these Spartan-looking programs that are difficult to get into.
But the basics are easy to learn, and there are plenty of [beginners' guides](https://towardsdatascience.com/an-easy-beginners-guide-to-git-2d5a99682a4c).

If you use Git consistently, you'll have a snapshot of every previous state of your document which you can revert to if necessary.
It also allows you to keep track of different 'branches' (for example, for teachers' suggestions) which you can later merge into your main document.
And finally, Git allows for very smooth collaboration in groups.

## Reasons for using (n)vim

Vim (or Neovim) is not the most straightforward text editor to work with.
If you're used to what-you-see-is-what-you-get word processors like Word or Writer, the switch to Vim+LaTeX will cause double whiplash; one for each part of your new setup.
So if that's your background, Vim might not be the obvious choice, and you might actually want to opt for a more 'conventional' editor such as [Overleaf](https://www.overleaf.com/) (online LaTeX editor), [TeXstudio](https://www.texstudio.org/) (offline LaTeX editor) or even a more conventional generic text editor such as [Emacs](https://tex.stackexchange.com/questions/339/latex-editors-ides/356#356).
(See [this Reddit thread](https://old.reddit.com/r/LaTeX/comments/kaqkhq/whats_a_good_latex_editor_for_a_beginner/) and [this Stackexchange thread](https://tex.stackexchange.com/questions/339/latex-editors-ides/) for more inspiration.)
However, there are reasons for opting for Vim or Neovim, despite their rather steep learning curve.

**Efficiency.**
Vim is often touted for being extremely efficient once you know how to work with it.
This is mostly because of its modular nature.
That is: it has different modes of operation where certain keys mean different things.

You start out in 'normal mode', which is an editing efficiency monster.
Every key on your keyboard has an editing purpose in normal mode, which allows you to do almost anything you want in a few keystrokes.
For example, you can delete a line with `dd`.
Deleting three words? `d3w`.
Deleting everything until that closing parenthesis? `df)`
The same goes for moving around.
`50gg` brings you to line 50.
`}` takes you to the next empty line.
`3w` jumps you three words ahead.

This can be extended with LaTeX-specific bindings such as `]]` for jumping to the next section, or `cse` to change the surrounding environment (see the [VimTeX](#vimtex) section).

Because the keyboard is so powerful in normal mode, you don't have to rely on your mouse for moving and editing.
This is, for many people, a very pleasant experience once they learn the basics of normal mode.

It is also these different modes that can make Vim [so insanely complicated](https://rawgit.com/darcyparker/1886716/raw/eab57dfe784f016085251771d65a75a471ca22d4/vimModeStateDiagram.svg).
But if you take it slow and learn only what is most relevant to you, they can be crazy powerful.

**Lightness and speed.**
(n)vim is incredibly light and fast by default.
Even when loaded with plugins, it never has any noticeable input delay for me, which is great for these flowy moments where you just want to type.

**Customisability and extendability.**
(n)vim can be customised to be exactly the way you like.
For example, you can define your own keybindings for common tasks, or thoroughly change Vim's looks and behaviour.
It can also be extended with endless plugins.
For LaTeX writing, [I recommend a few below](#2-nvim-plugins)!

## A simple (n)vim config

Both Vim and Neovim require little extra config for comfy LaTeX editing, so the settings here are quite vanilla.
I share all my settings in Vimscript, because I use one config file for both editors.

The following options are relevant to both vim and neovim:

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
*(with apoligies on behalf of Gitlab's vimscript syntax highlighting)*

The following options are relevant only to vim (since neovim already sets these by default):

```vim
set nocompatible		" no vi compatibility
set autoindent			" enable auto-indentation
set hlsearch			" enable search result highlighting
set backspace=indent,eol,start	" allow backspacing over everything in insert mode.
set ruler			" show the cursor position all the time
set incsearch			" enable incremental search

filetype plugin indent on	" enable file type detection
syntax on			" enable syntax highlighting

```

## Useful (n)vim plugins

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
For example, if you want to add [Awesome Vim Color Schemes](https://github.com/rafi/awesome-vim-colorschemes), simply add this line:

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

→ *Sidenote:* `\` is the default 'leader key' on (n)vim, which is meant to prevent custom keybindings from interfering with existing ones.
This is why a lot of plugins have keybindings that start with `\`.

#### Forward search
Hit `\lv` in a LaTeX document to jump to the corresponding place in the pdf.

#### Table of contents
Hit `\lt` to toggle a table of contents.

#### Folding
`za`	toggle current fold

`zA`	toggle current fold + all underlying folds

`zR`	open all folds ('**R**educe')

`zM`	close all folds	('**M**ore')

#### Moving between sections
Use `]]` and `[[` to quickly jump between sections.

Check the [VimTeX Github](https://github.com/lervag/vimtex#features) for a more complete list of features!

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
→ **NOTE: requires Neovim!**

Automatically continues lists, so you don't have to type `\item` over and over and over.

The only config required is:

```vim
lua require('autolist').setup({})
```

## (n)vim tips & tricks

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

## Useful LaTeX packages

### fontenc

```latex
% Set font encoding to T1
% Comment to restore to LaTeX default (OT1)
% If you have no idea what this is, it's best to leave it
\usepackage[T1]{fontenc}
```

This is really just a commonsense setting to bring LaTeX font encoding into the 21st century.
You can read more about it [here](https://tex.stackexchange.com/questions/664/why-should-i-use-usepackaget1fontenc).

### babel (internationalisation)

If you're not writing in American English, Babel takes care of pretty much all language-related settings for you.
You can give it the language directly by invoking `\usepackage[dutch]{babel}`, but I prefer doing it as follows:

```latex
\documentclass[dutch]{article}

% Sets document language based on the argument passed to the \documentclass variable
\usepackage{babel}
```

This way, any other packages that take a language as an argument also know that your document is in Dutch.

### biblatex-chicago (references and bibliography)

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

### hyperref (hyperlinks)

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
The `caption` package simply fixes an issue where clicking a link to a figure jumps the pdf to the caption instead of the figure itself.

### cleveref

Cleveref is a way to make internal references a bit easier.
In particular, Cleveref 'knows' what kind of thing you're referring to (a figure, a section, a page), so that it's easier to fit internal references into a sentence.

```latex
% Enables Cleveref, which partly automates internal references
% Instead of 'see paragraph \ref{sec:analysis}', write 'see \cref{sec:analysis}'
% Cleveref will automatically append the type of thing you're referencing (e.g. paragraph, figure, appendix, etc.) and do so in the right language
\usepackage{cleveref}
```

### booktabs (tables)

Booktabs makes it easy to create pretty and clear tables.
Specifically, the package follows [a set of rules](https://ftp.snt.utwente.nl/pub/software/tex/macros/latex/contrib/booktabs/booktabs.pdf) for making what they consider 'good tables'.
I agree with them, so I use it.

```latex
% Making tables in LaTeX can be done without any extra packages
% However, booktabs makes tables simple and pretty
\usepackage{booktabs}
```

You can then make a table like this:

```latex
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

### pdfpages (external PDF's)

pdfpages lets you include (parts of) a PDF file in your document.

```latex
% Insert PDF's
\usepackage{pdfpages}
```

Insert a file like this:

```latex
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

### geometry (paper size)

Set the dimensions of your paper.
I personally don't know what 'letter paper' even is, so I set mine to A4.

```latex
% Options: a4paper, letterpaper (LaTeX default)
\usepackage[a4paper]{geometry}
```

### setspace (spacing)

We all know that double spacing looks terrible, but sometimes an assignment requires you to use it.
Setspace has you covered.

```latex
\usepackage{setspace}
% Options: singlespacing (default), onehalfspacing, doublespacing
\onehalfspacing
```

### comment

This is just a convenience package, allowing you to comment out large portions of text with the `comment` environment.
Bonus: it works perfectly with [VimTeX](#vimtex), which will correctly highlight comment blocks and ignore them when counting words!

```latex
% Enables comment blocks
\usepackage{comment}

\begin{comment}
This is all commented out.
And so is this!
\end{comment}
```

## LaTeX tips & tricks

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
\section[Instrumental reason: Horkheimer's Eclipse of Reason]{Instrumental reason\\ {\large Horkheimer's Eclipse of Reason}}
```

### Appendix

When adding an appendix, a few things are important:

1. Starting on a new page;
2. Resetting page numbering;
3. A different page numbering style;
4. Resetting section numbering;
5. A different section numbering style:
6. A separate bibliography with a custom title and a smaller header;
7. A proper place in the table of contents.

Fortunately, the macro `\appendix` takes care of all of these except 3 and 6.

The page numbering style can be changed with the `\pagenumbering` command, and the separate bibliography can be arranged with the `refsection` environment.
The custom title and header can be defined after the `\printbibliography` command.

This is what it looks like:

```latex
% This adds an appendix to the document which is separate from the rest of the document, with a separate bibliography.

% Enables separate numbering for the appendix
% Options: arabic, roman, Roman, alph, Alph
\pagenumbering{roman}

% Creates a separate section of references for the appendix bibliography
\begin{refsection}

\appendix

% Write your body text here as you normally would, including chapters, sections, subsections, etc.
\section{First appendix}

% Prints the appendix bibliography with a smaller heading and custom title
\printbibliography[heading=subbibliography,title=Bibliography Appendix A]

\end{refsection}
```

### Every section on new page

Putting every section on its own page automatically requires defining a new command:

```latex
% Start a new page before every section
\let\stdsection\section
\renewcommand\section{\clearpage\stdsection}
```

### Two columns

Switching to two columns is as easy as typing `\twocolumn`.
However, in a document which consists fully or mostly of two columns, you should add the option `twocolumn` to the `\documentclass` command (see [this thread](https://tex.stackexchange.com/questions/332120/what-is-the-difference-between-twocolumn-and-documentclasstwocolumnbook) for some reasons).

Apart from that, I slightly tweak the default margins based on my own preferences.

```latex
%%%%%%%%%%%%
% Preamble %
%%%%%%%%%%%%

\documentclass[twocolumn]{article}

%%% Margins %%%
% The default column separation is too small for my taste, so I slightly increase it here
% Comment the three commands below to revert the margins to their LaTeX defaults
% If you're curious about what these commands do to the layout exactly, add \addpackage{layout} to the preamble, and \layout to the main text to see a visual representation

% Increase the column separation by 10pt
\addtolength{\columnsep}{10pt}
% Compensate by pushing the whole text 5pt left
\addtolength{\hoffset}{-5pt}
% Compensate by adding 10pt to the total text width, effectively pushing both columns 5pt outward
\addtolength{\textwidth}{10pt}
```

Switch to one-column mode with `\onecolumn`, and back with `\twocolumn`.

### Full-width figure in two-column mode
If you insert a regular figure while in two-column mode, it will only span a single column.
For a figure to take up the full two columns, simply add an asterisk, like so:

```latex
\begin{figure*}[h]
	\begin{center}
		% You can specify the size of the image by setting 'width=' or 'height='
		% The size can be set absolutely (in px, cm, etc.), relatively (with 'scale=1.5') or relative to \textwidth, like done below
		\includegraphics[width=0.8\textwidth]{image.png}
		\caption{A very beautiful image.}
		\label{fig:beautiful}
	\end{center}
\end{figure*}
```

---

## Further resources

### LaTeX
- [Overleaf's 30-minute introduction](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes) – Concise walkthrough for beginners.
- [Blog post: LaTeX for the humanities](https://www.overleaf.com/blog/636-guest-blog-post-latex-for-the-humanities) – Excellent writeup on why to use LaTeX in the humanities.
- [LaTeX for Philosophers](https://tanksley.me/latex-for-philosophers/index) – *Very* thorough LaTeX tutorial geared towards philosophy and the humanities in general. (I still need to go through this one.)
- [Getting to Grips with LaTeX](https://www.andy-roberts.net/writing/latex) – 12-step LaTeX walkthrough that starts at the very beginning. Great for those who are new to LaTeX!
- [LaTeX-doc-ptr](https://mirrors.evoluso.com/CTAN/info/latex-doc-ptr/latex-doc-ptr.pdf) – A great list of general LaTeX recommendations.

### Vim
- [Vim user manual](https://www.vi-improved.org/vimusermanual.pdf) – The official user-oriented Vim guide. Surprisingly readable and surprisingly useful!
- [Idiomatic vimrc](https://github.com/romainl/idiomatic-vimrc) – A very sensible guide to setting up your own vim config.
- [/u/kaisunc's Vim cheatsheet](https://old.reddit.com/r/vim/comments/n6qfu2/update_my_vim_cheatsheet_static_printable/) – A lovely visual Vim cheatsheet which covers most basic commands.
- [rtorr's Vim cheatsheet](https://vim.rtorr.com/) – A much more comprehensive cheatsheet.
- `vimtutor` – A built-in interactive getting-started guide to Vim. Type `vimtutor` in a terminal to open.
- [Openvim](https://openvim.com/) – An interactive Vim tutorial that covers the basics, much like vimtutor.
- `:h` – Vim's help files are a massive treasure trove of information. Want to know more about syntax highlighting? Type `:h syntax-highlighting` and you're off.
