# Users and Groups

A user account is used to set security limits between users and programs, that can execute commands. A user has a username, a user-ID (UID) and a password.

Every process that runs on a system is executed by or as a specific user. Every file has a specific user as owner.

## User types

There are three types of user accounts.

### Superuser

A superuser account is intended for system administration. The name of the superuser is `root` and the UID is `0`. The superuser has full access to the system.

### System user

System user accounts are used by processes, that deploy assisting services. Those processes or daemons usually don't have to be executed as superuser. Users don't log in interactively to system user accounts.

### Regular user

Most of the users have regular accounts. They only have restricted access to the system.

## Linux groups

A group is a collection of users, that have shared access to files and system resources. Groups have group names and group-ID's (GID). The assignment of a group to a GID is included in a identity management database. Usually the file `/etc/group` is used to save information about local groups.

### Primary groups and supplementary groups

Every user is assigned to exactly one primary group. For every local user its the group in `/etc/passwd`. The primary group owns the files created by the user.

When creating a regular user, a group with the same name as the user is being created which is considered the primary group for the user. The user is the only member of this private user group.

Users can also be assigned to supplementary groups. The membership to supplementary groups is being saved in `/etc/group`. Users gain access to files depending on whether one of their groups has access to them, and regardless of whether the group is a primary or a supplementary group.

The command `id` can display the group membership of an user.

## Access as superuser

On RHEL the superuser is `root`. This user is able to disable regular permissions on the system and is being used for system administration. To perform tasks such as installing or uninstalling software and managing system files and directories, users must transfer the appropriate permissions to the `root`-user.

Usually only the `root`-user is able to control more than one device at the same time, but there are exceptions. For example, regular users can manage removable media such as USB devices. Regular users are allowed to add and remove files on a removable drive and perform other administrative tasks. However, by default, only the `root`-user has permission to manage hard disk drives.

However, unrestricted privileges come with greater responsibility. The root user has unlimited possibilities to damage the system by deleting files and directories, deleting user accounts, adding vulnerabilities, etc.

!!! warning "Important"
    In RHEL 10, the `root`-account does not have a valid password by default, so you cannot log in directly as the `root`-user with a password.

## Switching user accounts

Using the command

```bash
su <username>
```

you can switch to another user account. If no username is passed, the command tries to change to the `root`-user.

## Managing user accounts

### Create a user

To create a new user the following command is being used:

```bash
useradd <username>
```

This command performs the following tasks to set up and configure the new account:

``` mermaid
graph TD
    UA["# useradd username"]:::main

    LOGIN["/etc/login.defs"]
    SKEL["/etc/skel"]
    HOME["/home/username"]
    GROUP["/etc/group"]
    PASSWD["/etc/passwd"]
    SHADOW["/etc/shadow"]

    UA -->|2 read| LOGIN
    UA -->|3 write| GROUP
    UA -->|3 write| PASSWD
    UA -->|3 write| SHADOW
    UA -->|4 create| HOME
    UA --> SKEL
    SKEL -->|5 copy| HOME

    classDef main stroke:#007bff,stroke-width:2px;
    style UA color:#000
```

??? example "Explanation"
    1. Execute `useradd <username>` as root
    2. Read user creation configuration from `/etc/login.defs`
    3. Write new user data in `/etc/group`, `/etc/passwd` and `/etc/shadow`
    4. User directory is being created under `/home/<username>`
    5. Copy user account data from `/etc/skel` to `/home/<username>`

!!! warning
    At this point, the user account does not have a valid password and the user can only log in once a password has been set.

### Edit existing users

The command `usermod --help` display options that can be used to modify a user account.

| Option | Usage |
| --- | --- |
| `-a`, `--append` | Usage with `-G` to add additional groups to the users group memberships, rather than replacing them |
| `-c`, `--comment` | Adds a comment |
| `-d`, `--home` | Sets the home directory for the user account |
| `-g`, `--gid` | Sets the primary group for the user account |
| `-G`, `--groups` | Comma separated list of supplementary user groups |
| `-L`, `--lock` | Locks the user account |
| `-m`, `--move-home` | Moves the home directory to another place, must be used with `-d` |
| `-s`, `--shell` | Sets a specific login shell for the user account |
| `-U`, `--unlock` | Unlocks a user account |

### Delete users

The command `userdel <username>` removes the user `<username>` from the file `/etc/passwd`, but retains the user's home directory. The command `userdel -r <username>` removes the user from the file `/etc/passwd` and deletes the user's home directory.

!!! danger "Security warning"
    If you remove a user without specifying the `userdel -r` option, the user's files are now associated with an unassigned UID. If you create a new user and assign that user the deleted user's UID, the new account will own those files, posing a security risk. Organizational security policies typically prohibit deleting user accounts and instead lock them to prevent this scenario.

### Set user password

The command `passwd <username>` sets the initial password or changes the existing password for a user.

The root user can set a password to any value. If the password does not meet the recommended minimum criteria, the terminal will display a message. You can re-enter the same password; it will be successfully updated by the `passwd` command.

A regular user must choose a password with at least eight characters. Do not use a dictionary word, your username, or your previous password.

## Managing local group accounts

Groups can be managed with the RHEL web console or the command line.

### Create a group

The command `groupadd` creates a new group. Without passing any parameters, the command will choose the next available GID. By default the value of GID will be higher than the GID value of every other existing group, even when a lower value is available.

With `groupadd -g <ID> <group>` you set the ID for a group. The command `groupadd` with the `-r` option creates system groups.

### Change existing groups

The command `groupmod` changes the attributes of an existing group. Using `-n` a new group name can be set.

### Delete groups

The command `groupdel <group>` deletes the group.

!!! warning "Important"
    A group can't be deleted, if its the primary group of an existing user.

### Primary and supplementary group memberships

A user can only be member of exactly 1 primary group. The primary group is shown in the line that defines the user's account in the `/etc/passwd` file.

The supplementary groups of an user are all other groups that are defined in `/etc/group`. A user can be member of many supplementary groups.

There is no difference between primary and supplementary groups when configuring group-based file permissions. The only difference between those memberships is when a user creates a new file. New files always have to have a user-owner and a group-owner, that are allocated while creation. The primary group of the user becomes the group-owner of the new file.

### Change group memberships

Using `usermod -g` the primary group of a user can be changed.

!!! note
    When changing the primary group of a user, the old primary group will not automatically become a supplementary group.

Using `groupmod -aU` supplementary groups can be added to one ore more users.

## Managing user passwords

### Shadow passwords and password guideline

Previously, encrypted passwords were stored in the `/etc/passwd` file, which could be read by anyone. The passwords, cryptographically encrypted using a hashing algorithm, were moved to the file `/etc/shadow`, which only the root user can read.

As in `/etc/passwd`, every user also has an entry in `/etc/shadow`. An example entry from the file `/etc/shadow` contains nine fields separated by colons:

```txt
user01:$y$j9T$qybndwEWzHhr0uTGAwO4Q0$OuNgGC5Mx2RrCO4JOXtR2VJfTA8dLPxa7NV1tvhziHC:20222:0:99999:7:2:20282::
```

??? example "Breakdown of the sections"
    - `user01`: Name of the user account
    - `$y$j9T$qybndwEWzHhr0uTGAwO4Q0$OuNgGC5Mx2RrCO4JOXtR2VJfTA8dLPxa7NV1tvhziHC`: Hashed password
    - `20222`: Days since the last password change
    - `0`: Minimum number of days that must pass, before the user can change the password again
    - `99999`: Maximum number of days, before the password has to be changed again
    - `7`: Number of days in advance the user is warned that their password is about to expire
    - `2`: Number of days without activity, starting from the day the password expires, before the account is automatically locked
    - `20282`: Number of days till the user account expires
    - The last field is usually empty and a placeholder for upcoming usages

### Verifying passwords

If a user wants to log in, the system is searching for an entry of the user in `/etc/shadow`. The given clear password is combined with the salt of the user. If the result matches the cryptographic hash value, the user has entered the correct password. If the result does not match the cryptographic hash value, the user has entered the wrong password and the login attempt fails. This method allows the system to determine whether the user has entered the correct password without having to store the password in a format that could be used for login.

### Restricting access

You can use the `usermod` command to change the expiration date of a user's account. For example, the `usermod` command with the `-L` option locks a user account, preventing the user from logging into the system.

If an employee leaves the company on a specific date, you can lock and close their user account using the `usermod` command. The date must be specified as the number of days since January 1, 1970, or in the format YYYY-MM-DD:

```bash
usermod -L -e 2026-06-01 user01
```

### `nologin`-shell

The `nologin` shell acts as a fallback shell for user accounts that are not intended for interactive system login. For security reasons, it is advisable to disable system login for an account if the user account does not require it.

This problem is usually solved by setting the user's login shell to /sbin/nologin. If the user tries to log in directly to the system, the nologin shell closes the connection.

!!! warning
    The `nologin` shell prevents interactive use of the system, but not all access. Users could still authenticate and upload or download files, for example, via web applications, file transfer programs, or email readers, if you use the user's password for authentication and do not require a shell.
