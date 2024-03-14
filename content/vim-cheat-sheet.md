+++
title = "Vim cheat sheet"
date = "2022-09-28"

[taxonomies]
tags = ["Vim"]
+++

This is a cheat sheet for the text editor Vim. If you want to learn Vim, I strongly recommend you to start out with doing `vimtutor`.

<!-- more -->
A short note: Vim works by combining operators with (optional numbers and) motions. 
To abort what you're doing (leaving Insert mode or Visual mode, aborting an operation, etc.) press `<ESC>`; `Ctrl+c` usually works too.

# Motions:
  * `w`  moves to first character of next word;
  * `b`  moves to first character of current or previous word;
  * `e`  moves to last character of current or next word;
  * `0`  moves to the first character of line;
  * `^`  moves to first non-blank character of line;
  * `$`  moves to last character of line;
  * `t{char}`  moves forward on the line to the position before the character entered after `t`;
  * `T{char}`  moves backward on the line to the position before the character entered after `T`;
  * `f{char}`  moves forward on the line to the position of the character entered after `f`;
  * `F{char}`  moves backward on the line to the position of the character entered after `F`;
  * These motions can be appended to a number to repeat the movement;

# Navigation:
  * `gg`  goes to first line of file;
  * `G`  goes to last line of file;
  * `Ctrl+g`  displays current line position of cursor and total number of lines of file;
  * `{line}G` or `{line}gg`  where `{line}` is the number of the line where you want to place the cursor;

# Insert mode:
  * `i`  enters insert mode at cursor location;
  * `a`  enters insert mode one step AFTER cursor position (`ea` appends to current word);
  * `I`  prepends to line, i.e. enters insert mode at the first non-blank character of line;
  * `A`  appends to line, i.e. enters insert mode AFTER the end of the line; 
  * `o`  opens the line below the cursor and enters Insert mode;
  * `O`  opens the line above the cursor and enters Insert mode;
  * `r`  replaces the symbol the cursor is placed on with the symbol entered after `r`.
  * `R`  replaces multiple symbols with the ones entered after  `R`, starting where the cursor was placed and stopping when pressing `<ESC>`.
  * `Ctrl+w`  deletes a whole word before the cursor while in Insert mode;
  * `Ctrl+u`  deletes the line before the cursor while in Insert mode;

# Visual selection mode:
  * `v`  enter Visual mode, where you can mark text by using motions, which can be deleted, yanked (copied), 
      saved to a new file named `NAME` (by typing  `:w NAME`  after text has been marked). 
  * `V`  enters Visual Line mode with whole lines are being marked;
  * `Ctrl+v`  enters Visual Block mode where rectangular blocks are marked. End with `<ESC>` to apply the following operations:
   ** to insert text, use  `Shift+i`;
   ** to change text, use `c`;
   ** to delete text, use `d` or `x` (this will be applied right away followed by exiting Visual Block mode);

# Cutting, copying, pasting using Vim's clipboard:
  * `d`  followed by motion to cut text: `dd`  to cut a whole line;
  * `D` or `d$`  to cut rest of line;
  * `c`  followed by motion cuts the chosen text and enters Insert mode: `cc` cuts the whole line; 
  * `x`  to cut a single character;
  * `y`  followed by motion to copy (AKA 'yank') text: `yy`  to yank a whole line
  * `p`  pastes text from Vim's clipboard, inserting it after the cursor;
  * `P`  pastes text from Vim's clipboard, inserting it after the cursor;
  * The above operators can be used when text has been marked in Visual mode;

# Edit:
  * `u`  undo last operation;
  * `Ctrl+r`  to redo undone operation;
  * `>>`  indents line where cursor is placed, `>` indents text marked via Visual mode;
  * `<<`  dedents line where cursor is placed, `<` dedents text marked via Visual mode;
  
# Commands:
  * `:w`  to save;
  * `:w NAME`  to save to not pre-existing file named `NAME`, either saving the whole file or 
     only the text marked via visual mode;
  * `:q!`  to quit without saving changes;
  * `:wq`  to save and quit;
  * `:r NAME`  to insert content of file `NAME` in line below the cursor;
  * `:r !{external command}`  reads from command and puts it below the cursor position;

# Cutting, copying, pasting using system clipboard:
  * `Shift+Ctrl+v` or `"+p`  pastes text to the file from the system clipboard;
  * `Shift+Ctrl+x` or `"+x`  cuts text from the file to the system clipboard;
  * `Shift+Ctrl+c` or `"+c`  copies text from the file to the system clipboard;
  * Explanation: In the second alternative of the operations above, the system clipboard is interacted with by entering `"` (the `registers symbol`), 
    then `+` to access the clipboard register, followed by what we want to do with the clipboard;  

# Search, find, replace:
  * `:/`  followed by a search word followed by `<Enter>` to find the first instance of the word,
      followed by  `n`  to continue search forward or  `N`  to continue search backward;
  * `:?` works like  `/`  but starts to search backward right away after `<Enter>`;
  * `:s/old/new`  substitutes `new` for `old` at the first match found: append with  `/g`  to
      do it for the whole line;
  * `:%s/old/new/g`  replaces every occurence in the whole file;
  * `:%s/old/new/gc`  finds all occurances in the file and asks whether you want to replace them;
  * `:#,#s/old/new/g`  replaces `old` for `new` between lines `#,#`;
  * `%`  to find matching `)`, `]`, or `}` which cursor is placed on;
  * `:set ic`  to set search to ignore case (`:set ignorecase`  also works);
  * `:set noic`  to set search to not ignore case;
  * `:/SEARCHWORD\c`  to search for `SEARCHWORD` and ignoring cases in just that search;
  * `:set hls`  is to enable highlighting of matches in search (`:set hlsearch`  also works);
  * `:set nohls`  is to disable highlighting of matches in search;
  * TODO: What does  `:set is`  do? So called `insearch`;

# Execute external commands:
  * `:!{external command}`  followed by `<Enter>` runs any external shell command, such as `ls -a` or `dir`;
  
# Completion:
  * Starting to write a command, like  `:e`  followed by  `<TAB>`  autocompletes it in
    alphabetical order, such as the case in most shells and terminals. Pressing 
    `Ctrl+d`  displays all possible completions.

# Navigating directories
  * Running `vim .` in a terminal opens a directory listing that can be navigated with `j`, `k` and `<Enter>`. 
    To create a new file, press `%`; to create a new directory, press `d`. To return to this view from a file that has been entered from here, 
    use `:Ex`. External commands can be run by `:! {command}`.
