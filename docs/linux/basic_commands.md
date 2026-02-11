# Basic Linux Commands

## System status & resources
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

## Processmanagement & services
- `kill`/`pkill`: Kills processes
  - `kill <PID>`: Sends `SIGTERM`
  - `kill -9 <PID>`: Sends `SIGKILL`
  - `pkill -u <user>`: Kills all processes of a specific user
- `systemctl`: Controls services
  - `systemctl status <service>`
  - `systemctl start/stop/restart <service>`
  - `systemctl enable <service>`

## Logs and text processing
- `tail`: Displays the end of a file
  - `-f`: Stays open and displays new lines live
  - `-i`: ignore uppercase/lowercase
  - `-v`: inverts search
- `history`: Displays last used commands
  - `!123`: Execute command number 123 from history
  - `STRG+R`: Start inverted search in terminal

## Network
- `ip a`: Shows all IP's and interfaces
- `ss`: Displays open ports and sockets
  - `tulpn`
- `ping`
- `curl`: Datatransfer via URL
  - `-I`: Only header
  - `-L`: Follows redirects

## Permissions & users
- `chmod`: Change read/write/execute permissions
  - `-R`: Recurse
- `chown`: Change owner
- `sudo`: root
  - `-i`: permanent root shell

## Files & archives
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

## Man-pages
- `man <command>`: Opens manual