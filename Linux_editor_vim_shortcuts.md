
# vim 编辑器快捷备忘单

------

# 目录

------

1. [vim 学习概述](#vim-学习概述)
2. [Vim 的几种模式](#Vim-的几种模式)
3. [光标定位](#光标定位)
3. [操作符](#操作符)
4. [操作组合](#操作组合)
5. [撤消与重做](#撤消与重做)
6. [匹配搜索](#匹配搜索)
7. [匹配替换](#匹配替换)
8. [高级匹配替换](#高级匹配替换)
9. [综合命令](#综合命令)
10. [文档保存和退出](#文档保存和退出)
11. [文本输入补全](#文本输入补全)
12. [VIM 配置文件](#VIM-配置文件)
13. [常用设置](#常用设置)
14. [多文件编辑](#多文件编辑)
15. [窗口分割](#窗口分割)
16. [Tab page](#Tab-page)
17. [DIRECTORY EXPLORATION COMMANDS](#DIRECTORY-EXPLORATION-COMMANDS)
18. [标记](#标记)
19. [寄存器](#寄存器)
20. [Macro](#Macro)
21. [排版](#排版)
22. [基本排版](#基本排版)
23. [高级排版](#高级排版)
24. [映射](#映射)
25. [杂项](#杂项)


------

# vim 学习概述

VIM 软件包提供了一个指导练习的附件程序 vimtutor ，并且可以指定语言 vimtutor [language] （如 vimtutor zh）。此附件程序是理解以下备忘单的基础。支持的语种请查看目录 /usr/share/vim/vim80/tutor/tutor[.language]，或在 vimtutor 手册页中查看。

VIM 帮助命令首页下的其它手册内容在此备忘单内也有部分涵盖。官网帮助页面内容更全。

节选的内容可在较多环境下使用，最好根据自己学习过程的需求做相应的整理或增删。推荐使用版本管理工具 git。

VIM 官网：https://www.vim.org/

VIM 帮助页面：https://vimhelp.org/

------

## Vim 的几种模式

```
普通模式，是 Vim 默认模式，在进入其它模式后，按 <ESC> 键返回普通模式。

替换模式：

    - 普通模式按 r，替换光标处的单个字符，替换后自动返回到普通模式。
    - 普通模式按 R，在光标处进入替换模式。

插入模式：

    - 普通模式按 i/I/a/A，在光标位置/行首/下一个字符/行尾进入插入模式。
    - 普通模式按 o/O，在光标所在行行下/上进入插入模式。

可视模式：

    - 普通模式按 v，进入可视模式, 高亮选取文本；
    - 普通模式按 Shift + V，高亮按行选取；
    - 普通模式按 Ctrl + V，高亮按块选取。

底部命令模式，可在普通模式/可视模式下使用 : 、\ 或 ? 执行命令或临时配置 vim。
```

------

## 光标定位

移动光标在组合操作中常称为动作。这些动作在普通模式中执行。

```
h，j，k，l		/* 左、下、上、右移。

gj, gk			/* 自动换行的行间下，上移动。

[ home | 0 ] | ^ | g_ | [ end | $ ]	/* 移动到行首/首字符/尾字符/行尾；
g^ | gm | g$		/* 移动到行首/中/尾。

w | W			/* 右移一个单词/字符串位，光标停在首字符；
b | B			/* 左移一个单词/字符串位，光标停在首字符；
e | E			/* 右移一个单词/字符串位，光标停在尾字符；
ge | gE			/* 左移一个单词/字符串位，光标停在尾字符；

gg | G			/* 移动到首/末行；
Number[gg |G]		/* 移动到指定行；

/* 移动到当前行 char 字符位/前，如再输入分号（;），则移动到下一个匹配位。
{ f|tchar }
{ F|Tchar }		/* 同上，只是反向。

%			/* 小、方、花括号。

( | )			/* the beginning of previous/next sentence.
{ | }			/* the beginning of current/next paragraph.
[[ | ]]			/* the beginning of current/next section.

/* 在当前行移动光标至 Nth 字符/相对当前光标位置的 Nth 字符。
Number| | Number<space>

Number%			/* Nth percentage of file.
Number<enter>		/* Nth line from current line.

[ nG | ngg ]		/* 移动到第 n 行。
{ H | M | L }		/* 移动到屏幕页首/页中/页尾。

CTRL-F/B		/* Scroll down/up full page.
CTRL-D/U		/* Scroll down/up half page.
```

------

## 操作符

删除，改变，剪切，复制，粘贴:

```
x, c, d , y		/* 分别为删除、改变、剪切和复制操作符。

{ p | P }		/* 粘贴到所在行下/上。

{ yy | [ dd | D ] }	/* 复制/删除（剪切）所在行。
[n]dd			/* 删除（剪切）n 行。
```

------

## 操作组合

```
    次数 动作
    | ----- | ---> 可以组成作用域

     [操作次数] 操作符 作用域 （格式之间没有空格）

    可视模式步骤：
    可视模式 [操作次数] 作用域 操作符（格式之间没有空格）
       |     | --------------|
       v        选择可视区域   执行操作

注：改变、剪切和复制命令称作操作符，操作次数可在操作符之前或之后。
```

examples:

```
3j		/* 下移三行。
4x		/* 删除4个字符。

d3fs		/* 删除三个连续行内匹配 s。
3dfs		/* 同上。

Ctrl+V2jd	/* 可视模式行选取，下移两行选取，删除。

3a!<Esc>	/* 添加三个感叹号。

d3ap		/* 删除3个段落。
```

文本对象

```
aw | as | ap		/* 一个词/一句/一段。
a( | a[	| a{		/* 小括号块/中括号/花括号。
a< | at		/* 单个 html <aaa> / <aaa> </aaa> 。
a" | a' | a`	/* 由双/单/反引号阔起的部分。

%			/* 小、方、花括号。

| 词、句、段 |              括号           |      引号      |      html 标签       |
| ---------- | --------------------------- | -------------- | -------------------- |
| aw | iw    | a) = a( = ab | i) = i( =ib  | a"  | a'  | a` | a> = a<  |  i> = i<  |
| aW | iW    | a] = a[      | i] = i[      | i"  | i'  | i` | at       |  it       |
| as | is    | a} = a{ = aB | i} = {i = iB |                |                      |
| ap | ip    |              |              |                |
| a 整体     | i 不含括引号                | 有无 <>        | 有无 <aaa></aaa>     |

注：`:help text-objects` 查询有哪些文本对象。

注：可视模式下选取文本后，`:w filename` 保存所选内容到指定文件。Ctrl+V 要选取长短不一致的多行时，要使用 $。
```

------

## 撤消与重做

```
u		/* 撤销之前的一个改动，除了 u 本身的行为。
U			/* 撤销在一行中所作的所有改动。

:undolist		/* 撤销历史。
[count]u		/* 撤销 count 个改动。
:undo count		/* 撤销 count 个改变。

.			/* 重复上一次操作。

ctrl + R		/* 撤销之前的撤销。
:redo			/* 同上。

:earlier 9m		/* 回到 9 分钟前。
:later 18s		/* 前进 18 秒。
```

------

## 匹配搜索

```
/keyword		/* 向上查找 keyword， n 下一个； N 前一个。
?keyword		/* 向下查找 keyword， n 下一个； N 前一个。
	\<keyword	/* 仅匹配与 keyword 同样开头的单词。
	keyword\>	/* 仅匹配与 keyword 同样结尾的单词。
	\<keyword\>	/* 仅匹配与 keyword 完全一样的单词。

/* 非底部命令模式的快捷命令查找。
{ * | # }	/* 在任何单词下按 */# 号，则向下/上查找此单词。

注：.*[]^%/\?~$ 字符具有特殊含义，如要搜索此类字符必须在字符前加 \。
```

------

## 匹配替换

```
:[range]s[ubstitute]/{pattern}/{string}/[flags] [count]

:s/old/new		/* 在当前行替换第一个匹配。
:s/old/new/g		/* g 当前行替换所有匹配。
:#,#s/old/new/g		/* #, #s 行范围之内。
:%s/old/new/g		/* % 范围是整个文件。
:%s/old/new/ge		/* 对多个文件进行匹配，e 可以规避错误而中断，如无匹配。
:%s/old/new/gc		/* c 每次替换都提示确认。

             i		/* 忽略大小写。
             I		/* 不忽略大小写。

:'<,'>s/old/new/g	/* '<,'> 范围是选择的可视块。

:.,$			/* 范围是当前行到行尾。
:.+3,$-5		/* 范围是当前行+3到行尾-5。
"'b,'t			/* 范围是标记 b 到 t。

:%s/^/#/g		/* 在每一行的行首插入 #，^ 表示行首。
:%s/$/./g		/* 在每一行的行尾插入 .，$ 表示行尾。
:%s/\s\+$//g		/* 移除行尾空格。
```

------

## 高级匹配替换

```
/* Switch the first column to the second column.

        Doe, John

:%s/\([^,]*\), \(.*\)/\2 \1/

 \(     \)                The first part between \( \) matches "Last"
   [^,]                       match anything but a comma
       *                      any number of times
          ,               matches ", " literally
            \(  \)        The second part between \( \) matches "First"
              .               any character
               *              any number of times
```

------

## 综合命令

```
:help		/* 帮助手册页，文档最前部描述了如何快速使用帮助。

ctrl + G		/* 显示当前编辑状态，包含文件名和光标位置。

ga			/* View ASCII value of a char under the cursor.

:!{program}		/* 执行外部命令。
:m,nw !v{program}	/* 以文件 m 至 n 行之间的内容作为命令的参数执行命令。
:. !bash	/* 当前行作为 bash 的参数执行，读取输出，当前行会被删除。

:m,nd			/* 剪切 m 到 n 行的内容。

:r filename		/* 在光标处插入指定文件的内容。
:r !{program}		/* 在光标处插入命令的输出。

:e[dit] filename	/* 载入指定文件。
:e[dit]!		/* 重载文件，文件的最后保存状态。
:e[dit]! filename	/* 强制放弃当前的修改，并载入指定的文件。

:e[dit] /path		/* explore directories to find files.

:pwd			/* 显示 vim 工作目录。
:cd path		/* 改变 vim 工作目录。

:f[ile]			/* 显示当前编辑状态。等同 ctrl + G。
:f[ile] filename	/* 把当前文件名设为指定文件名（仅在缓存中）。
:files			/* 显示所有打开的文件，包含缓存中的。

:history		/* 显示命令历史。
q:		/* Open a windows at the bottom to display command history.

:Man bash-command		/* IN a new windows dispaly man page.
```

examples:

```
/* It's so convinient to read the output of the external command.
:r !ls -al /

total 24
dr-xr-xr-x.  17 root root  224 Oct 24 08:41 .
dr-xr-xr-x.  17 root root  224 Oct 24 08:41 ..
lrwxrwxrwx.   1 root root    7 Apr 23  2020 bin -> usr/bin
...
drwxr-xr-x.  13 root root  155 Oct 31 10:28 usr
drwxr-xr-x.  21 root root 4096 Oct 24 08:52 var
```

注: Tab 命令补全在此处也适用。

------

## 文档保存和退出

```
{ :w[rite] | :w[rite] filename }	/* 保存/按指定文件名保存。
{ :q[uit] | :q[uit]! }		/* 退出/退出并放弃修改。
{ :wq | :wq! }		/* 保存并退出/强行退出。
```

------

## 文本输入补全

```
CTRL-X CTRL-N/P		/* Word completion - forward/backward.
CTRL-X CTRL-L		/* Line completion. Matching a line in the page.
CTRL-X CTRL-F		/* Filename completion.
```

------

## VIM 配置文件

```
/etc/vimrc		/* VIM 的系统配置文件。
~/.vimrc		/* VIM 的个人配置文件。

:scriptnames		/* 列表所有的 VIM 配置文件，插件。
```

------

## 常用设置

```
/* 列表选项。
:se[t]			/* 显示所有与默认设置不同的设置。
:se[t] all		/* 显示所有选项，但不包括终端选项。
:se[t] termcap	/* 显示所有终端选项，但不显示 GUI 中的 key codes。
:se[t] {option}?	/* 显示指定选项的值。

/* 设置选项为默认值。
:se[t] {option}&	/* 重置指定选项为默认值。
:se[t] {option}&vi	/* 重置指定选项为 vi 默认值。
:se[t] {option}&vim	/* 重置指定选项为 vim 默认值。
:se[t] all&		/* 重置所有选项为默认值。

/* 关闭或反转选项。
:se[t] no{option}	/* 关闭指定选项。
:se[t] {option}! | inv{option}		/* 切换或反转指定选项值。

/* 开启选项。
:se[t] {option}	on | off

		/* 编辑屏幕三个底部状态显示。
	[smd | showmod]	/* 开启模式显示。例如插入模式等。
	[sc | showcmd]	/* 开启显示输入的命令。
	ru[ler]		/* 开启在右下角显示光标位置。

	wrap		/* 显示时自动换行。

	nu[mber]	/* 开启显示行号。
	numberwidth=	/* 行号字符宽度，默认 4 字符。

	list		/* 开启显示非打印字符。
	listchars=	/* 设置 list 显示那些字符。

	[ic | ignorecase]	/* 开启忽略大小写。
	[hls | hlsearch]	/* 开启高亮匹配所有搜索。
	[is | incsearch]	/* 开启搜索输入同步匹配高亮。
	[ws | wrapscan]		/* 开启文件滚动搜索。

	[ai | autoindent]	/* 开启自动缩进。以首行缩进作为其下行缩进方式。
	[si | smartindent]	/* 开启智能缩进。必须开启 autoindent。

	autochdir		/* 根据所编辑文件的位置自动切换 vim 工作目录。

:set softtabstop=4		/* 用以指定数量的空格代替 TAB 制表符。

/* 设置文本宽度（默认没有限度 =0）。设置了值，达行限后不分割单词自动加换行。
:set textwidth=0

:set linebreak			/* 折行添加换行符。

/* 开启关闭文件类型侦测。此设置可用与设置语法高亮。
:filet[ype]
:filetype plugin		/* 开启关闭文件类型插件。
:filetype indent		/* 开启关闭缩进。
:filetype plugin indent		/* 开启关闭插件文件缩进。

:syn[tax]			/* 开启/关闭语法高亮。

/* Set file encoding. Modify it and save your desired encoding files.
:set fileformat=
		unix		/* <LF>. The default is `unix,dos`.
		dos		/* <CR><LF>.
		mac		/* <CR>.

'backup' 'writebackup'	action	~
   off	     off	no backup made
   off	     on		backup current file, deleted afterwards (default)
   on	     off	delete old backup, backup current file
   on	     on		delete old backup, backup current file
```

------

## 多文件编辑

```
vim file1 file2 filen		/* 编辑多个文件，但编辑器只显示一个文件。

:<command>
:next			/* 切换到下一个文件。
:previous		/* 切换到前一个文件。
:first			/* 切换到第一个文件。
:last			/* 切换到最后一个文件。

:w<command>
	w		/* 切换并保存修改。

:[count]<command>[!]
 [count]		/* 前后切换移动偏移数。
                 [!]	/* 切换并放弃修改。

:args<!> newfile1 newfile2 newfilen

     /* 无参数，将显示多文件编辑状态。文件列表及编辑位置 [file1]。
		[file1] file2 filen

	newfile1 newfile2 newfilen	/* 编辑新的文件列表。
      !			/* 放弃修改。

:set autowrite		/* 开启在文件间切换时的自动保存。
:set noautowrite	/* 关闭此功能。

注：Ctrl+G 也可显示文件状态。
```

------

## 窗口分割

```
/* Open files and split windows horizontally/vertically.
vim -o/O [file1, fileN,...]

:sp[lit] filename	/* 水平分割窗口，并在新窗口打开指定文件。
:vs[plit] filename	/* 垂直分割窗口，并在新窗口打开指定文件。
:new		/* split vertically and start editing an empty file.
:vnew		/* split horizontally and start editing an empty file.

/* set/increase/decrease current window height to/by N.
:res[ize] N | +N | -N
/* set/increase/decrease current window width to/by N.
:vertical res[ize] N | +N | -N

Ctrl + w	/* 窗口操作。

	w		/* 切换窗口。
	s		/* 水平分割窗口。
	v		/* 垂直分割窗口。
	q		/* 退出光标所在窗口。
	x		/* 与下一个窗口交换位置。
	=		/* 使所有窗口的高和宽相等。

:close			/* 关闭一个窗口。
:only			/* 除了当前窗口，关闭所有窗口。

:qall			/* 退出所有窗口。
:qall!			/* 退出并取消所有未保存修改。
:wall			/* 保存所有窗口的改变。
:wqall			/* 保存并退出所有窗口。
```

注：文本版本比较工具 vimdiff, 当设置了 backup，就会生成 file~ 文件。 vimdiff file~ file 可以直观的查看文件差异，也是 git diff 默认图形比较工具。

------

## Tab page

A tab page holds one or more windows, just like a web browser.

```
/* Open files in a tab page.
vim -p [file1, fileN,...]

/* open a new page/to edit a file, count specify where to open a tab.
:[count]tabe[dit] | tabnew {file}

/* Tab index(count):
0	/* before the first tab.
1	/* the first tab.
.	/* after the current tab.
-	/* before the current tab.
+	/* after the next tab.
$	/* after the last tab.

:tabonly		/* close all tab pages except the current one.

:{count}tabn[ext]	/* go to next tab page {count}.
:{count}tabp[revious]	/* go to previous tab page {count}.
:{count}tabm[ove]	/* move tab page to {count}.

:tab split		/* opens current buffer in new tab page.

/* Execute {cmd} and when it opens a new window open a new tab page instead.
:[count]tab {cmd}

/* A example, opens tab page with help for "tabedit" after the last one.
:$tab help tabedit

/* default " ", "a" for GUI, MS-DOS and Win32, set to "a" in defaults.vim.
:set mouse=a		/* file tabs now can be selected by clicking.
```

------

## DIRECTORY EXPLORATION COMMANDS

```
:[N]Ex[plore]  [dir]...		/* Explore directory of current file.
:[N]Hex[plore] [dir]...		/* Horizontal Split & Explore.
:[N]Lex[plore] [dir]...		/* Left Explorer Toggle.
:[N]Sex[plore] [dir]...		/* Split&Explore current file's directory.
:[N]Vex[plore] [dir]...		/* Vertical Split & Explore.
:Tex[plore]    [dir]...		/* Tab & Explore.

<enter>		/* Open the file/Go to the directory under the cursor.
D		/* Delete the file under the cursor.
R		/* Rename the file under the cursor.
X		/* Execute the file under the cursor.
```

------

## 标记

在 VIM 中，除了短距离的移动，如上下左右，按词，fchar 等移动之外，都属于跳跃。每一次跳跃都会按序记录下来，并且可以指定跳转到指定的记录点，每个记录点也可以称为标记。

无名标记
```
/* 列表记录的跳跃记录。在列表条目中首字符若为 <，则表示当前所在跳跃。
:jumps

ctrl + O		/* 返回到跳跃记录中的上一个跳跃点。
{ctrl + I | TAB}	/* ctrl + O 的反向操作。

``			/* 前一个跳跃点，但仅在两个跳跃点之间来回跳跃。

/* jump to location #Number ablove/below location #0 in jump list.
NumberCTRL-O | NumberCTRL-I
```

`m` 为标记指令，共可指定 26 标记，字母 a - z。用 `"` 调用标记。

具名标记
```
:marks			/* 列表具名标记。

m[a-z]			/* 生成具名标记。
`[a-z]			/* 跳跃到指定具名标记。
'[a-z]		/* The beginning of the specific marked line/area.

/* 特殊标记列表。
     '			/* 跳跃之前光标所在位置。
     "			/* 文件编辑的最后所在位置。
     [			/* 最后一次修改的起始位置。
     ]			/* 最后一次修改的结束位置。

     .		/* The position of where the last change was made.

     <		/* The first line of previously selected visual area.
     >		/* The last line of previously selected visual area.
```

------

## 寄存器

寄存器可以用来保存操作的文本或宏命令，以方便在其它处应用。除只读寄存器外，寄存器内容可以附加或修改。

```
:reg[ister]	/* 列表所有寄存器，及其内容。

:let @{register-specified} = "{content-desired}"	/* 修改寄存器内容。

"{register}{action}	/* 寄存器调用与应用格式。
```

/* 具名寄存器。

```
a-z		/* 在剪切、复制或粘贴前都可以使用 a-z 有名寄存器。
A-Z		/* 用大写字母 A-Z 表示在相应的寄存器中追加内容。
```

examples:

```
"ay$		/* 调用寄存器 a，复制内容。复制的内容被复制到寄存器 a。
"ap		/* 调用寄存器 a，应用内容粘贴。
"Ayy		/* 附加到寄存器 a，复制整行。整行内容被附加到寄存器 a。

:let @a = "Never too old to learn!" 		/* 修改寄存器 a 的内容。

/* 可视模式选取 4 行，调用寄存器 b，删除。删除的内容被复制到寄存器 b。
Shift+V3j"bd
```

/* 特殊寄存器

```
/* 无名寄存器：
"		/* 每次使用 c，d，y，p 作用的内容。

/* 数字寄存器:
0		/* 保存最新复制的内容。
1-9		/* c，d，s，y，p，x 作用的文本内容记录。

/* 只读寄存器：
%		/* 当前文件名。
.		/* 上一次插入的文本。
:		/* 最近执行的命令。

/* 其它寄存器
-		/* 最近小于一行的删除。
/		/* 最近的搜索匹配。
=		/* 最近的正则表达式寄存器。
_		/* 黑洞寄存器（在命令前使用此寄存器，则在寄存器中不被记录）。

#		/* alternate buffer register。

*		/* 系统剪贴版（X11 primary）。
+		/* 系统剪贴板（X11）。
~	/* The "~ register is only used when dropping plain text onto Vim.
```

------

## Macro

A macro is like a sequence of recording complex commands and stored in a specified register. Then you can replay it.

```
q{register}<a sequence of commands>q	/* Record a series of commands.
[count]@{register}	/* Play the macro. count means executing times.

/* Edit the macro in a register.
"{register}p		/* Put the text from n register and edit it.
"{register}y$		/* Yank the modified commands into the n register.
```

examples:

```
qao#includes ""<Esc>q	/* Record a macro in register a.
qa			/* Begin the macro named a.
  o#includes ""		/* new line, insert text `#includes ""`.
               <Esc>	/* quit insert mode.
                    q	/* quit the recording.

3@a			/* Play the macro in 3 times.

#includes ""
#includes ""
#includes ""
#includes ""


/* Edit the macro.

"ap		/* Output the macro in a register.
"ayy		/* Yank the whole modified line into a register.

`o#includes ""`		/* the origin content of this macro.
`o#include ""`		/* the modified content.

/* Create a macro which deletes all of spaces at the end of lines in a file.
qb
   :%s/\s\+$//ge<Enter>
                       :w<Enter>
                                q

/* Output the macro and check it out.
"bp
`:%s/\s\+$//ge:w`

/* Make markdown tab of contents.
`0f[yi[$a(#pa)`
```

------

## 排版

请先查阅以下链接文档介绍以方便理解以下快捷方式：

https://vimhelp.org/usr_25.txt.html#usr_25.txt

### 基本排版

文本缩进。
```
{<< | >>}	/* 左/右缩进。
```

对齐方式。
```
:{range}ce(nter) [width]	/* 所选行文字居中。
:{range}ri(ght) [width]		/* 所选行文字右对齐。
:{range}le(ft) [margin]		/* 所选行文字左对齐。

{range} 为 m,n ，也就是起始行至结束行的文本范围。[width] 为宽度，textwidth 为 0,则取 80，[margin] 是以空格为单位的边距。

注：文本对齐会影响文本两端原有的空格和制表符。
```


行拼接。
```
J	/* 行拼接，去掉原行间空格再添加一个空格，默认拼接下行。
gJ		/* 直接去掉行间换行符。
```

重排行，即对过长的文字进行断行。

```
gq
  q		/* 重排所在行。

可视模式 [所选区域] gq （字段间没有空格）
```

------

## 高级排版

对列应用相同行内容。

/* Inserting.

```
origin content            the result

include one             | include main.one
include two             | include main.two
include three           | include main.three

/* Insert columns.
fo			/* Move to o of one of the first line.
  Ctrl+V		/* Virtual mode.
        2j		/* Select 3 lines.
          I		/* Insert mode.
           main.	/* Text content.
                <Esc>	/* The end of comands.
```

/* Appending.

```
origin content             the result

include main.one        |  include main.one  |
include main.two        |  include main.two  |
include main.three      |  include main.three  |

f$
  Ctrl+V
        $
         2j
           A
            <Space><Space>|
                           <Esc>
```

/* Replacing and others.

```
^fe2j
     Ctrl+V
           c
            #includes<Esc>

/* Delete columns to the end of line, then insert new text.

           C
            new text<Esc>

/* Other commands that change the characters in the block:

        ~       swap case       (a -> A and A -> a)
        U       make uppercase  (a -> A and A -> A)
        u       make lowercase  (a -> a and A -> a)
```

拷贝和复制文本块

```
title  column 1  column 2  column 3  |    column 3
a        1          2        3       |      3
b        2          2        3       |      3
c

/* Copy the column 3 block before the column 1.
:set virtualedit=all

/column 3		/* Select the column 3 block and copy it.
   Ctrl+V
         3j9l
             y

^fe<space><space>p	/* Go to the position and paste the block.

:set virtualedit=	/* Restore the virtualedit setting.
```
**Note**: `all` means that the cursor can be positioned where there is no
acutal character.


TURNING A PARAGRAPH INTO ONE LINE

```
If you want to import text into a program like MS-Word, each paragraph should
be a single line.  If your paragraphs are currently separated with empty
lines, this is how you turn each paragraph into a single line:

        :g/./,/^$/join

That looks complicated.  Let's break it up in pieces:

        :g/./           A ":global" command that finds all lines that contain
                        at least one character.
             ,/^$/      A range, starting from the current line (the non-empty
                        line) until an empty line.
                  join  The ":join" command joins the range of lines together
                        into one line.

Note that this doesn't work when the separating line is blank but not empty;
when it contains spaces and/or tabs.  This command does work with blank lines:

        :g/\S/,/^\s*$/join

This still requires a blank or empty line at the end of the file for the last
paragraph to be joined.
```

------

## 映射

绑定一组命令到单个按键。

```
/* 将后面的命令组合映射到功能键 <F5>。命令组合：插入 { 字符，<Esc> 退出插入模式，e 移动到行尾，a 追加 } 字符，退出插入模式。
:map <F5> i{<Esc>ea}<Esc>
```

------

## 杂项

```
:runtime! ftplugin/man.vim
```

------
