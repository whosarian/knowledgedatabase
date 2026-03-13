# Vim

!!! example "Open a file with `vim` using"
    ```bash
    vim <filename>
    ```

## Modes

Vim always starts in normal mode.

### Normal Mode

- Navigation through the document
- Deleting lines, copying, pasting and undoing
- Using ++escape++ wil always bring you back to normal mode

### Insert Mode

There are many different ways to activate the insert mode:

- ++i++ (insert): Add text in front of the cursor
- ++a++ (append): Add text behind the cursor
- ++shift+i++: Jumps to the start of the current line and changes to insert mode
- ++shift+a++: Jumps to the end of the current line and changes to insert mode
- ++o++: Opens a new empty line under the cursor
- ++shift+o++: Opens a new empty line above the cursor

### Command-Line Mode

Opens up a terminal at the bottom of the window.

- ++colon++: To indicate the start of a command
- ++slash++: Forwardsearch
- ++question++: Backwardsearch

!!! tip "Important commands"
    | Command | Action |
    | --- | --- |
    | `:w` | Saves the file |
    | `:q` | Quits vim, but only if nothing has changed |
    | `:wq` or `:x` | Saves changes and quits vim |
    | `:q!` | Quits vim without saving changes |
    | `:w <filename>` | Saves the current text in a new file |
    | `:set number` | Display line numbers |
    | `:set nonumber`| Hide line numbers |
    | `:set paste` | Disable automatic indentation |
    | `:set nopaste` | Enable automatic indentation |
    | `:e <filename>` | Opens a different file in the current window |
    | `:sp <filename>` | Split display horizontal and opens the other file |
    | `:vsp <filename>` | Split display vertical and opens the other file |

### Visual Mode

Select text ranges in order to then delete (++d++), copy (++y++) or indent (++greater++).

- ++v++: Character-based visual mode
- ++shift+v++: Line-by-line visual mode
- ++ctrl+v++: Block-wise visual mode

### Replace Mode

- ++shift+r++
- The existing text to the right of the cursor is immediately overwritten (replaced) instead of being moved to the right.
