---
layout: post
title:  "Vim Nuggets: Part 1"
date:   2018-12-02 22:22:32 +0530
permalink: /vim-nuggets-part-1/
---
I work in a Linux environment most of the time, so vim is my primary text editor. Here are some tips that I use everyday to make my life easier. I will assume that people reading this have basic knowledge of vim including basic movements like `h`, `j`, `k`, `l`, motions like `0`, `$`, `gg`, `G`, `w`, `b`, `e` etc operators like `d`(elete), `y`(ank), `c`(hange) etc.

### Use `.` to repeat the last command

The `.` is used to repeat the last change. Keep in mind that this does not repeat motion commands.

[Sample gif for dot command](/assets/uploads/2018/12/2018-02-11-dot-command.gif)

### Use the `*` command to find the word under cursor

The `*` command is used to search for all occurrences of a word in a given file. This is more useful than using `/` as `/` highlights all occurrences of a word even if it is part of some other word, while * highlights all standalone occurrences of the word under it.

[Sample gif for star command](/assets/uploads/2018/12/2018-02-11-star-command.gif)

### Use `aw`

This is one of the most useful ones. Many times you will be in the middle of the word, and would like to operate on it(y, d, c etc), what you usually do is go to the start of the word using `b` and perform the operation using operator + `w`. However instead of going to the start, you can achieve this using the `aw` motion. Remember it as 'a word'. Combine this with the operator to perform the operation on the word under the cursor. `iw` is similar to aw, except it does not include the space after the word like `aw` does. `iw` and `aw` are known as text objects.

[Sample gif for aw](/assets/uploads/2018/12/2018-02-11-aw.gif)

### Use `o` to toggle cursor in visual mode

When in visual mode, when you are selecting text on one side, you may want to include or exclude text on the other side. While in visual mode, hitting `o`, will toggle the cursor to the other side of the visual selection.

[Sample gif for toggle visual](/assets/uploads/2018/12/2018-02-11-toggle-visual.gif)

### Use `[{` to go to the beginning of the code block

Many times when I am in the middle of the code, and will like to go to it's beginning, insted of scrolling, use `[{` to go the beginning of the code block. You can also combine `[` with other constructs like `(` to have the same functionality. You can also use `]` with `)`, `}` to go to the end of the code block.

[Sample gif for code block](/assets/uploads/2018/12/2018-02-11-code-block.gif)

### Use macros

You can think of macros as a series of keystrokes, that can be executed using another predefined keystroke. You can record macros, by using q followed by the name of a regsiter (uppercase and lowercase letters, numerals), followed by the key combination, and at the end using q again to stop recording the macro. You can execute the macro using @, followed by the name of the register in which the macro was recorded.

For example, if you want to append foo to a set of lines which are not contiguous, you can record a macro for this. You can follow the follow the steps below to record the macro in the p register. Assume vim is in normal mode

*   `qp`, start recording the macro and save it to p register
*   `Shift+I`, prepend to the line
*   `foo`, type the text foo
*   `Esc`, go to normal mode
*   `0`, go to beginning of the line
*   `q`, stop recording the macro

Now the macro is recorded and you can go to every desired line, and use `@p` to execute the macro. Moreover the previously executed macro can be executed again using `@@`.

### Use system clipboard

This is a personal one. I can frequently copy to and paste from vim to system clipboard and vice versa, but vim copy and paste commands can only do so to vim buffers. You can use system clipboard instead of vim buffer by using `set clipboard=unnamedplus` to copy from and paste to clipboard using `y` and `p` in vim.

[Sample gif for clipboard](/assets/uploads/2018/12/2018-02-11-clipboard.gif)

### Combine operators and motions

Combine operators like `d`, `c`, `y`, `v` etc with motions like `0`, `$`, `f`, `t`, `gg`, `G`, `aw` etc. I will not elaborate on this, but will leave it to the reader to try this on it's own. Doing this will help you discover the true power of vim.
