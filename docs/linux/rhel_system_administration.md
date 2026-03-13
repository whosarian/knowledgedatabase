# Red Hat Enterprise Linux System Administration

## Executing commands with bash

*Using `;` between commands will execute all the commands, no matter if what the output is:*  

```bash
whoami ; foo ; bar ; date
```

Output:

```bash
student
bash: foo: command not found...
Failed to search for file: GDBus.Error:org.freedesktop.DBus.Error.NameHasNoOwner: Could not activate remote peer 'org.freedesktop.PackageKit': activation request failed: unit is masked
bash: bar: command not found...
Failed to search for file: GDBus.Error:org.freedesktop.DBus.Error.NameHasNoOwner: Could not activate remote peer 'org.freedesktop.PackageKit': activation request failed: unit is masked
Fri Mar 6 13:19:27 UTC 2026
```

*Using `&&` between commands will only execute the next command, if the one before was successfull:*

```txt
whoami && foo && bar && date
```

Output:

```txt
student
bash: foo: command not found...
Failed to search for file: GDBus.Error:org.freedesktop.DBus.Error.NameHasNoOwner: Could not activate remote peer 'org.freedesktop.PackageKit': activation request failed: unit is masked
```

*Using `||` between commands will only execute the next command if the one before failed:*

```bash
foo || bar || date || whoami
```

Output:

```txt
bash: foo: command not found...
Failed to search for file: GDBus.Error:org.freedesktop.DBus.Error.NameHasNoOwner: Could not activate remote peer 'org.freedesktop.PackageKit': activation request failed: unit is masked
bash: bar: command not found...
Failed to search for file: GDBus.Error:org.freedesktop.DBus.Error.NameHasNoOwner: Could not activate remote peer 'org.freedesktop.PackageKit': activation request failed: unit is masked
Fri Mar 6 13:19:27 UTC 2026
```

*Commands can be broken down to be easier readable using `\`:*

```bash
mkdir -pv foo && \
> cd foo && \
> rm -rf foo
```

The first line that is entered without the  `\` will execute the command.

## Display the content of a file

- `cat <path_to_file>`: Displays the whole content of a file from top to bottom
- `tac <path_to_file>`: Displays the whole coneten of a file from bottom to top
- `head <path_to_file>`: Displays the first 10 lines of a file
- `tail <path_to_file>`: Displays the last 10 lines of a file
- `tail -f <path_to_file>`: Updates the last 10 lines of a file live
- `less <path_to_file>`: Displays the whole content of file page by page (allows searching using `/`)

## Navigate in a command line

- `Strg + A`: Jump to the start of the line
- `Strg + E`: Jump to the end of the line
- `Strg + Right_Arrow`: Jump one word to the right
- `Strg + Left_Arrow`: Jump one word to the left
- `Strg + U`: Delete everything thats left from the cursor
- `Strg + K`: Delete everything thats right from the cursor

## Filesystem hierarchy

All files in a Linux system are stored in file systems organized in a single inverted tree structure called the file system hierarchy. This tree structure is inverted because the root of the tree is at the top of the hierarchy, and directories and subdirectories spread out below the root.

The root directory, indicated by a single forward slash (`/`), is the top level of the file system hierarchy. The forward slash (`/`) is also used as a directory separator in filenames.

!!! note "The different content types of file system directories are described using the following terms:"
    - Static content remains unchanged unless explicitly modified or reconfigured.
    - Dynamic or variable content can be modified or added to by active processes.
    - Persistent content persists after a system restart.
    - Runtime content from a process or the system is deleted upon restart.

### Important directories in RHEL

| Storage location | Purpose |
| --- | --- |
| `/boot` | Files for the startup process |
| `/dev` | Special device files used by the system to access hardware |
| `/etc` | System specific configuration files |
| `/home` | User directory for regular users |
| `/root` | User directory for superusers |
| `/run` | Runtime data for processes that have been started since the last startup operation |
| `/tmp` | Storage space for temporary files |
| `/usr` | Installed software, shared libraries, including files and read-only program data |
| `/var` | System-specific variable data that should be retained between startup processes |

## Creating links between files

You can create multiple filenames that point to the same file. These filenames are called links.

You can create two types of links: a hard link or a symbolic link (sometimes called a soft link). Each type of link is useful for different use cases.

### Hardlinks

Every file begins with a single hard link from its original name to the data in the file system. When you create a hard link to a file, you create a different name that points to the same data. The new hard link behaves exactly like the original file name. Once created, you cannot tell the difference between the new hard link and the original file name.

To determine if two files are hard-linked, use the `ls` command with the `-i` option to list the inode number of each file. If the files are in the same file system and their inode numbers are the same, then the files are hard links pointing to the same file content.

!!! warning "Hard links have some limitations. First, hard links can only be used with regular files. You cannot use the `ln` command to create a hard link to a directory or a specific file. Second, hard links can only be used if both files reside in the same file system. The file system hierarchy can span multiple storage devices. Depending on your system configuration, this directory and its contents might be stored on a different file system when you navigate to a new directory."

### Symbolic links

The `ln` command with the `-s` option creates a symbolic link, also known as a soft link. A symbolic link is not a regular file, but a special type of file that points to an existing file or directory.

- Symbolic links can connect two files in different systems
- Symbolic links can point to a directory or a specific file, in addition to a regual file

!!! tip "A symbolic link can point to a directory. The symbolic link then behaves like a directory. When you change to the symbolic link using the `cd` command, the current working directory becomes the linked directory."

## Command line extensions

When you type a command at the Bash shell prompt, the shell processes that command line through several extensions before executing it. You can use these shell extensions to perform complex tasks that would otherwise be difficult or impossible.

Bash executes the following extensions:

- Pathname expansion: allows one or more files to be selected to pattern matching
- Paranthesis expansion: can generate multiple strings
- Tilde extension: extends to a path to a user directory
- Variable extension: replaces text with the value stored in the varible
- Command substitution: replaces text with the output of a command

### Pathname expansion

Path name expansion, formerly known as globbing, is one of Bash's most useful features. Path name expansion uses wildcard characters. These are metacharacters that expand to match the searched file and path names. Therefore, commands can be applied to a specific number of files in a single execution.

| Pattern | Matches |
| --- | --- |
| `*` | String with zero or more characters |
| `?` | Every single character |
| `[abc]` | Every character between the square brackets |
| `[!abc]` | Every character except those between the square brackets |
| `[^abc]` | Every character except those between the square brackets |
| `[[:alpha:]]` | Every character of the alphabet |
| `[[:lower:]]` | Every lowercase character |
| `[[:upper:]]` | Every uppercase character |
| `[[:alnum:]]` | Every character of the alphabet or numbers |
| `[[:punct:]]`| Every printable character thats not a space or alphanumeric |
| `[[:digit:]]`| Any digit from 0 to 9 |
| `[[:space:]]` | Every single empty space, including tabs, line breaks, paragraph ends, page feeds or spaces |

### Paranthesis expansion

Parenthesis expansion generates any arbitrary string. Parentheses contain a comma-separated list of strings or a sequence of expressions. The result includes the text before or after the parentheses definition. Parenthesis expansions using curly braces can be nested within each other.

For example, the colon syntax `{m..p}` is extended to `m n o p` within curly braces.

### Tilde extension

In Bash, the tilde character (`~`) is a keyboard shortcut for the current user's home directory. You can also use a tilde (`~`) followed by a username to resolve the specified user's home directory.

### Variable extension

A variable behaves like a named container in which a value is stored. Variables simplify accessing and modifying the stored data, either via the command line or a shell script.

You can assign data as a value to a variable using the following syntax:

```bash
<VARIABLENAME> = value
```

You can use variable expansion to convert the variable name on the command line to its value:

```bash
USERNAME = arian
echo $USERNAME
```

### Command substitution

Command substitution allows the output of a command to replace the actual command on the command line. Command substitution occurs when a command is enclosed in parentheses and preceded by a dollar sign (`$`). Multiple command extensions can be nested within each other using the format `$(command)`.

```bash
echo Today is $(date +%A)
```

### Protecting arguments from expansion

Many characters have specific meanings in the Bash shell. To prevent shell extensions from being applied to parts of your command line, you can enclose characters and strings in quotation marks and escape them.

The backslash (`\`) is an escape character in the Bash shell. It protects the character immediately following it from being extended.

```bash
echo The value of $HOME is your home directory. # The value of /home/user is your home directory.
echo The value of \$HOME is your home directory. # The value of $HOME is your home directory.
```

## Editing files with `vim`

!!! example "Open a file with `vim` using"
    ```bash
    vim <filename>
    ```

### Modes

Vim always starts in normal mode.

#### Normal Mode

- Navigation through the document
- Deleting lines, copying, pasting and undoing
- Using ++escape++ wil always bring you back to normal mode

#### Insert Mode

There are many different ways to activate the insert mode:

- ++i++ (insert): Add text in front of the cursor
- ++a++ (append): Add text behind the cursor
- ++shift+i++: Jumps to the start of the current line and changes to insert mode
- ++shift+a++: Jumps to the end of the current line and changes to insert mode
- ++o++: Opens a new empty line under the cursor
- ++shift+o++: Opens a new empty line above the cursor

#### Command-Line Mode

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

#### Visual Mode

Select text ranges in order to then delete (++d++), copy (++y++) or indent (++greater++).

- ++v++: Character-based visual mode
- ++shift+v++: Line-by-line visual mode
- ++ctrl+v++: Block-wise visual mode

#### Replace Mode

- ++shift+r++
- The existing text to the right of the cursor is immediately overwritten (replaced) instead of being moved to the right.

## Redirecting outputs

A running program or process reads input and writes output. Executing a command at the shell prompt typically reads the input from the keyboard and sends the output to the terminal window.

A process uses numbered channels, called file descriptors, to receive input and send output. All processes start with at least three file descriptors:

- Standard input (Channel 0): Reads the input
- Standard output (Channel 1): Sends the output
- Standard error: (Channel 2): Sendr error messages

!!! note "The following table summarizes information about the file descriptors:"
    | Number | Name | Description | Connection | Usage |
    | --- | --- | --- | --- | --- |
    | 0 | `stdin` | Standard input | Keyboard | Read-only |
    | 1 | `stdout` | Standard output | Terminal | Write-only |
    | 2 | `stderr` | Stanard error | Terminal | Write-only |
    | 3 | `filename` | Other files | None | Read, write or both |

### Operators

| Operator | Description |
| --- | --- |
| `> FILE` | Redirect `stdout` to override a file |
| `>> FILE` | Redirect `stdout` to append to a file |
| `2> FILE` | Redirect `stderr` to override a file |
| `2> /dev/null` | Discard `stderr` messages by redirecting them to `/dev/null` |
| `> FILE 2>&1` or `&> FILE` | Redirect `stdout` and `stderr` to override the same file |
| `>> FILE 2>&1` or `&>> FILE` | Redirect `stdout` and `stderr` to append to the same file |

### Creating pipelines

A pipeline is a sequence of one or more commands separated by the pipe character (`|`). A pipeline connects the standard output of the first command to the standard input of the next command.

Pipelines allow one process to manipulate and format the output of another process before sending it to the terminal. Imagine data flowing through the pipeline from one process to another, being modified by each command it passes through.

??? example "Examples"
    ```bash
    ls -l /usr/bin | less # The output of the `ls` command is redirected to the `less` command, so that each screen is displayed sequentially on the terminal
    ```
    ```bash
    ls | wc -l # Output of the ls command to the wc -l command, which counts the number of lines received by the ls command and outputs the value to the terminal
    ```
    ```bash
    ls -t | head -n 10 > /tmp/first-ten-changed-files # Output of the ls -t command to the head command to display the first 10 lines, with the final result redirected to the file /tmp/first-ten-changed-files
    ```

!!! info "Note"
    When you combine redirection with a pipeline, the shell first sets up the entire pipeline and then forwards input/output. If output redirection is used in the middle of a pipeline, the output is directed to a file, not to the next command in the pipeline.
