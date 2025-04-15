---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
Module:
  - "[[1 - Fundamental Concepts]]"
Week: 1
Lecture:
  - "2"
Chapter: 
Slides/Notes:
  - "[[L1.2-ethics-paul.pdf]]"
Date: 2025-01-09
aliases: []
Date created: Thu., Jan. 9, 2025, 3:10:29 pm
Date modified: Fri., Jan. 17, 2025, 8:51:55 pm
---

# File Management in UNIX

> [!question]- What is a file?
> - Sequence of bytes stored “somewhere”

> [!goal]+ What is a file system?
> - Why do we need one?

## Storage Media

> [!def]+ Storage Media
>
> - Hard drive, solid state drive, etc.
> - Also just a (large) sequence of bytes

- File system is needed to organize bytes on the file system

![](https://i.imgur.com/ouDfEuy.png)

## Unix/Linux File System

- Everything is a file
    - Plaintext documents
    - Executables
        - e.g., gcc
    - Directories are also files
- **Root** directory
    - Name `/`
    - Starting point
- **Directory** (folder)
    - Special file that contains mappings of filenames to their data and *metadata*
- **Metadata**
    - Information kept about a file
    - e.g., File size, file owner, permissions, last modified time, etc.

### File System Directory Hierarchy

![](https://i.imgur.com/oLESBBB.png)

## Shared Systems

- When you log into a lab workstation or remote log into `teach.cs`,
    - You are using a **shared system**

> [!question] How does the file system know which files belong to you and prevent others from seeing or modifying them?

## Permissions

- Every file and directory has an owner and a group
- Every file and directory has a set of **permissions**
- **Permissions**
    - Mechanisms that allow us to keep our own data private and also to share files with others

![](https://i.imgur.com/UIMQBWc.png)

- Type of file:
    - `d` = directory
    - `-` = regular file

![|330](https://i.imgur.com/abKuFiO.png)

|           | Read `r`                             | Write `w`           | Execute `x`       |
| --------- | ------------------------------------ | ------------------- | ----------------- |
| File      | View contents                        | Modify contents     | Load and run file |
| Directory | View directory contents (e.g., `ls`) | Add or delete files | “pass through”    |

- If you can read a directory/folder,
    - Can run `ls`

> [!example]+ Reading Permission String
> Given: `-rwxr-xr--`
>
> - First character (`-`) indicates regular file
> - Next 3 characters (`rwx`) = owner permissions
> - Next 3 characters (`r-x`) = group permissions
> - Last 3 characters (`r--`) = others permissions
>
> So this file has:
>
> - Owner: read, write, execute
> - Group: read, execute
> - Others: read only

### Unix Group

- A set of users who can share files
- A user can belong in many groups
    - e.g., Be in the `csc209` group, `class of 2027` group, and `utsg` group
- A file can only be set to ==one group==
- File owner does not have to be in the file’s group

### Changing File Permissions

- `chmod`
    - “Change mode”
    - Command you use to change permissions of a file
- `chmod 755 <filename>`
    - Three numbers between 0 and 7
        - The *octal value* for that category of user

| Binary | Value | Permission |
| ------ | ----- | ---------- |
| `001`  | 1     | execute    |
| `010`  | 2     | write      |
| `100`  | 4     | read       |

> [!important]+ Frequently Used Permission Numbers
>
> - `777` (`rwxrwxrwx`)
>     - Full permissions for everyone (rarely used, security risk)
> - `755` (`rwxr-xr-x`)
>     - Directories, executable files
>     - Owner can do anything
>     - Others can read and execute
> - `644` (`rw-r--r--`)
>     - Regular files
>     - Owner can read/write
>     - Others can only read
> - `600` (`rw-------`)
>     - Private files
>     - Only owner can read/write
> - `700` (`rwx------`)
>     - Private directories
>     - Only owner has all permissions

- Another approach:
    - `chmod u+rwx`
    - `chmod go-x`
    - Adds or removes permissions for those categories of users

#### Symbolic Notation for `chmod`

> [!info]+ Understanding `chmod` symbols
> Format: `chmod WHO±PERMISSION`
>
> Who:
>
> - `u` = user (owner)
> - `g` = group
> - `o` = others
> - `a` = all (equivalent to ugo)
>
> Permission:
>
> - `r` = read
> - `w` = write
> - `x` = execute
>
> Operators:
>
> - `+` adds permission
> - `-` removes permission
> - `=` sets exact permission

> [!example]+ Common `chmod` Examples
>
> - `chmod u+x file`
>     - Give owner execute permission
> - `chmod go-w file`
>     - Remove write permission from group and others
> - `chmod a=r file`
>     - Set all users to read-only
> - `chmod u+rwx,go+rx file`
>     - Multiple changes separated by comma
> - `chmod 644 file`
>     - Same as `chmod u=rw,go=r file`

> [!warning]+ Common Permission Mistakes
>
> - Setting files to 777 (security risk)
> - Removing execute (`x`) from directories
>     - Cannot access files inside without it
> - Removing read (`r`) from directories
>     - Cannot list contents
> - Setting sensitive files readable by others

## Privacy

When you shift from your own computer to a *shared computer system*, the biggest differences are:

- Shared computational resources
    - Raises questions about ==fairness==
- Shared file storage
    - Raises questions about ==privacy==

> [!tldr] See [[The Ethics of Data Protection]].
