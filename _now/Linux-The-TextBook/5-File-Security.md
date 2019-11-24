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
- the `ls -ld` command to display the long list of directories only.

### Changing File Access Privileges

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-7.png)
![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-8.png)
![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-9.png)

The access permissions for all the files and directories under one or more directories can be set by using the chmod command with the `-R` option.

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-10.png)

### Default File Access Privileges

When a new file or directory is created, Linux sets its access privileges based on the current mask value. On Linux systems, the default access privileges for the newly created files and directories are `777` for executable files and directories and `666` for text files.

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-11.png)

The argument of umask is a bit mask, specified in octal, that identifies the permission bits that are to be turned off when a new file is created. The values of other access permission bits are computed by the Boolean expression:

`A = B AND C' = BC'`

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-12.png)

We now show a few examples how Linux assigns file access permissions to newly created files and directories for a given mask. We determine the access permissions for a newly created directory or executable file for the mask value 022 by using the earlier Boolean expression.

```
C = 022
C'
B = 777
A = B AND C' = 111 101 101 = 755 (octal) = 111101101 (binary)
                                           rwxr-xr-x (symbolic)
```

Thus, file access permissions are 755 (read, write, execute for user, read and execute for group and others). For a text file, B is 666. Thus, access permissions for a newly created text file would be 644 as follows:

```
C' = 111 101 101
B = 666 = 110 110 110
A = B AND C' = 110 100 100 = 644 (octal) = 110100100 (binary)
                                           rw-r--r-- (symbolic)
```

The access permissions for a newly created text file for the mask value 077 would be 600, as follows:

```
C = 077      = 000 111 111
C'           = 111 000 000
B = 666      = 110 110 110
A = B AND C' = 110 000 000 = 600 (octal) = 110000000 (binary)
                                           rw------- (symbolic)
```

## Special Access Bits

### SUID Bit

we want users to be able to change their passwords, but at the same time, they must not have write access to the `/etc/passwd` file to keep information about other users in this file from being compromised.

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-13.png)

Every Linux file has an additional protection bit, called the SUID bit, associated with it. If this bit is set for a file containing an executable program, the program takes on the privileges of the owner of the file when it executes. Thus, if a file is owned by root and has its SUID bit set, it runs with superuser privileges. This bit is set, for example, for the passwd command. So, when you run the passwd command, it can write to the `/etc/passwd` file (replacing your existing password with the new password), even though you do not have access privileges to write to the file.

### SGID Bit

The SGID bit works in the same manner in which the SUID bit does, but it causes the access permissions of the process to take the group identity of the group to which the owner of the file belongs.

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-14.png)

### Sticky Bit

The last of the 12 access bits, the sticky bit, is on if the execute bit for others is t (or T), as in the case / tmp given as follows:

![Drag Racing](https://github.com/frhan/study/blob/master/_now/Linux-The-TextBook/Images/5-15.png)
