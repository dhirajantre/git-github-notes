# Git and GitHub Notes

My personal learning notes on Git and GitHub commands. All commands are in code blocks, so they are easy to copy. Replace the parts inside `< >` with your own values. These are learning notes, so they may contain mistakes.

## Contents

1. [Git: Basics](#1-git-basics)
2. [Git: Compare Changes](#2-git-compare-changes)
3. [Git: Undo Changes](#3-git-undo-changes)
4. [Git: Branches](#4-git-branches)
5. [Git: Stash](#5-git-stash)
6. [.gitignore](#6-gitignore)
7. [GitHub: Remotes, Push and Pull](#7-github-remotes-push-and-pull)

## 1. Git: Basics

Git has three main areas. Your files move through them like this:

```text
+-------------------+
| Working Directory |
+-------------------+
          |
          |  git add
          v
+-------------------+
|   Staging Area    |
+-------------------+
          |
          |  git commit
          v
+-------------------+
|  Git Repository   |
+-------------------+
```

- **Working Directory**: the project folder on your computer where your code files are. You create and edit files here.
- **Staging Area**: the files you added with `git add`. They are ready to commit, but not committed yet.
- **Git Repository**: where Git stores your commits (inside the hidden `.git` folder, in Git's object database).

```bash
git init                         # create a new Git repository in the current folder
git status                       # show modified, staged and untracked files
git add .                        # stage all changes in the current folder
git commit -m "message"          # save the staged changes as a commit with a message
git log                          # show past commits
git log --oneline                # show each commit in one line (short hash + message)
```

## 2. Git: Compare Changes

`HEAD` is the latest commit of the current branch, and `HEAD~1` is the commit before it.

```bash
git diff                         # show unstaged changes (working directory vs staging area)
git diff --staged                # show staged changes (staging area vs last commit)
git diff HEAD~1 HEAD             # show changes between the previous commit and the latest commit
```

## 3. Git: Undo Changes

### Discard changes and unstage files

```bash
git restore <filename>           # discard the unstaged changes of one file
git restore .                    # discard the unstaged changes of all files in the current folder
git restore --staged <filename>  # unstage a file (its changes stay in the working directory)
git checkout -f                  # discard all local changes (staged and unstaged) of tracked files
```

Warning: discarded changes cannot be recovered.

### Undo the last commit with git reset

The three options decide what happens to the changes of the commit you undo:

```bash
git reset --soft HEAD~1          # undo the last commit, the changes stay staged
git reset HEAD~1                 # undo the last commit, the changes stay in the working directory (unstaged)
git reset --hard HEAD~1          # undo the last commit and delete its changes completely
```

Warning: `--hard` permanently deletes your changes, so use it carefully. Do not reset commits that you have already pushed and shared with others.

## 4. Git: Branches

### Check, create and switch branches

```bash
git branch                       # list all local branches (the current branch is marked with *)
git branch --show-current        # show only the name of the current branch
git branch <branch-name>         # create a new branch
git switch <branch-name>         # switch to another branch
git switch -c <branch-name>      # create a new branch and switch to it
```

### Merge a branch

To merge a branch into `main`, you must first switch to the `main` branch.

```bash
git switch main
git merge <branch-name>          # merge <branch-name> into the current branch
```

### Delete a branch

```bash
git branch -d <branch-name>      # delete a branch (works only if it is already merged)
git branch -D <branch-name>      # force delete a branch (even if it is not merged)
```

### Rename a branch

```bash
git branch -m <new-name>                # rename the current branch
git branch -m <old-name> <new-name>     # rename another branch
```

### List merged and unmerged branches

```bash
git branch --merged              # branches that are already merged into the current branch
git branch --no-merged           # branches that are not merged yet
```

## 5. Git: Stash

`git stash` temporarily saves your uncommitted changes and gives you a clean working tree. Untracked files are not stashed unless you use `-u`.

```bash
git stash                        # stash the current changes
git stash push -m "message"      # stash with a message
git stash -u                     # include untracked files
git stash list                   # list all stashes
git stash pop                    # apply the latest stash and remove it from the stash list
git stash apply                  # apply the latest stash (it stays in the stash list)
git stash apply stash@{2}        # apply a specific stash
git stash drop                   # delete the latest stash
git stash drop stash@{1}         # delete a specific stash
git stash clear                  # delete all stashes
```

## 6. .gitignore

`.gitignore` is a file that contains file names and patterns that Git should ignore, which means Git will not track them.

## 7. GitHub: Remotes, Push and Pull

`origin` is the default short name (alias) for the URL of the remote repository.

### Push your code to GitHub for the first time

```bash
git remote add origin https://github.com/<username>/<repo-name>.git
git branch -M main
git push -u origin main
```

- `git remote add origin <url>` saves the repository URL with the short name `origin`.
- `git branch -M main` renames the current branch (usually `master`) to `main`, because GitHub uses `main` as the default branch name.
- `git push -u origin main` is needed only the first time. The `-u` option links your local `main` branch with `origin/main`, so after this you can just run `git push` and `git pull`.

### Push later changes

```bash
git push
```

### Check remotes and remote branches

```bash
git remote -v                    # show all remotes with their URLs
git branch -r                    # list all remote branches
```

### Get code from GitHub

```bash
git clone <repo-url>             # copy a repository from GitHub to your computer
git pull                         # fetch the changes and merge them into your local branch
git fetch --all                  # fetch changes from all remotes (does not merge)
```

### Delete a remote branch

```bash
git push origin --delete <branch-name>
```
