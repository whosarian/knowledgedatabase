# Command Line

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
| `[[:punct:]]` | Every printable character thats not a space or alphanumeric |
| `[[:digit:]]` | Any digit from 0 to 9 |
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
