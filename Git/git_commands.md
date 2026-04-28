# Git Reference Notes

## 1. Git Basics

### `git init`
Used when creating a new project to initialize a Git repository (creates `.git` folder).

> `git init` = convert normal folder into Git repo 📁➡️📦

---

### `git add .`
Used to stage (prepare) changes for commit.

---

### `git commit`
Saves the staged changes as a snapshot in the local repository (in `.git`).

---

### `git push`
Sends local commits to the remote repository.

| If you want to...                  | Use this command                    |
|------------------------------------|-------------------------------------|
| Push specific branch (no link)     | `git push origin feature-login`     |
| Push and "link" for later          | `git push -u origin feature-login`  |
| Push after it's already linked     | `git push`                          |

| Current Branch | Linked To (Upstream) | Command    | Result                          |
|----------------|----------------------|------------|---------------------------------|
| `main`         | `origin/main`        | `git push` | Code goes to main on GitHub.    |
| `branch1`      | `origin/branch1`     | `git push` | Code goes to branch1 on GitHub. |
| `branch2`      | `origin/branch2`     | `git push` | Code goes to branch2 on GitHub. |

---

### `git status`
Shows which files are modified/staged/untracked and also shows on which branch you are currently at.

---

### `git diff`
Shows what all changes made in files before staging.

| Command             | When to use          | What it shows                                                  |
|---------------------|----------------------|----------------------------------------------------------------|
| `git diff`          | Before `git add`     | Changes in your files that are not yet staged.                 |
| `git diff --staged` | After `git add`      | Changes waiting in the "staging area" to be committed.         |
| `git diff HEAD`     | Any time before commit | Everything (staged + unstaged) compared to your last commit. |

> **Note:** `git diff --cached` is exactly the same as `--staged`.

---

### `git show`
Shows the last commit details (by default, the latest).

| Command    | When          | Shows               |
|------------|---------------|---------------------|
| `git diff` | Before commit | Current changes     |
| `git show` | After commit  | Last commit changes |

---

## 2. Git Branching

| Command                               | What it does                          |
|---------------------------------------|---------------------------------------|
| `git checkout -b 'new_branch_name'`   | Creates a new branch and switch to it |
| `git checkout 'branch_name'`          | Switch to another branch              |
| `git branch`                          | List all the branches                 |
| `git branch -d 'branch-name'`         | Deletes local branch                  |
| `git branch -m 'new_name'`            | Rename current branch                 |
| `git branch -m 'old-name' 'new-name'` | Rename some other branch              |

---

## 3. Git Remote

| Command                              | What it does           |
|--------------------------------------|------------------------|
| `git remote rename origin 'new_name'`| Rename remote origin   |
| `git remote -v`                      | Checks repo connection |

---

## 4. Git Merge

> 🧠 Merge conflict occurs when pulling or merging changes from another branch that modified the same file in the same place.

---

## 5. Undoing in Git

### 👉 Undo Staging
Undo the staged files.

```bash
git reset
git reset 'File_Name'
```

### 👉 Undo Commit:

```bash
# Removes the last commit from history and keeps all file changes UNSTAGED.
git reset --mixed HEAD~1
git reset HEAD~1

# Removes the last commit from history and keeps all file changes STAGED.
git reset --soft HEAD~1

# Removes the last commit from history and also deletes all file changes.
git reset --hard HEAD~1
```

> To go to a specific commit, use its commit ID instead of `HEAD~1`.

```bash
# Removes all uncommitted file changes and keeps commit history.
git reset --hard
```

```bash
# Deletes the only (current) commit when no previous commit exists.
# Keeps all file changes safe in working directory.
git update-ref -d HEAD
```

---

## 6. Git History

### `git log`
Shows the full commit history of the current branch.

> `git log` = see all the snapshots you have saved so far 📜

```bash
git log
```

Each entry shows:
- **commit ID** – unique hash for that commit
- **Author** – who made the commit
- **Date** – when it was committed
- **Message** – what the commit was about

---

### `git log --oneline`
Shows the same commit history but in a compact, one-line-per-commit format.

> 👉 Use this when you just want a quick overview — cleaner and easier to read.

```bash
git log --oneline
```

| Command            | What it shows                                          |
|--------------------|--------------------------------------------------------|
| `git log`          | Full details — commit ID, author, date, message        |
| `git log --oneline`| Short commit ID + commit message only (one line each)  |

---

### Useful `git log` Variants

| Command                        | What it does                                          |
|--------------------------------|-------------------------------------------------------|
| `git log -n 5`                 | Shows only the last 5 commits                         |
| `git log --oneline --all`      | Shows all commits from all branches (compact)         |
| `git log --oneline --graph`    | Shows branch structure visually in terminal           |

---

## 7. Git Stashing

### What is Stash?
- Stash is a **temporary storage** in Git.
- When you are working on something but suddenly need to switch branches — you can't commit half-done work.
- So you **stash it** (save it temporarily) and come back to it later.

> 👉 `git stash` = pause your work, save it aside, switch branch, come back and continue 🗂️

---

### `git stash`
Saves your current uncommitted changes temporarily and cleans your working directory.

```bash
git stash
```

---

### `git stash pop`
Brings back the last stashed changes and removes it from the stash list.

```bash
git stash pop
```

---

### `git stash list`
Shows all the stashes you have saved.

```bash
git stash list
```

> Each stash is saved as `stash@{0}`, `stash@{1}`, etc. — latest is always `stash@{0}`.

---

### `git stash apply`
Brings back a stash but **keeps it in the stash list** (does not delete it).

```bash
git stash apply           # applies the latest stash
git stash apply stash@{2} # applies a specific stash
```

---

### `git stash drop`
Deletes a specific stash from the stash list.

```bash
git stash drop            # drops the latest stash
git stash drop stash@{2}  # drops a specific stash
```

---

### `git stash clear`
Deletes **all** stashes at once.

```bash
git stash clear
```

---

### Quick Reference Table

| Command                        | What it does                                         |
|--------------------------------|------------------------------------------------------|
| `git stash`                    | Save current changes temporarily                     |
| `git stash pop`                | Bring back last stash + remove it from list          |
| `git stash list`               | See all saved stashes                                |
| `git stash apply`              | Bring back last stash but keep it in list            |
| `git stash apply stash@{n}`    | Bring back a specific stash                          |
| `git stash drop`               | Delete the latest stash                              |
| `git stash drop stash@{n}`     | Delete a specific stash                              |
| `git stash clear`              | Delete all stashes                                   |




## Questions

**1. What is a PR?**
- Request to merge code.

**2. What is a Fork?**
- A Fork is a copy of someone else's repository into our own GitHub account.

**3. What is Merge?**
- Combining one branch into another (e.g., feature branch into main).