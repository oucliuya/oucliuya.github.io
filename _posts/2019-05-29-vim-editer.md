---
title: vim 操作备忘
layout: post
date:   2019-05-29 10:08:00 +0800
categories: document
tag: 笔记
---
## 系统配置
配置完成之后，tab自动变成4个空格
```
" add by school1024.com
set ts=4
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent
```


## 旧文件的处理

### TAB 换成空格
```
:set ts=4
:set expandtab
:%retab!
```

### 空格换成 TAB
```
:set ts=4
:set noexpandtab
:%retab!
```

### vim 配置私藏
修改 ~/.vimrc 文件：
{% highlight bash %}
syntax enable
syntax on
set nocompatible
colorscheme molokai "需要下载molokai color scheme，看过最舒服的vim配色，谁用谁知道，地址https://github.com/tomasr/molokai
let g:rehash256 = 1
set t_Co=256
set backspace=2
set cindent
set cursorline
set cursorcolumn
set autoindent
set smartindent
set tabstop=4
set softtabstop=4
set shiftwidth=4
set expandtab
set showmatch
set incsearch
set hlsearch
set ignorecase
set smartcase
set number
set ruler
set encoding=utf-8
set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1 "编码匹配默认优先级顺序
set undofile
set undodir=~/.vimundodir
{% endhighlight %}

## vim 配置2
{% highlight bash %}
let mapleader=";"
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set encoding=utf-8
set termencoding=utf-8
colorscheme default  
" 开启文件类型侦测
filetype on
" 根据侦测到的不同类型加载对应的插件
filetype plugin on
" 定义快捷键到行首和行尾
nmap LB 0
" 开启实时搜索功能
set incsearch
" 搜索时大小写不敏感
set ignorecase
" 关闭兼容模式
set nocompatible
" vim 自身命令行模式智能补全
set wildmenu
" 禁止光标闪烁
set gcr=a:block-blinkon0
" 总是显示状态栏
set laststatus=2
" 开启行号显示
set number
" 高亮显示当前行/列
set cursorline
"set cursorcolumn
" 高亮显示搜索结果
set hlsearch
" 设置 gvim 显示字体
set guifont=YaHei\ Consolas\ Hybrid\ 11.5
" 禁止折行
"set nowrap
" 设置状态栏主题风格
let g:Powerline_colorscheme='solarized256'
set nocompatible
set laststatus=2
set t_Co=256
if ! has("gui_running")
    set t_Co=256
endif
if &diff
"    colors delek
    colors blue
endif
let g:Powerline_symbols = 'fancy'
let Powerline_symbols='compatible'
" 开启语法高亮功能
syntax enable
" " 允许用指定语法高亮配色方案替换默认方案
syntax on
" 自适应不同语言的智能缩进
filetype indent on
" " 将制表符扩展为空格
set expandtab
" 设置编辑时制表符占用空格数
set tabstop=4
" 设置格式化时制表符占用空格数
set shiftwidth=4
" 让 vim 把连续数量的空格视为一个制表符
set softtabstop=4
" 随 vim 自启动
"let g:indent_guides_enable_on_vim_startup=1
" 从第二层开始可视化显示缩进
let g:indent_guides_start_level=2
" 色块宽度
let g:indent_guides_guide_size=1
" 快捷键 i 开/关缩进可视化
:nmap <silent> <Leader>i <Plug>IndentGuidesToggle
" 基于缩进或语法进行代码折叠
set foldmethod=indent
set foldmethod=syntax
" 启动 vim 时关闭折叠代码
set nofoldenable
" *.cpp 和 *.h 间切换
nmap <Leader>ch :A<CR>
" 子窗口中显示 *.cpp 或 *.h
nmap <Leader>sch :AS<CR>
" 正向遍历同名标签
nmap <Leader>tn :tnext<CR>
" 反向遍历同名标签
nmap <Leader>tp :tprevious<CR>

"NERDTree快捷键
"设置NERDTree子窗口宽度
nnoremap <silent> fl :NERDTreeToggle<CR>
let NERDTreeWinSize=23
let NERDTreeWinPos="left"

" 设置插件 indexer 调用 ctags 的参数
" 默认 --c++-kinds=+p+l，重新设置为 --c++-kinds=+p+l+x+c+d+e+f+g+m+n+s+t+u+v
" 默认 --fields=+iaS 不满足 YCM 要求，需改为 --fields=+iaSl
let g:indexer_ctagsCommandLineOptions="--c++-kinds=+p+l+x+c+d+e+f+g+m+n+s+t+u+v --fields=+iaSl --extra=+q"
" 只能是 #include 或已打开的文件
" 设置 tagbar 子窗口的位置出现在主编辑区的左边 
" 设置显示／隐藏标签列表子窗口的快捷键。速记：tag list 
" 设置标签子窗口的宽度 
" tagbar 子窗口中不显示冗余帮助信息 
" 设置 ctags 对哪些代码元素生成标签
"定义函数SetTitle，自动插入文件头

" *.cpp 和 *.h 间切换
nmap <silent> <Leader>sw :FSHere<cr>
"taglist窗口显示在右侧，缺省为左侧
nnoremap <silent> tl :Tlist<CR>    "设置显示标签列表子窗口的快捷键
let Tlist_Use_Right_Window=1
let Tlist_Exit_OnlyWindow=1          "如果taglist窗口是最后一个窗口，则退出vim
let Tlist_Show_One_File=1           "不同时显示多个文件的tag，只显示当前文件的
let Tlist_WinWidth=20                 "设置标签子窗口的宽度
let OmniCpp_DefaultNamespaces=["_GLIBCXX_STD"]
let g:ycm_global_ycm_extra_conf='/home/mysql/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py'
let g:ycm_seed_identifiers_with_syntax=1
set tags=tags;
set autochdir

"配置miniBufExpl配置
"let g:miniBufExplorerMoreThanOne = 0
"let g:miniBufExplorerMaxHeight=30
"let g:miniBufExplorer_Exit_OnlyWindow = 1          "如果taglist窗口是最后一个窗口，则退出vim
"let g:miniBufExplMapCtabSwitchBufs = 1
"let g:ycm_collect_identifiers_from_tags_files=1   " 开启 YCM

" 基于标签引擎
let g:ycm_min_num_of_chars_for_completion=2   "   
" 从第2个键入字符就开始罗列匹配项
let g:ycm_cache_omnifunc=0    "   
" 禁止缓存匹配项,每次都重新生成匹配项
let g:ycm_seed_identifiers_with_syntax=1  " 语法关键字补全
"nnoremap <F5> :YcmForceCompileAndDiagnostics<CR>  "force recomile with
" syntastic
nnoremap <leader>lo :lopen<CR>   "open locationlist
nnoremap <leader>lc :lclose<CR>  "close locationlist
inoremap <leader><leader> <C-x><C-o>
"在注释输入中也能补全
let g:ycm_complete_in_comments = 1 
"在字符串输入中也能补全
let g:ycm_complete_in_strings = 1 
"注释和字符串中的文字也会被收入补全
let g:ycm_collect_identifiers_from_comments_and_strings = 0 

let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#buffer_nr_show = 1
nnoremap <C-N> :bn<CR>
nnoremap <C-P> :bp<CR>

"每行最长字节个数的竖线
let colorcolumn="100"
highlight CursorLine cterm=NONE ctermbg=blue ctermfg=NONE guibg=NONE guifg=NONE
{% endhighlight %}