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
