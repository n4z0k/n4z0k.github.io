---
author: Alfeze
created: 2026-08-25
---

# Git Basics


>**Git** is a **Distributed Version Control System (DVCS)** used to track changes to files, maintain a complete history of a project, and enable collaboration through branches, commits, merges, and remote repositories.

---
## How Git works

![[How_Git_Works.png|542]]

![[Git_Workflow.png|541]]
## How to Install Git

To install Git, go to the official Git website [Git](https://git-scm.com/), download the appropriate version for your operating system, and follow the installation instructions.

---
## Verify Git Installation

To verify that Git is installed correctly

```bash
git --version
```

## Git Help

Display Git's built-in help and the most commonly used Git commands.

```bash
git --help
```

## Initialize a Git Repository

To initialize a Git repository, navigate to your project directory and run the following command:

```bash
git init
```

This command creates a hidden `.git` directory that contains the repository metadata and version history.

## Git Configuration Help

Display the documentation for the `git config` command : 

```bash
git --help config
```

## Configure Git Identity

Set the username associated with your Git commits:

```bash
git config --global user.name "Name"
```

Set the email address associated with your Git commits:

```bash
git config --blobal user.email "your-email@example.com"
```

>[!NOTE]
>The  `--global` option applies the configuration to all Git repositories for the current user.

## Display Git Configuration 

Display the current Git configuration:

```bash
git config --list
```

This can be used to verify settings such as your username, email address, and other Git options.

## Check Repository Status

Display the current state of the working directory and staging area:

 ```bash
 git status
 ```

The command show tracked, modified, staged, and untracked files in the current repository.

## Stage Changes

Add a specific files to the staging area:

 ```bash
 git add <file>
 ```


Add all changes in the current directory and its subdirectories to the staging area: 

```bash
git add .
```

>[!NOTE]
>`git add` does not create a commit. It only stages changes for the next commit. 

Add all changes across the entire repository to the staging area:

```bash
git add --all
```

Short form : 

```bash
git add -A
```

>[!TIP]
>`git add .` stages changes from the current directory and its subdirectories, while `git add --all` stages changes across the entire repository.

## Commit Changes

Create new commit containing the changes currently stored in the staging area: 

```bash
git commit -m "<message>"
```


The `-m` option allow you to specify the commit message directly from the command line. 

>[!TIP]
>Use short and descriptive commit messages that clearly explain what changed.

## Commit Tracked Changes

Automatically stage all modified and deleted **tracked files**, then create a commit:

```bash
git commit -a -m "<message>"
```


The `-a` option automatically stages modified and deleted files that are already tracked by Git before creating the commit. 

>[!WARNING]
>`git commit -a` does **not** include new untracked files. New files must first be added with `git add`.
>

## View Commit History

Display the commit history of the current repository: 

```bash
git log
```

Display a compact version of the commit history:

```bash
git log --oneline
```

Each commit contains information such as the commit hash, author, date, and commit message.

>[!TIP]
>`git log --oneline` is useful for quickly viewing the repository history.

Display only the last `<n>`commits :

```bash
git log -n <number>
```

Display the commit history of a specific file, including the changes introduced by each commit:

```bash
git log -p <file>
```


>[!TIP]
>The `-p` option displays the patch (diff) introduced by each commit

## View Changes

Display the differences between the working directory and the staging area : 

```bash
git diff
```

This command shows the changes made to tracked files that have **not yet been staged**.

>[!TIPS]
>Use `git diff` before `git add` to review your changes, and `git diff --staged` before `git commit` to review what wil be included in the next commit.


---
## Cheatsheet

![[Git_CheatSheet.jpg]]

---

# Git Branches

> Essential Git commands to create, switch, and merge branches for parallel development.
## Create a Branch

Create a new branch from the current commit:

```bash
git branch <branch-name>
```

> [!WARNING]
> Creating a branch does **not** switch to it. You remain on your current branch until you explicitly switch.

---
## Switch to a Branch

Switch the working directory to another branch:

```bash
git switch <branch-name>
```

Legacy equivalent:

```bash
git checkout <branch-name>
```

> [!TIP]
> Prefer `git switch` for changing branches. It is more explicit than the older `git checkout` syntax, which also handles file restoration and detached HEAD.

---
## Create and Switch in One Step

```bash
git switch -c <branch-name>
```

Legacy equivalent:

```bash
git checkout -b <branch-name>
```

---
## List Branches

List local branches:

```bash
git branch
```

List remote branches:

```bash
git branch -r
```

List all branches (local and remote):

```bash
git branch -a
```

---
## Merge a Branch

Merge combines the changes from one branch into another.

Step 1 — switch to the branch that will **receive** the merge:

```bash
git switch main
```

Step 2 — merge the other branch into it:

```bash
git merge <branch-name>
```

> [!NOTE]
> If both branches modified the same lines of the same file, Git cannot merge automatically and a **merge conflict** occurs. Git marks the conflicting sections in the file for manual resolution.

---
## Rename a Branch

Rename the current local branch:

```bash
git branch -m <new-name>
```


> [!NOTE] 
> > Use `-M` (uppercase, `--move --force`) instead of `-m` if a branch with the target name already exists and you want to overwrite it. This is the standard command used to rename `master` into `main`.
> > 
> 

```bash
git branch -M main
```

---
## Delete a Branch

Delete a local branch (only if already merged):

```bash
git branch -d <branch-name>
```

Force delete a local branch (even if not merged):

```bash
git branch -D <branch-name>
```

> [!WARNING]
> `-D` deletes the branch even if its changes were never merged anywhere else. Any unmerged commits become unreachable.

---
## Quick Reference
| Command                | Purpose                             |
| ---------------------- | ----------------------------------- |
| `git branch <name>`    | Create a branch                     |
| `git switch <name>`    | Switch to a branch                  |
| `git switch -c <name>` | Create and switch in one step       |
| `git branch`           | List local branches                 |
| `git merge <name>`     | Merge a branch into the current one |
| `git branch -d <name>` | Delete a merged branch              |

---
# Git Remote

> Essential Git commands to connect a local repository to a remote server (GitLab, GitHub) and synchronize changes.

## Clone an Existing Repository

Create a local copy of a remote repository:

```bash
git clone <repository-url>
```

> [!NOTE]
> Cloning automatically sets up a remote named `origin` pointing to the source repository.

---
## Link a Local Repository to a Remote

If you initialized your repository locally with `git init`, connect it to a remote:

```bash
git remote add origin <repository-url>
```

Verify the configured remote:

```bash
git remote -v
```

---
## Push Changes to the Remote

Push and set the branch to track the remote branch (only needed the first time):

```bash
git push -u origin <branch-name>
```

> [!TIP]
> After using `-u` once, you can simply run `git push` for that branch — Git remembers which remote branch to push to.

---
## Pull Changes from the Remote

Download and merge changes from the remote into your current branch:

```bash
git pull origin <branch-name>
```

Download and rebase instead of merge (keeps a cleaner, linear history):

```bash
git pull origin <branch-name> --rebase
```

> [!WARNING]
> If you have uncommitted local changes, `git pull` may fail or create conflicts. Commit or stash your work first.

---
## Fetch Without Merging

Download remote changes without merging them into your current branch:

```bash
git fetch origin
```

> [!NOTE]
> `git fetch` only updates your knowledge of the remote (e.g. `origin/main`). It does **not** touch your working directory or current branch. Use `git pull` when you want to fetch and merge in one step.

---
## Change the Remote URL

```bash
git remote set-url origin <new-url>
```

---
## Quick Reference
| Command                              | Purpose                                        |
| -------------------------------------- | ------------------------------------------------ |
| `git clone <url>`                     | Copy a remote repository locally                |
| `git remote add origin <url>`         | Link a local repository to a remote             |
| `git push -u origin <branch>`         | Upload commits (first time, sets tracking)      |
| `git pull origin <branch>`            | Download and merge remote changes               |
| `git fetch origin`                    | Download remote changes without merging         |

---
# Git Reset 

> Essential Git commands to undo commits by moving HEAD and the branch pointer backward.

## How Reset Differs from Restore

`git restore` operates on **files**. `git reset` operates on **commits** — it moves the current branch pointer (and HEAD) to a different commit.

> [!NOTE]
> Use `git restore` to undo changes to a specific file. Use `git reset` to undo one or more commits entirely.

---
## Soft Reset

Move the branch pointer to a previous commit, keeping all changes staged:

```bash
git reset --soft HEAD~1
```

> [!TIP]
> Useful when you want to redo a commit — for example, to combine it with more changes or write a better commit message. Your work is preserved in the Staging Area, ready to commit again.

---
## Mixed Reset (default)

Move the branch pointer back and unstage the changes, but keep them in the working directory:

```bash
git reset HEAD~1
```

or explicitly:

```bash
git reset --mixed HEAD~1
```

> [!NOTE]
> This is the default mode of `git reset` when no option is specified. Your file modifications are kept, but you will need to `git add` them again before committing.

---
## Hard Reset

Move the branch pointer back and discard all changes completely:

```bash
git reset --hard HEAD~1
```

> [!WARNING]
> This permanently discards commits and all uncommitted changes in the Working Directory and Staging Area. This action cannot be undone through normal Git commands. Use with extreme caution.

---
## Resetting to a Specific Commit

All the commands above also work with a specific commit ID instead of `HEAD~1`:

```bash
git reset --hard <commit-id>
```

Find the target commit first:

```bash
git log --oneline
```

---
## Quick Reference
| Command                          | Working Directory | Staging Area | Commit History |
| ----------------------------------- | -------------------- | --------------- | ----------------- |
| `git reset --soft HEAD~1`         | unchanged            | unchanged        | moved back        |
| `git reset --mixed HEAD~1`        | unchanged            | reset            | moved back        |
| `git reset --hard HEAD~1`         | reset                | reset            | moved back        |

---
# Git Stash 

 >Temporarily save uncommitted changes without creating a commit, so you can switch context and come back to them later.

## Why Use Stash

If you need to switch branches but your current changes aren't ready to commit, `git stash` lets you set them aside temporarily and restore them later.

---
## Save Changes to the Stash

Stash all tracked, modified changes (staged and unstaged):

```bash
git stash
```

Stash with a descriptive message:

```bash
git stash save "<message>"
```

> [!NOTE]
> By default, `git stash` does **not** include untracked or ignored files.

Include untracked files:

```bash
git stash -u
```

---
## List Stashed Changes

```bash
git stash list
```

> [!TIP]
> Each stash is labeled like `stash@{0}`, `stash@{1}`, etc. The most recent stash is always `stash@{0}`.

---
## Apply a Stash

Reapply the most recent stash, keeping it in the stash list:

```bash
git stash apply
```

Apply a specific stash:

```bash
git stash apply stash@{1}
```

---
## Apply and Remove a Stash

Reapply the most recent stash and remove it from the stash list:

```bash
git stash pop
```

> [!WARNING]
> If applying the stash causes a conflict, the stash is **not** removed from the list, so you don't lose it. Resolve the conflict, then remove it manually with `git stash drop`.

---
## Remove a Stash

Delete a specific stash without applying it:

```bash
git stash drop stash@{0}
```

Delete all stashes:

```bash
git stash clear
```

---
## Quick Reference
| Command                     | Purpose                                       |
| ------------------------------ | ------------------------------------------------ |
| `git stash`                   | Save current changes temporarily                |
| `git stash list`              | Show all saved stashes                          |
| `git stash apply`             | Reapply the latest stash, keep it in the list   |
| `git stash pop`               | Reapply the latest stash, remove it from the list |
| `git stash drop`              | Delete a specific stash                         |

---

# Git Undo and Recovery

> Essential Git commands to discard changes, unstage files, and restore previous versions.

## Discard Unstaged Changes

Discard unstaged modifications made to a tracked file:

```bash
git restore <file>
```

With this command, Git restores the file in the **Working Directory** using the version currently stored in the **Staging Area**.

>[!WARNING]
>The unstaged modifications are discarded. For example, if **Version A** is staged and you modify the file into **Version B** without running `git add`, this command replaces Version B with Version A. 

---
## Unstage Changes

Remove a file from the **Staging Area** while keeping its modifications in the **Working Directory**: 

```bash
git restore --staged <file>
```

This is useful when a file was accidentally staged with `git add`.

> [!NOTE] 
> This essentially **undoes `git add`**. If **Version B** was staged accidentally, it is removed from the Staging Area but Version B remains unchanged in the Working Directory.

---
## Restore a Previous Version

Display the commit history of a specific file:

```bash
git log --oneline -- <file>
```

To also display the changes introduced by each commit:

```bash 
git log --oneline -p -- <file> 
```

Identify the commit ID containing the version you want to restore.

Restore the file from the selected commit: 

```bash 
git restore --source=<commit-id> <file> 
```

> [!NOTE] 
> This restores the selected version into the **Working Directory** without switching branches or moving `HEAD`.

Review the restored version:

```bash 
git diff 
```

If you want to keep it, stage and commit it normally: 

```bash 
git add <file> git commit -m "Restore previous version" 
```

---
## Legacy `git checkout`

Older Git documentation may use `git checkout` to restore a file from a previous commit:

```bash 
git checkout <commit-id> -- <file> 
```

Modern equivalent: 

```bash 
git restore --source=<commit-id> <file>
```

> [!TIP] 
> Prefer `git restore` for restoring files. It is more explicit than the older `git checkout` syntax.

---
## Quick Reference 

| Command                            | Purpose                                    |
| ---------------------------------- | ------------------------------------------ |
| `git restore <file>`               | Discard unstaged changes                   |
| `git restore --staged <file>`      | Undo `git add` while keeping modifications |
| `git log --oneline -- <file>`      | Find previous versions of a file           |
| `git restore --source=<id> <file>` | Restore a file from a previous commit      |

---
# Gitignore 

> A `.gitignore` file specifies files and directories that Git should intentionally ignore and not track.

## Basic Syntax

Ignore a specific file:

```gitignore
config.txt
```

Ignore a directory : 

```gitignore
node_modules/
```

Ignore all files with a specific extension : 

```gitignore
.log*
```

## Example

```gitignore
# Dependencies 
node_modules/ 

# Environment files 
.env 

# Logs 
*.log 

# Operating system files 
.DS_Store 
Thumbs.db
```

>[!IMPORTANT]
>`gitignore` does not automatically stop tracking files that have already been commited to the repository.

## Ignore an Already Tracked File

Adding a tracked file to `.gitignore` **does not automatically stop Git from tracking it**.

Remove the file from the Git index while keeping it in the working directory: 

```bash
git rm --cached <file>
```

For a directory : 

```bash
git rm -r --cached <directory>
```

Then add the file or directory to `gitignore` and commit the changes.

>[!WARNING]
>The `--cached` option removes the file only from Git's index. The local file is preserved.

---
## Related

- [[MOC_Development|Development]]







