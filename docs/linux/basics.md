# Basics

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

## Commands

### System status & resources

- `top`/`htop`: Displays running processes, CPU and RAM usage in realtime
- `df` (Disk free): Shows free storage capacity on mounted filesystems
  - `h`: Human readable
  - `T`: Shows filesystem type
- `du` (Disk Usage): Shows how much storage files and folders occupy
  - `-sh *`: Shows a summary in readable size for all elements in the current folder
  - `-d 1`: Directory depht (1 in this case)
- `free`: Displays current RAM usage
  - `-s 3`: Updates every 3 seconds
- `uptime`: Displays the current uptime of the system and all logged in users

### Processmanagement & services

- `kill`/`pkill`: Kills processes
  - `kill <PID>`: Sends `SIGTERM`
  - `kill -9 <PID>`: Sends `SIGKILL`
  - `pkill -u <user>`: Kills all processes of a specific user
- `systemctl`: Controls services
  - `systemctl status <service>`
  - `systemctl start/stop/restart <service>`
  - `systemctl enable <service>`

### Logs and text processing

- `tail`: Displays the end of a file
  - `-f`: Stays open and displays new lines live
  - `-i`: ignore uppercase/lowercase
  - `-v`: inverts search
- `history`: Displays last used commands
  - `!123`: Execute command number 123 from history
  - `STRG+R`: Start inverted search in terminal

### Network

- `ip a`: Shows all IP's and interfaces
- `ss`: Displays open ports and sockets
  - `tulpn`
- `ping`
- `curl`: Datatransfer via URL
  - `-I`: Only header
  - `-L`: Follows redirects

### Permissions & users

- `chmod`: Change read/write/execute permissions
  - `-R`: Recurse
- `chown`: Change owner
- `sudo`: root
  - `-i`: permanent root shell

### Files & archives

- `find`: Find files
  - `-name "*.log"`: Files that end on `".log"`
  - `-mtime +7`: Files that are older than 7 days
  - `-size +100M`: Files that are larger than 100 MB
- `tar`: Manage archives
  - `c`: Create
  - `z`: Compress
  - `v`: Verbose
  - `f`: File
  - `x`: Extract

### Man-pages

- `man <command>`: Opens manual
