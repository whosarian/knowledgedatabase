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
