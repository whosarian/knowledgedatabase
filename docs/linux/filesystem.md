# Filesystem

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

### Hard links

Every file begins with a single hard link from its original name to the data in the file system. When you create a hard link to a file, you create a different name that points to the same data. The new hard link behaves exactly like the original file name. Once created, you cannot tell the difference between the new hard link and the original file name.

To determine if two files are hard-linked, use the `ls` command with the `-i` option to list the inode number of each file. If the files are in the same file system and their inode numbers are the same, then the files are hard links pointing to the same file content.

!!! warning "Hard links have some limitations. First, hard links can only be used with regular files. You cannot use the `ln` command to create a hard link to a directory or a specific file. Second, hard links can only be used if both files reside in the same file system. The file system hierarchy can span multiple storage devices. Depending on your system configuration, this directory and its contents might be stored on a different file system when you navigate to a new directory."

### Symbolic links

The `ln` command with the `-s` option creates a symbolic link, also known as a soft link. A symbolic link is not a regular file, but a special type of file that points to an existing file or directory.

- Symbolic links can connect two files in different systems
- Symbolic links can point to a directory or a specific file, in addition to a regular file

!!! tip "A symbolic link can point to a directory. The symbolic link then behaves like a directory. When you change to the symbolic link using the `cd` command, the current working directory becomes the linked directory."
