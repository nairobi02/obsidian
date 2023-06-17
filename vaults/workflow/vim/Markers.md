# Markers


A **mark** allows you to record your current position so you can return to it later. There is no visible indication of where marks are set.

Each file has a set of marks identified by lowercase letters (a-z). In addition there is a global set of marks identified by uppercase letters (A-Z) that identify a position within a particular file. For example, you may be editing ten files. Each file could have mark **a**, but only one file can have mark **A**.

Because of their limitations, uppercase marks may at first glance seem less versatile than their lowercase counterpart, but this feature allows them to be used as a quick sort of "file bookmark." For example, open your .[vimrc](https://vim.fandom.com/wiki/Vimrc "Vimrc"), press `mV`, and close Vim. The next time you want to edit your .vimrc, just press `'V` to open it. This assumes that you have kept the default ['viminfo'](http://vimdoc.sourceforge.net/cgi-bin/help?tag=%27viminfo%27) behavior, so that uppercase marks are all remembered in the [viminfo-file](http://vimdoc.sourceforge.net/cgi-bin/help?tag=viminfo-file) between Vim sessions.

## Setting a `mark`
To set a mark, type `m` followed by a letter. For example, `ma` sets mark **a** at the current position (line and column). If you set mark **a**, any mark in the current file that was previously identified as **a** is removed. If you set mark **A**, any previous mark **A** (in any file) is removed.

## Using a mark
To jump to a mark enter an apostrophe (`'`) or backtick (`` ` ``) followed by a letter. Using an apostrophe jumps to the beginning of the line holding the mark, while a backtick jumps to the line and column of the mark.

Using a lowercase letter (for example `` `a ``) will only work if that mark exists in the current buffer. Using an uppercase letter (for example `` `A ``) will jump to the file and the position holding the mark (you do not need to open the file prior to jumping to the mark).

|Command|Description|
|---|---|
|`ma`|set mark **a** at current cursor location|
|`'a`|jump to line of mark **a** (first non-blank character in line)|
|`` `a ``|jump to position (line and column) of mark **a**|
|`d'a`|delete from current line to line of mark **a**|
|``d`a``|delete from current cursor position to position of mark **a**|
|`c'a`|change text from current line to line of mark **a**|
|``y`a``|yank text to unnamed buffer from cursor to position of mark **a**|
|`:marks`|list all the current marks|
|`:marks aB`|list marks **a**, **B**|

