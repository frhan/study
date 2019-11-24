# File Security

## Password-Based Protection

- Every user of a Linux-based computer system is assigned a login name (a name by which the user is known to the Linux system) and a password.
- All login names are public knowledge and can be found in the `/etc/passwd` file

## Protection Based on Access Permission

## Types of Users

- Each user in a Linux system belongs to a group of users
- A user can belong to multiple groups, but a typical Linux user belongs to a single group.
- All the groups in the system and their memberships are listed in the file `/etc/group`
- Comprise the three types of users of a Linux file: _user, group, and others_.
- The login name for the superuser is root and the user ID is 0.

![Drag Racing](Dragster.jpg)

The first field specifies the group name, the second specifies some information about the group, the third specifies the group ID as a number, and the last specifies a comma-separated list of users who are mem- bers of the group.
