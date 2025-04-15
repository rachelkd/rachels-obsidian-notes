---
dg-publish: true
tags: ["#lecture", "#note", university]
Course Code:
  - "[[CSC207]]"
Week: 1
Module:
  - "[[1 - Software Developer Skills and Tools]]"
Date: 2024-09-05
Date created: Thu., Sep. 5, 2024, 1:16:12 pm
Date modified: Tue., Dec. 10, 2024, 6:03:27 am
---

# Terminal Commands

`pwd`
- Present working directory

    ```
    pwd
    /Users/rachel/Desktop/CSC207/csc207
    ```

# Git

CSC207 will normally create the remote repo for you:
1. GitHub
2. MarkUs

- You *clone* the remote repo
- Work on files *locally*
- *Add* and *commit* changes to local repo
- *Push* changes to the remote repository

# Remote Repository

- What is the **origin**?
    - The repo that lives on another server
    - ![](https://i.imgur.com/4jRmMRO.png)

# Clone to Get Local Repository

- `clone` command makes a copy of the remote repository on your local machine
- ![](https://i.imgur.com/Qf2LOBT.png)
- → There are two repositories
    - Git is a *distributed* [[Version Control#^40ec88|version control system]]
    - ![](https://i.imgur.com/k4J4Npp.png)
    - Implementation details are in local repo

# Local Repository

- Actual repo + how Git works is hidden
    - There is a file, `.git`, that contains all the repository details
    - It is there, but it is like a private attribute in Python
- Git provides a bunch of commands to update repo
- Can *create* new files, *make changes* to files, and *delete* files
- When you want to **commit** a change to the local repository → first **stage** the changes
    - i.e., When you want to officially do something, tell Git

# Staging Changes

- `git add` does not add files to repo!
    - Instead: Marks a file as being part of the current change
    - Think of `git add` as `git` *stage*

> [!warning]- Consequence
> When you make changes to a file, and then add and commit them,
> - The next time you make some changes to a file, you will still have to run `git add` to stage the changes to the next commit

# Git Status

- A file can be in one of four states:
    - `untracked`
        - Never run a git command on the file
    - `tracked`
        - Committed
    - `staged`
        - `git add` has been used to add changes in this file to the next commit
    - `dirty`/`modified`
        - File has changes that haven’t been staged

> [!tip]- Use `git status` regularly → helps make sure changes you have made make it into the repo
> - IntelliJ uses colours and symbols to help you easily see status of files

# Basic Workflow (no branching)

- Starting a project:
    - `git clone <url>`
- Normal work, after you have made some changes:
    - `git status`
        - See what has really changed
        - Remind you what you’ve been working on
    - `git add file1 file2 file3 …`
        - Stage changes i.e., “Hey Git, this is what I’ve been working on”
    - `git commit -m "meaningful commit message"`
        - Updates local repo, save a copy locally; have not given it to anyone else
        - Changes status of staged file(s)
    - `git push`
        - Pushes local changes to the remote repo
    - IntelliJ: Either type these commands in the Terminal or use the graphical UI it provides

# Distributed Version Control Systems

![](https://i.imgur.com/DwrbuJh.png)

- Paul is going to `clone` the (server) repo i.e., master or origin

![](https://i.imgur.com/zZjApuz.png)

- Jon makes changes, stages/adds, and commits
- `git push` sends the files to the server repo
- Lindsey does `git pull` to get up-to-date files
    - Git will tell her that there were changes; here’s what changed; you now have the up-to-date files

![](https://i.imgur.com/74cH6F7.png)

- Maybe Paul forgot to update (`git pull`)
- He makes changes, and tries to `push` → Git will complain: **conflict**
    - ==Other changes== have happened at the same time ==in the same file(s)==
    - Push is going to fail
    - Read the error messages, follow the instruction Git provides

# Branch and Merge Workflow

> [!important]+ To avoid conflicts, developers often create a **branch**
> - Work on new feature in new branch (a local copy)
> - Submit a **pull request** (i.e., Push it to the remote/server repo)

- When you make your changes, you push directly onto the remote repo → Problem: if anyone forgets to pull, then they are not up-to-date
- When you make a new feature, you should ==make a new branch==

![|300](https://i.imgur.com/5ooRjcA.png)

- Name branch something meaningful: what you’ve been working on
- Push the branch to the remote repo
    - Acts as an alternate repo
    - If anyone wants to, they can get a copy from the new branch, look at changes, decide if later want to merge changes in the main branch
    - Does not replace the other branches

For example,
- The current state of the program is `main`
- You push an alternative that you are proposing to the team
- Team can do a review
- If happy with branch J, they can make that the new version of the code

We may still encounter conflicts with branching, but it allows better control of when conflicts occur. If we weren’t using branching, then a team member might run into a conflict and not be expecting it and have to deal with it without the time to do so.
