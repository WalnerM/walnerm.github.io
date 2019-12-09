---
layout: page
title: Git
author: Leandro Chiarini and Walner Mendonça
permalink: /wtf-is-git/
---

> # What is Git and why you should care?

>> By Leandro Chiarini and Walner Mendonça

-------------------------------------------------------------------------------

# What is it? What is it for?

  "Git is a free and open source distributed version  control system
  designed to handle everything  from small to very large projects
  with speed and efficiency. "

-------------------------------------------------------------------------------

# Advantages when compared to Dropbox-like services

- git saves all versions of the files
- git can be setup in any directory
- git can ignore files you do not want to sync
- git was made for collaborative coding
- git can be hosted locally or on multiple web services

-------------------------------------------------------------------------------

# Shell basic commands


|---------|-----------------------------------------|-----------------------|
| COMMAND | FUNCTION                                | COMMON USAGE          |
|---------|-----------------------------------------|-----------------------|
| pwd     | Print the current working directory.    | pwd                   |
|---------|-----------------------------------------|-----------------------|
| ls      | List files and directories.             | ls                    |
|         |                                         | ls -a                 |
|---------|-----------------------------------------|-----------------------|
| cd      | Change directory.                       | cd path/to/directory  |
|         |                                         | cd ..                 |
|---------|-----------------------------------------|-----------------------|
| cp      | Copy files and directories.             | cp file new-file      |
|         |                                         | cp -r dir new-dir     |
|---------|-----------------------------------------|-----------------------|
| mv      | Move files and directories.             | mv file dir           |
|         |                                         | mv file new-file-name |
|---------|-----------------------------------------|-----------------------|
| rm      | Remove files and directories.           | rm file               |
|         |                                         | rm -r dir             |
|---------|-----------------------------------------|-----------------------|
| mkdir   | Create a directory.                     | mkdir dir-name        |
|---------|-----------------------------------------|-----------------------|
| man     | Show manual instructions for a command. | man command           |
|---------|-----------------------------------------|-----------------------|

-------------------------------------------------------------------------------

## Configuring git on Linux/Mac

-   It comes pre-installed (almost surely).
-   git config

```console
$ git config --global user.name "Paul Erdős"
$ git config --global user.email "erdos@impa.br"
$ git config --global core.editor "nano"
```


## Using git on Windows

-   Gitbash
-   Powershell

-------------------------------------------------------------------------------

# Don't panic!

- It's okay to ask for help...

```console
$ git config --help
```


- Too long; didn't read...

```console
$ sudo apt-get install tldr
$ tldr git config 
```

-------------------------------------------------------------------------------

# Setting a local repository


*git init*

- Initialize an empty repository in the current directory.

```console
$ mkdir path/to/the/new/repository
$ cd path/to/the/new/repository
$ git init
```


*git clone*

- Clone a repository in the current directory.

```console
$ cd path/to/where/we/should/put/the/repository
$ git clone https://remote/repository/location/
```


*.gitignore*

- A list with all the files that should be ignored by git.

```console
$ nano .gitignore
```

-------------------------------------------------------------------------------

# Basic commands

*git status*

-  Show your current branch (to be explained in a bit).
-  Display changes since last commit; show staged and unstaged files.
-  It should be your guide for git...

```console
$ git status
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
	modified:   prez.md
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
	deleted:    Presentation/slides.tex
```


*git add*

- Stage a file to be modified in the next commit.

```console
$ git add prez.md
$ git add *
$ git add -f fig1.pdf
```

-------------------------------------------------------------------------------

# Basic commands

*git diff --cached*

- Show differences between staged files and their last committed versions.

```console
$ git diff prez.md
```


*git commit*

- Save a new snapshot of the directory by changing only the staged files.

```console
$ git commit
$ git commit -m "Change slide about basic workflow"
$ git commit -m -a "Add another slide about basic workflow"
```


*git commit --amend*

- Replace the last commit with currently staged changes. 

```console
$ git commit --amend
```

-------------------------------------------------------------------------------

# Basic commands

*git log*

- Shows history of all commits in the local and remote repositories.

```console
$ git log
$ git log --oneline
$ git log --graph
```

- An example of an outcome:

```console
$ git log -10
edb09f0f44990416b04328f5 Make tittle slides
f56a47546af7ba11b3fb6f22 Add more about git in the intro
20b4a76382c513cc785981d7 Building the presentation.
961c5a22dad9c44de5d9c3db Adding prez.md
ad781b84ed0f9d3bfc398b58 Adding presentation.md
2aa803ca5bfc0d73f126a4c8 Merge remote-tracking branch 'origin/master'
e2edec1c75918f5c957b2e78 Adding websites to Ordem.org
c356e98edc3a109656c6f1ec Add slides file
d204c19b067dce9ddb784320 Merge remote-tracking branch 'origin/master'
e902641bc88c296ae4e02264 Adding ordem.org
```

-------------------------------------------------------------------------------

# Working with a remote repository

*git remote*

- Manage remote repositories.

```console
$ git remote add origin remote_url
$ git remote add remote_name https://host-url.com/remote-repository
$ git remote remove origin
```


*git push*

- Send all local commits to the remote repository.

```console
$ git push origin master
$ git push -u origin master
$ git push
```


*git pull*

- Download changes from remote repository and merge it to local repository.
- This possibly will lead to conflicts, which will need to be resolved by
merging (we will show this in a minute).

```console
$ git pull
```

-------------------------------------------------------------------------------

# Basic workflow


  1. Wake up!
  
  2. Update your local repository: *git pull*
  
  3. Make your changes.
  
  4. Check what you changed: *git diff*
  
  5. Prepare the files to be committed: *git add*
  
  6. Check what is going to be committed: *git status*
  
  7. Commit: *git commit*
  
  8. Keep working... Make new changes and go back to step 3.
  
  9. In the end of the day, publish your changes to remote repository: *git push*
  
  10. Go to bed, you deserve some rest :)


-------------------------------------------------------------------------------

# Branching


*git checkout*

- Create or move to another branch.
- Discard unstaged changed files.
- Move directory between previous commits.

```console
$ git checkout -b branch_name
$ git checkout filename.tex
$ git checkout *
$ git checkout b6120565229d4c87fe492ecdff75e8f7e3c8751d
```


*git branch*

- List, create, delete or rename branches.

```console
$ git branch
$ git branch -a
$ git branch branch_name
$ git branch -d branch_to_be_deleted
$ git branch -m old_branch_name new_branch_name
```

-------------------------------------------------------------------------------

# Branching


*git merge*

- Merge a branch onto the current branch.

```console
$ git merge branch_name 
```


*git rebase*

- Reapply commits from one branch on top of another branch. 
- Commonly used to "move" an entire branch to another base, 
  creating copies of the commits in the new location.

```console
$ git checkout branch_name
$ git rebase master
```

-------------------------------------------------------------------------------

# Resolving conflicts

- You get a conflict!

- Check your status: *git status*

- Look for the conflicts in the files.

```console
<<<<<<< HEAD
This text is in the HEAD.
=======
This text is changed in the branch Ramo.
>>>>>>> Ramo
```

- Fix the conflicts.

- Get the changed files ready for commit: *git add*

- Once done: *git commit*

- If you regret it: *git merge --abort*


-------------------------------------------------------------------------------

# Extra useful commands


*git revert*

- Create new commits which reverse the effect of earlier ones.

```console
$ git revert @
$ git revert -n master~2
```


*git reset*

- Undo commits or unstage changes, by resetting the current git HEAD to the specified state.

```console
$ git reset HEAD~
$ git reset --hard HEAD~
```


*git stash*

- Stash local Git changes in a temporary area. 

```console
$ git stash
$ git stash pop
```

-------------------------------------------------------------------------------

# Extra useful commands


*git blame*

- Show commit hash and last author on each line of a file.

```console
$ git blame file
```


*git tag*

- Create, list, delete or verify tags.

```console
$ git tag
$ git tag tag_name
$ git tag tag_name commit_ref
$ git tag -d tag_name
$ git push --tags
$ git fetch --tags
```

-------------------------------------------------------------------------------

# Website and tools

 + [Atlasian](https://www.atlassian.com/git/tutorials)
 
 + [The simple guide](https://rogerdudler.github.io/git-guide/)
 
 + [Git-scm](https://git-scm.com/docs/gittutorial)
 
 + [.gitignore generator](https://www.gitignore.io/)
 
 + [Overleaf](https://www.overleaf.com)
