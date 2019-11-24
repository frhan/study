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

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-1.png)

The first field specifies the group name, the second specifies some information about the group, the third specifies the group ID as a number, and the last specifies a comma-separated list of users who are mem- bers of the group.

- The default group mem- bership of a user is specified in the user’s entry in the `/etc/passwd` file

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-2.png)
![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-3.png)

## Types of File Operations/Access Permissions

### Access Permissions for Directories

- The read permission for a directory allows you to read the contents of the directory; recall that the contents of a directory are the names of files and directories in it. Thus, the ls command can be used to list its con- tents.
- The write permission for a directory allows you to create a new directory or a file in it or to remove an existing entry from it
- The execute permission for a directory is permission to search the directory but not to read from or write to it.

### Determining File Access Privileges

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-4.png)
![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-5.png)
![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-6.png)

- If an argument of the `ls -l` command is a directory, the command displays the long list of all the files and directories in it.
- the ls -ld command to display the long list of directories only.

### Changing File Access Privileges

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-7.png)
![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-8.png)
![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-9.png)
