



# insert mode : 


## Insert Mode Entry Commands

| Command | Description                                                       |
| ------- | ----------------------------------------------------------------- |
| `i`     | Insert before the cursor                                          |
| `I`     | Insert at the beginning of the line                               |
| `a`     | Append after the cursor                                           |
| `A`     | Append at the end of the line                                     |
| `o`     | Open a new line below and insert                                  |
| `O`     | Open a new line above and insert                                  |
| `gi`    | Insert at the last insert position                                |
| `gI`    | Insert at the beginning of the line (even in virtual spaces)      |
| `s`     | Substitute character (delete it and enter insert mode)            |
| `S`     | Substitute entire line (like `cc`)                                |
| `C`     | Change to end of line (like `c$`)                                 |
| `R`     | Enter Replace mode (overwrite text)                               |
| `gr`    | Enter Virtual Replace mode (like R, but visual columns preserved) |



# normal mode : 


| Motion          | Description                             | Example/Analogy                 |
| --------------- | --------------------------------------- | ------------------------------- |
| `h` / `l`       | Left / Right                            | Move one character left/right   |
| `j` / `k`       | Down / Up line                          | Navigate lines                  |
| `w` / `W`       | Next word (small `w` skips punctuation) | Jump word-by-word               |
| `b` / `B`       | Previous word                           | Go back word-by-word            |
| `e` / `E`       | End of word                             | Jump to the end of the word     |
| `0` / `^`       | Line start (`^` skips indent)           | Go to beginning of line         |
| `$`             | End of line                             |                                 |
| `gg`            | Go to top of file                       | Think “Go Go” to top            |
| `G`             | Go to bottom of file                    |                                 |
| `H` / `M` / `L` | Top/Middle/Bottom of screen             | High, Middle, Low of viewport   |
| `{` / `}`       | Previous/Next paragraph                 | Block-level jumps               |
| `%`             | Jump to matching `()`, `{}`, or `[]`    | Like bracket balance check      |
| `f<char>`       | Move to next `<char>` on line           | `f,` jumps to next comma        |
| `;` / `,`       | Repeat forward / backward `f` or `t`    | Repeat last character motion    |
| `/pattern`      | Search forward for text                 | `/for` finds next `for` keyword |
| `?pattern`      | Search backward                         |                                 |
| `n` / `N`       | Next / Previous match                   | Navigate search results         |

### Line & Block Operations

|         |                     |
| ------- | ------------------- |
| Command | Description         |
| `>>`    | Indent line         |
| `<<`    | Unindent line       |
| `=`     | Auto-indent         |
| `J`     | Join line with next |
| `~`     | Toggle case         |
## motion : 

### delete : 

|Command|Description|
|---|---|
|`dd`|Delete the **entire current line**|
|`D`|Delete from **cursor to end of line** (same as `d$`)|
|`dw`|Delete from **cursor to start of next word**|
|`daw`|Delete **around a word** (includes spaces/punctuation)|
|`diw`|Delete **inside a word** (does not include spaces)|
|`de`|Delete until the **end of the current word**|
|`dE`|Delete until the **end of WORD** (including punctuation)|
|`d0`|Delete from **cursor to beginning of line**|
|`d^`|Delete from **cursor to first non-whitespace**|
|`d$`|Delete from **cursor to end of line**|
|`dG`|Delete from **cursor to end of file**|
|`db`|Delete from **cursor to beginning of previous word**|
|`dB`|Delete from **cursor to beginning of previous WORD**|
|`d}?`|Delete from **cursor to next sentence** (`d?}` is `d)`)|
|`d(`|Delete backward to **beginning of sentence**|
|`d}`|Delete forward to **end of paragraph**|
|`d{`|Delete backward to **beginning of paragraph**|
|`d/…`|Delete until the **search pattern**|
|`d?…`|Delete backward until the **search pattern**|
|`df<char>`|Delete through the **next occurrence** of `<char>`|
|`dF<char>`|Delete backward through the previous `<char>`|
|`dt<char>`|Delete until **before** the next `<char>`|
|`dT<char>`|Delete backward until **after** the previous `<char>`|
|`d%`|Delete matching **bracket pair** (on a brace, parenthesis, etc.)|

### dfd

| Operator  | Action           | Example | Description                 |
| --------- | ---------------- | ------- | --------------------------- |
| `d`       | delete           | `dw`    | delete word                 |
| `y`       | yank (copy)      | `y$`    | copy to end of line         |
| `c`       | change           | `c}`    | change until next paragraph |
| `>` / `<` | indent/un-indent | `>G`    | indent until end of file    |
| `~`       | toggle case      | `g~w`   | toggle case of next word    |
| `=`       | auto-indent      | `=}`    | re-indent paragraph         |


| Command | Description          |
| ------- | -------------------- |
| `.`     | Repeat last command  |
| `3w`    | Move 3 words forward |
| `5dd`   | Delete 5 lines       |
| `d3w`   | Delete 3 words       |


# Ex mode : 

| Command       | Description              |
| ------------- | ------------------------ |
| `:w`          | Write (save)             |
| `:q`          | Quit                     |
| `:wq` / `:x`  | Save and quit            |
| `:q!`         | Quit without saving      |
| `:e filename` | Open file                |
| `:vsp file`   | Vertical split open file |
| `:tabnew`     | Open new tab             |
| `:bd`         | Delete (close) buffer    |
| `:ls`         | List buffers             |


# visual mode : 


