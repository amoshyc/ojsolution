###################################################
Vimrc
###################################################

.. sidebar:: Tags

    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: vim
    :linenos:

    set number	" Show line numbers
    set linebreak	" Break lines at word (requires Wrap lines)
    set showbreak=+++	" Wrap-broken line prefix
    set textwidth=150	" Line wrap (number of cols)
    set showmatch	" Highlight matching brace
    set visualbell	"  Use visual bell (no beeping)

    set hlsearch	" Highlight all search results
    set smartcase	" Enable smart-case search
    set ignorecase	" Always case-insensitive
    set incsearch	" Searches for strings incrementally

    set autoindent	" Auto-indent new lines
    set shiftwidth=4	 " Number of auto-indent spaces
    set smartindent	" Enable smart-indent
    set smarttab	" Enable smart-tabs
    set softtabstop=4	 " Number of spaces per Tab
    set cursorline	" Show underline
    set cursorcolumn
    set ruler	" Show row and column ruler information

    set mouse=a

    set undolevels=10000	" Number of undo levels
    set backspace=indent,eol,start	" Backspace behaviour

    set splitbelow	"New spilt starts below
    set splitright	"New split starts on the right
    set scrolloff=5 "auto scroll on the bottom 5 lines

    filetype on	"enable file detection
    syntax on	"syntax highlight

    highlight Comment ctermfg=cyan
    set showmode

    set encoding=utf-8
    set fileencoding=utf-8
    scriptencoding=utf-8
