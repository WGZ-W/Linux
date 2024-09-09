## 基础设置
- set showcmd
- set showmode
- set autoindent
- set cindent				## c format indent
- set softtabstop=4
- set noexpandtab
- set nocompatible
- set background=dark

## 帮助文档
```
    :help vimrc
```
## 不兼容 vi
```
    set nocompatible
```
## Enable type file detection. Vim will be able to try to detect the type of file in use.
```
- filetype on
```

## Enable plugins and load plugin for the detected file type.
```
- filetype plugin on
```

## Load an indent file for the detected file type.
```
- filetype indent on
```

## Turn syntax highlighting on.
```
- syntax on
```

## Add numbers to each line on the left-hand side.
```
- set number

- set relativenumber
```

## Highlight cursor line underneath the cursor horizontally.
```
- set cursorline
```

## Highlight cursor line underneath the cursor vertically.
```
## - set cursorcolumn
```

## - set shift width to 4 spaces.
```
- set shiftwidth=4
```

## - set tab width to 4 columns.
```
- set tabstop=4
```

## Use space characters instead of tabs.
```
## - set expandtab
```

## Do not save backup files.
```
- set nobackup
```

## Do not let cursor scroll below or above N number of lines when scrolling.
## - set scrolloff=10

## Do not wrap lines. Allow long lines to extend as far as the line goes.
## - set nowrap

## While searching though a file incrementally highlight matching characters as you type.
- set incsearch

## Ignore capital letters during search. 补全和搜索忽略大小写
- set ignorecase

## Override the ignorecase option if searching for capital letters.
## This will allow you to search specifically for capital letters.
- set smartcase

## Show partial command you type in the last line of the screen.
- set showcmd

## Show the mode you are on the last line.
- set showmode

## Show matching words during a search.
- set showmatch

## Use highlighting when doing a search.
- set hlsearch

## - set the commands to save in history default number is 20.
- set history=1000

## Enable auto completion menu after pressing TAB.
- set wildmenu

## Make wildmenu behave like similar to Bash completion.
- set wildmode=list:longest

## There are certain files that we would never want to edit with Vim.
## Wildmenu will ignore files with these extensions.
- set wildignore=*.docx,*.jpg,*.png,*.gif,*.pdf,*.pyc,*.exe,*.flv,*.img,*.xlsx


## VIMSCRIPT -------------------------------------------------------------- {{{

## This will enable code folding.
## Use the marker method of folding.
augroup filetype_vim
    autocmd!
    autocmd FileType vim - setlocal foldmethod=marker
augroup END

## More Vimscripts code goes here.

## }}}







## 设置一些映射
noremap <M-j> <Down>
noremap <M-k> <Up>
noremap <M-h> <Left>
noremap <M-l> <Right>




## 定义一些函数
map <F5> :call CompileRunGcc()<CR>
func! CompileRunGcc()
	exec ##w##
	exec ##!gcc % -o %<##
	exec ##! ./%<##
endfunc

