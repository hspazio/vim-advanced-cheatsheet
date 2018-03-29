# Vim Advanced Cheatsheet

Personally curated list of advanced Vim tricks inspired by http://vimcasts.org

Table of contents
1. [Search and Replace](#search-and-replace)
2. [Registers](#registers)
3. [Navigation](#navigation)
4. [NORMAL mode](#normal-mode)
5. [INSERT mode](#insert-mode)
6. [VISUAL mode](#visual-mode)
7. [Spelling](#spelling)
8. [History](#history)
9. [Commands](#commands)

## Search and Replace

### Search in current buffer
`:set hlsearch` `:set nohlsearch` to toggle search highlighting

* Use `*` to search for the word under the cursor - like `/word` but without typing
* Use `#` to search for the word under the cursor in reverse order
* Use `/word` otherwise

### Replace in current buffer

* `:%s/\s\+$//e` - strip trailing white spaces 
  - replace in the whole file `:%s` 
  - the spaces till the end of the line `/\s\+$/`
  - with nothing `//`
  - and suppress any errors if no matches found `e`
* `:s/\%V#/ /g` - replace `#` character with whitespace only within the VISUAL selection `\%V`

### Search in multiple files
__Vimgrep__ is a very powerful search tool that allows searching not only in the current file but in any provided targets. 

Results will appear in the __quickfix__.

`:vimgrep/{pattern}/{target}` - search for pattern where {target} can be:

  - `%` current file
  - `*.rb` wildcards or file list
  - `` `find . -type f` `` backtick expression
  - `##` special symbol that represents the __args__ (set by `:args <expr>`)

Also `:vimgrep` can be shorted as `:vim` 

### Replace in multiple files
 * `:vimgrep /{patter}/ {target}` search for a pattern
 * `:cdo s/{pattern}/{replacement}/ge` replace for each file in the quickfix - `e` will ignore errors if not matches found
 * `:cdo w` or `:cdo update` to write the changes

### Multiple repeats
* `:g/monkey/d` - delete all lines containing "monkey"
* `:g/^monkey/d` - delete all lines starting with "monkey"
* `:v/monkey/d` - delete all lines that do not match "monkey" (like grep -v)
* `:g/^$/d` - delete blank lines

All commands above consist in the composition of 3 subcommands:

1. grep the document globally `:g`
2. to find blank lines `/^$/`
3. and delete them `d`

## Registers
* `"ayy` yanks current line into register "a"
* `"ap` pastes from register "a"
* `"Ayy` appends yanked line to register "a"
* `:reg a` inspects register "a"
* `qaq` resets register "a"
* `:g/monkey/yank A` yanks all lines containing "monkey" and append them into register "a"

## Navigation

* `Ctrl-w g f` while being with the cursor on a word (e.g. "file.txt") opens the file with that name on a new window
* from terminal: `vim -p file1 file2 file3` opens Vim and the 3 files on separate tabs (panels)

* `Ctrl-^` alternates between 2 buffers
  - type `:ls` to see the open buffers
  - switch between active buffer (marked as `%a`) and the previous one (marked as `#h`) where "h" stands for "hidden"
* `:only` closes all windows leaving only the current one open
* `Ctrl-w _` maximizes the current window vertically
* `Ctrl-w |` maximizes the current window horizontally
* `Ctrl-w =` equalizes/restores original sizes
* `Ctrl-w r` rotates windows clockwise (`R` for inverse)
* `Ctrl-w HJKL` moves the window to the far side direction
* `Ctrl-w T` extracts the current window in a new tab
* `:tabclose` closes current tab
* `:tabonly` closes all tabs leaving the current one open
* `:tabmove <number>` moves the current tab to the N position
* `<number>gt` goes to the N-th tab

### File explore window
* `:Explore` opens an interactive file explorer - same as `:e.`
  - `%` creates a file
  - `d` creates a directory
* `:sp.` opens the file explorer in an horizontal split
* `:vs.` opens the file explorer in a vertical split


## NORMAL mode

* `:r! <system cmd>` >> runs a command on the system shell and reads the output into the current position of the current buffer

### Motions
* `t` for "tag": `cit` as "change inner html tag"
* `/` for "search" term: `c/term` as "change until 'term' is found" 
* `?` is like `/` but searches backwards
* `r` for "replace": select line + `r#` will replace all characters with `#`
* `a` goes to INSERT mode "after" the current character
* `A` goes to INSERT mode at the end of the line ("after ALL characters in line")
* `Y` yanks entire line
* `P` pastes above the cursor while `p` pastes after the cursor
* `g;` goes to the last INSERT mode position - using it multiple times jumps backwards in the changelist history
  - use `g,` to move forward in the changelist
* `%` jumps between parenthesis or line block delimiters (E.g. Ruby's begin-end)
* `s` deletes "character" under cursor and goes into INSERT mode
* `S` deletes "line" and goes into INSERT mode

* With lines that wrap on multiple "visible" lines, prefix commands with `g` to move through visible lines
  - `gj` moves to the next visible line
  - `g$` moves at the end of the visible line

* `:-7t.` copy line from 7 lines above (with relative numbers) "to" (`t`) "this" (`.`) location
  - use `:+10t.` to copy the 10th line below.
  - without +/- sign it consider the "actual" line number
  - `t` is the short for `copy` command


## INSERT mode

### Suggestions
* `Ctr-p` and `Ctr-n` opens a list of suggestions for auto-completion
* `Ctr-x Ctr-f` opens a list of "suggestions based on the files/directories" in the current directory
* `Ctr-x Ctr-l` opens a list of "suggestions based on the lines" in the current file
* `Ctr-x Ctr-p` will suggest similar patterns - also repeating the same commands will continuously add the next line 

### Paste commands
* `Ctr-r 0` pastes content of register `"0` - Like `"0p` in NORMAL mode
* `Ctr-r =` evaluates an expression and inserts the result inline


## VISUAL mode
* `Ctrl-v` to enter in Visual select mode
  - `o` to control the "opposite" corner for selection

## Spelling
* `:set spell` enables the spell checker (`:set nospell` to turn off) - alternatively `:set spell!` to toggle on/off
* `]s` goes to the next misspelled word - or previous one with `[s`
* `z=` opens a list of suggestions
* `1z=` tries to autocorrect using the "first" (more likely) occurrence
* `zg` marks word as good - whitelist word
* `zw` marks word as bad - blacklist word
* `zug` and `zuw` to undo
* `:set spelllang=en` sets the language for the dictionary

## History
* `:earlier 1h` reverts to version of 1 hour ago
* `:later 1h` goes forward to 1 hour ahead

## Commands
* `:!%` runs current file in shell

### Argdo
* `:argdo <command>` runs the command for all files in the `:args` list
  - `:argdo %s/monkey/gorilla/ge` replaces "monkey" with "gorilla" and ignores errors if no matches found
  - `:silent argdo %s/Bob/Alice/ge` hides details of the substitutions
* `:bufdo <command>` does the same for all files in open buffers
* `:argdo update` saves the changes on all modified files
