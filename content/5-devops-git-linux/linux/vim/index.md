


#### Exit the vim:
| Command   | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `:q`      | Quit (fails if there are unsaved changes)                    |
| `:q!`     | Quit without saving changes                                  |
| `:wq`     | Write (save) and quit                                        |
| `:x , ZZ` | Save and quit (only writes if changes were made)             |
| `:wq!`    | Force save and quit (useful when permissions are restricted) |

#### insert mode :

| Command | Description                            |
| ------- | -------------------------------------- |
| `i`     | Insert before the cursor               |
| `I`     | Insert at the beginning of the line    |
| `a`     | Insert (append) after the cursor       |
| `A`     | Insert (append) at the end of the line |
| `o`     | Append (open) a new line below         |
| `O`     | Append (open) a new line above         |
#### Delete text:
| Command     | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| `Backspace` | Deletes the character before the cursor                      |
| `Ctrl-h`    | Same as `Backspace`, deletes one character before the cursor |
| `Ctrl-w`    | Deletes the previous word                                    |
| `Ctrl-u`    | Deletes everything before the cursor on the current line     |
#### cursor movement:
| Command       | Description                                        |
| ------------- | -------------------------------------------------- |
| `h`           | Move left (←)                                      |
| `l`           | Move right (→)                                     |
| `j`           | Move down (↓)                                      |
| `k`           | Move up (↑)                                        |
| `0`           | Move to the beginning of the current line          |
| `^`           | Move to the first non-blank character of the line  |
| `$`           | Move to the end of the current line                |
| `w`           | Move to the beginning of the next word             |
| `e`           | Move to the end of the current/next word           |
| `b`           | Move to the beginning of the current/previous word |
| `gg`          | Move to the beginning of the file                  |
| `G`           | Move to the end of the file                        |
| `nG`          | Move to line `n` (e.g., `10G` moves to line 10)    |
| `H`           | Move to the top of the screen                      |
| `M`           | Move to the middle of the screen                   |
| `L`           | Move to the bottom of the screen                   |
| `Ctrl-d`      | Move half a screen down                            |
| `Ctrl-u`      | Move half a screen up                              |
| `Ctrl-f`      | Move one full screen down                          |
| **o**`Ctrl-b` | Move one full screen up                            |
