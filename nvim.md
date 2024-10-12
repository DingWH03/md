# 我的nvim配置

## nvim版本

由于debian/ubuntu镜像源仓库内nvim版本较低，我采用源码编译安装的方法，安装方法也较为简单，克隆源代码之后直接编译安装即可。由于系统环境中安装了nvim，避免破环系统环境，我将nvim安装在home目录下并手动配置用户变量。

```bash
git clone https://github.com/neovim/neovim.git # 克隆代码
make CMAKE_BUILD_TYPE=RelWithDebInfo CMAKE_INSTALL_PREFIX=/home/dwh/Apps/nvim/ -j6 # 设置目录和编译线程数
make install
```

最后配置$PATH，添加`export PATH=/home/dwh/Apps/nvim/bin:$PATH`到~/.bashrc中即可（或其他shell配置文件）。

```shell
$ nvim --version
NVIM v0.11.0-dev-943+g641c4b1a2
Build type: RelWithDebInfo
LuaJIT 2.1.1727870382
Run "nvim -V1 -v" for more info
```

## config目录结构

```
.
├── init.lua # nvim配置文件入口
├── lua
│   ├── keymaps.lua # 按键映射
│   ├── options.lua # 配置选项
│   └── plugins
├── README.md
└── vimrc
```

## 基础配置vimrc

> 使用`ln -s ~/.config.nvim/vimrc ~/.vimrc`命令可以将配置文件共享给vim使用。

### Options

```
" 编码
set encoding=utf-8
set fileencodings=utf-8,gb2312,gbk,gb18030,latin1
set fileformat=unix
set fileformats=unix,dos
 
" 缩进与格式
filetype indent on
set autoindent
set smarttab
set cindent
set shiftwidth=4
set tabstop=4
set expandtab
set softtabstop=4
set backspace=eol,start,indent
 
" 搜索
set hlsearch
set incsearch
set ignorecase
set smartcase
```

### keymaps

```
" 优化jk设置
nnoremap j gj
nnoremap k gk
vnoremap j gj
vnoremap k gk
 
" alt+hjkl移动行
" mini.move
nnoremap <M-j> :m +1<CR>==
vnoremap <M-j> :m '>+1<CR>gv=gv
nnoremap <M-k> :m -2<CR>==
vnoremap <M-k> :m '<-2<CR>gv=gv
nnoremap <M-h> <<
vnoremap <M-h> <gv
nnoremap <M-l> >>
vnoremap <M-l> >gv
 
" H, L jump to line home / end
nnoremap H ^
nnoremap L $
vnoremap H ^
vnoremap L $
onoremap H ^
onoremap L $
 
" 切换buffer
nnoremap <C-n> <cmd>bnext<CR>
nnoremap <C-p> <cmd>bprevious<CR>
 
" 简单的自动括号实现，给vim用的
if !has('nvim')
    inoremap ( ()<Left>
    inoremap <expr> ) getline(line('.'))[col('.')-1]==')' ? '<Right>' : ')'
 
    inoremap [ []<Left>
    inoremap <expr> ] getline(line('.'))[col('.')-1]==']' ? '<Right>' : ']'
 
    inoremap { {}<Left>
    inoremap <expr> } getline(line('.'))[col('.')-1]=='}' ? '<Right>' : '}'
 
    inoremap < <><Left>
    inoremap <expr> > getline(line('.'))[col('.')-1]=='>' ? '<Right>' : '>'
 
    " inoremap ' ''<Left>
    " inoremap <expr> ' getline(line('.'))[col('.')-1]=="'" ? '<Right>' : "'"
    " inoremap " ""<Left>
    " inoremap <expr> " getline(line('.'))[col('.')-1]=='"' ? '<Right>' : '"'
    " inoremap ` ``<Left>
    " inoremap <expr> ` getline(line('.'))[col('.')-1]=='`' ? '<Right>' : '`'
endif
```

## 选项配置

- 默认采用系统剪贴板，同时支持鼠标操控 Nvim
- Tab 和空格的换算
- UI 界面
- “智能”搜索

`~/.config/nvim/lua/options.lua`

```lua
-- Hint: use `:h <option>` to figure out the meaning if needed
vim.opt.clipboard = 'unnamedplus' -- use system clipboard
vim.opt.completeopt = { 'menu', 'menuone', 'noselect' }
vim.opt.mouse = 'a' -- allow the mouse to be used in Nvim

-- Tab
vim.opt.tabstop = 4 -- number of visual spaces per TAB
vim.opt.softtabstop = 4 -- number of spacesin tab when editing
vim.opt.shiftwidth = 4 -- insert 4 spaces on a tab
vim.opt.expandtab = true -- tabs are spaces, mainly because of python

-- UI config
vim.opt.number = true -- show absolute number
vim.opt.relativenumber = true -- add numbers to each line on the left side
vim.opt.cursorline = true -- highlight cursor line underneath the cursor horizontally
vim.opt.splitbelow = true -- open new vertical split bottom
vim.opt.splitright = true -- open new horizontal splits right
-- vim.opt.termguicolors = true        -- enabl 24-bit RGB color in the TUI
vim.opt.showmode = false -- we are experienced, wo don't need the "-- INSERT --" mode hint

-- Searching
vim.opt.incsearch = true -- search as characters are entered
vim.opt.hlsearch = false -- do not highlight matches
vim.opt.ignorecase = true -- ignore case in searches by default
vim.opt.smartcase = true -- but make it case sensitive if an uppercase is entered
```

在init.lua中导入`require('options')`。

## 按键配置

- 用 <C-h/j/k/l> 快速在多窗口之间移动光标
- 用 Ctrl + 方向键进行窗口大小的调整
- 选择模式下可以一直用 Tab 或者 Shift-Tab 改变缩进

`~/.config/nvim/lua/keymaps.lua`

```
-- define common options
local opts = {
    noremap = true,      -- non-recursive
    silent = true,       -- do not show message
}

-----------------
-- Normal mode --
-----------------

-- Hint: see `:h vim.map.set()`
-- Better window navigation
vim.keymap.set('n', '<C-h>', '<C-w>h', opts)
vim.keymap.set('n', '<C-j>', '<C-w>j', opts)
vim.keymap.set('n', '<C-k>', '<C-w>k', opts)
vim.keymap.set('n', '<C-l>', '<C-w>l', opts)

-- Resize with arrows
-- delta: 2 lines
vim.keymap.set('n', '<C-Up>', ':resize -2<CR>', opts)
vim.keymap.set('n', '<C-Down>', ':resize +2<CR>', opts)
vim.keymap.set('n', '<C-Left>', ':vertical resize -2<CR>', opts)
vim.keymap.set('n', '<C-Right>', ':vertical resize +2<CR>', opts)

-----------------
-- Visual mode --
-----------------

-- Hint: start visual mode with the same area as the previous area and the same mode
vim.keymap.set('v', '<', '<gv', opts)
vim.keymap.set('v', '>', '>gv', opts)
```

在init.lua中导入`require('keymaps')`。

## 插件管理器

我使用`lazy.nvim`作为nvim的插件管理器。

### 初始化lazy.nvim

`~/.config/nvim/lua/lazynvim-init.lua`

```lua
-- 1. 准备lazy.nvim模块（存在性检测）
-- stdpath("data")
-- macOS/Linux: ~/.local/share/nvim
-- Windows: ~/AppData/Local/nvim-data
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
	vim.fn.system({
		"git",
		"clone",
		"--filter=blob:none",
		"https://github.com/folke/lazy.nvim.git",
		"--branch=stable", -- latest stable release
		lazypath,
	})
end
-- 
-- 2. 将 lazypath 设置为运行时路径
-- rtp（runtime path）
-- nvim进行路径搜索的时候，除已有的路径，还会从prepend的路径中查找
-- 否则，下面 require("lazy") 是找不到的
vim.opt.rtp:prepend(lazypath)

-- 3. 加载lazy.nvim模块
require("lazy").setup({})
```

在init.lua中导入`require('keymaps')`。

上述配置完毕以后，让我们首次启动nvim，第一次启动的时候，由于会从远端下载lazy.nvim模块，所以会有一定的延迟。然后，我们就会进入正常的nvim界面。然后命令模式下输入指令`:Lazy`后，我们会看到nvim的界面弹出一个对话框，展示lazy的状态。

### 安装插件

####  方式1:直接配置

只需要在上面的lazynvim-init.lua的中require("lazy").setup({})添加插件安装代码即可：

```lua
local nvim_tree_plugin = {
    "nvim-tree/nvim-tree.lua",
    version = "*",
    dependencies = {"nvim-tree/nvim-web-devicons"},
    config = function()
        require("nvim-tree").setup {}
    end
}
local lualine_plugin = {
    'nvim-lualine/lualine.nvim',
    config = function()
        require('lualine').setup()
    end
}
require("lazy").setup({nvim_tree_plugin, lualine_plugin})
```

上述方式下，我们首先定义了两个插件配置的table；然后，在setup中第一个参数table中，逐个添加插件。这样lazy.nvim就能帮我们将插件进行下载、安装。

#### 方式2:plugins目录统一编排

上述方式1固然简单，但每一次想要添加一个插件就需要在lazynvim-init.lua中添加插件代码；另外，大量的插件配置势必造成lazynvim-init.lua愈发臃肿。好在lazy.nvim还支持我们以更加优雅的方式编排插件：使用plugins目录统一编排插件。具体做法为：

第一步：lazynvim-init.lua中的setup参数变为setup("plugins")，同时移除掉有关具体插件安装配置的代码；

第二步：在lazynvim-init.lua所在目录下创建一个名为"plugins"的目录；

第三步：在plugins目录中创建插件配置模块lua脚本。在这一步中，我们分别创建两个lua脚本来分别作为两个插件的配置模块：

`plugin-lualine.lua`

```lua
return {
    {
        'nvim-lualine/lualine.nvim',
        config = function()
            require('lualine').setup()
        end
    }
}
```

`plugin-nvim-tree.lua`

```lua
return {
    {
        "nvim-tree/nvim-tree.lua",
        version = "*",
        dependencies = {"nvim-tree/nvim-web-devicons"},
        config = function()
            require("nvim-tree").setup {}
        end
    }
}
```

这里有两个注意点：1）文件名可以随意；2）每一个脚本模块都将返回一个table，且table的每一项都是一个插件配置（这里每个文件只有一项插件配置），lazy会把这些table合并为一个插件配置的table进行加载

要添加其他模块,只需要仿照这两个模块的方式,在`plugins`文件夹下建立更多文件即可.

### 插件推荐
