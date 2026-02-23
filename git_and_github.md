# Complete Git and GitHub Tutorial for Beginners
**By Apna College (Shraddha Khapra) | Beginners to Advanced**

> 📎 Official Cheat Sheet: [Google Drive Notes](https://drive.google.com/drive/folders/1wfNTKinBAV6CCxaI5lfSnnRFAYpy0uEl)

---

## Table of Contents
1. [Introduction to Git](#1-introduction-to-git)
2. [Understanding Version Control Systems](#2-understanding-version-control-systems)
3. [Introduction to GitHub](#3-introduction-to-github)
4. [Creating a GitHub Account & Repository](#4-creating-a-github-account--repository)
5. [Setting Up Git Locally (VS Code + Git Bash)](#5-setting-up-git-locally-vs-code--git-bash)
6. [Core Git Commands](#6-core-git-commands)
7. [Git Workflow (Local → Remote)](#7-git-workflow-local--remote)
8. [Branching in Git](#8-branching-in-git)
9. [Merging & Pull Requests](#9-merging--pull-requests)
10. [Resolving Merge Conflicts](#10-resolving-merge-conflicts)
11. [Undoing Changes in Git](#11-undoing-changes-in-git)
12. [Git Stash](#12-git-stash)
13. [Forking & Contributing to Open Source](#13-forking--contributing-to-open-source)
14. [.gitignore](#14-gitignore)
15. [Git Rebase](#15-git-rebase)
16. [Complete Command Reference Cheatsheet](#16-complete-command-reference-cheatsheet)

---

## 1. Introduction to Git

**What is Git?**
- Git is a **Version Control System (VCS)** — it tracks changes in your code over time.
- Every developer, regardless of project size, must know Git.
- Think of it like a **bank statement for your code**: every transaction (change) is recorded.
- Free and open-source.
- Created by **Linus Torvalds** (same person who created Linux).

**Why do you need Git?**
- Without Git, you'd manually save copies of files like `project_v1.zip`, `project_v2_final.zip`, `project_FINAL_REAL.zip` — Git eliminates this mess.
- Allows you to **revert** to any previous state of your code easily.
- Essential for **team collaboration** — multiple people can work on the same project without overwriting each other's work.

---

## 2. Understanding Version Control Systems

**What does a VCS do?**
- Maintains a **complete history** of all changes made to a project.
- Tracks: which files were added, modified, or deleted, and who made the change and when.
- Allows **reverting** to any previous state without losing current work.

**Key Features of Git:**

**1. Tracking Project History**
- Git records every change as a "snapshot" called a **commit**.
- You can go back to any commit at any point in time.
- No need to manually delete old files — Git stores the entire history.

**2. Collaboration**
- Multiple developers can work on the same project on different branches simultaneously.
- Git manages contributions from team members and merges them cleanly.
- Used in every software company — small startups to Google, Microsoft, etc.

---

## 3. Introduction to GitHub

**What is GitHub?**
- **GitHub ≠ Git.** Git is the tool (runs on your computer). GitHub is a cloud **platform** to store and share your Git repositories.
- GitHub is like **Instagram for code**: your repositories are like posts, and others can view, star, and fork them.
- GitHub profiles are widely used in resumes and job applications — recruiters check your GitHub to see your actual project work.

**Key GitHub Concepts:**

| Term | Meaning |
|------|---------|
| **Repository (repo)** | A folder containing your entire project code, tracked by Git |
| **Remote** | The version of your repo stored on GitHub (online) |
| **Local** | The version of your repo on your own laptop/computer |
| **Commit** | A saved snapshot of changes |
| **Branch** | An independent line of development |
| **Pull Request (PR)** | A request to merge one branch into another |
| **Fork** | A personal copy of someone else's repository on your GitHub |

---

## 4. Creating a GitHub Account & Repository

### Creating Your Account
1. Go to [github.com](https://github.com) → click **Sign Up**.
2. Enter your **personal email** (avoid college email — it may expire after graduation).
3. Set a password and choose a unique username.
4. Verify your account via the emailed code.
5. Answer onboarding questions → your **Dashboard** is ready.

### Dashboard Overview
- Shows your **contribution activity** (green squares = days you committed code).
- View your **profile** and **repositories**.
- The dashboard is your home base.

### Creating a New Repository
1. Click **New** in the Repositories section.
2. Give your repo a name (e.g., `college-demo`).
3. Set visibility: **Public** (visible to all) or **Private**.
4. ✅ Check **"Initialize this repository with a README"**.
5. Click **Create Repository**.

### README.md
- A special markdown file that acts as the **homepage/description** of your project.
- Contains: project name, purpose, features, usage instructions, tech stack.
- Always shown at the bottom of the repository page on GitHub.
- Supports **Markdown** formatting (headings, bold, lists, links, images).

```markdown
# My Project
A brief description of the project.

## Features
- Feature 1
- Feature 2

## Author
Your Name
```

### Commits on GitHub
- Every change saved on GitHub is a **commit**.
- The first change is called the **Initial Commit**.
- Think of committing like a **marriage** — it's permanent, recorded, and meaningful.
- Every commit should have a **message** describing what changed and why.

---

## 5. Setting Up Git Locally (VS Code + Git Bash)

### Tools Needed
| Tool | Purpose | Download |
|------|---------|---------|
| **VS Code** | Free, powerful code editor | code.visualstudio.com |
| **Git Bash** (Windows) | Terminal to run Git commands | Bundled with Git for Windows |
| **Terminal** (Mac/Linux) | Pre-installed, no download needed | — |

### Installing Git
- **Windows**: Download from [git-scm.com](https://git-scm.com) → includes Git Bash.
- **Mac**: Usually pre-installed. Confirm with: `git --version`
- **Linux**: `sudo apt install git`

### Verify Installation
```bash
git --version
# Output: git version 2.x.x
```

### Configuring Git (First-Time Setup)
Before using Git, you must tell it who you are. Every commit you make will be tagged with this identity.

```bash
# Set your name (global — applies to all repos on this machine)
git config --global user.name "Your Name"

# Set your email (must match your GitHub email)
git config --global user.email "you@example.com"

# View current config
git config --list
```

**Global vs Local config:**
- `--global`: Applies to ALL repositories on your machine.
- `--local` (or no flag): Applies only to the current repository.

---

## 6. Core Git Commands

### The Three Areas of Git
```
Working Directory  →  Staging Area  →  Local Repository  →  Remote (GitHub)
  (your files)         (git add)         (git commit)         (git push)
```

### Essential Commands

```bash
# Initialize a new Git repo in current folder
git init

# Clone an existing remote repo to your local machine
git clone <repo-url>

# Check the status of files (modified? staged? untracked?)
git status

# Stage a specific file for commit
git add filename.txt

# Stage ALL changed files at once
git add .

# Commit staged changes with a message
git commit -m "your descriptive message here"

# Push local commits to remote repository
git push origin main

# Pull latest changes from remote to local
git pull origin main

# View commit history
git log

# View condensed commit history (one line per commit)
git log --oneline
```

### File Status Lifecycle

```
Untracked → Staged → Committed → Pushed
```

| Status | Meaning |
|--------|---------|
| **Untracked** | New file that Git doesn't know about yet |
| **Modified** | File was previously tracked but has new changes |
| **Staged** | File has been added (`git add`) and is ready to commit |
| **Committed** | Changes saved permanently in local history |
| **Pushed** | Changes uploaded to GitHub |

### Understanding `git status` Output
```bash
git status

# Possible outputs:
On branch main
Your branch is up to date with 'origin/main'
nothing to commit, working tree clean

# Or when files have changed:
Changes not staged for commit:
  modified: index.html

Untracked files:
  style.css
```

---

## 7. Git Workflow (Local → Remote)

### Workflow A — Start from GitHub (Recommended for Beginners)
```
1. Create repo on GitHub (with README)
        ↓
2. Clone it locally: git clone <url>
        ↓
3. Make changes in VS Code
        ↓
4. Stage changes: git add .
        ↓
5. Commit changes: git commit -m "message"
        ↓
6. Push to GitHub: git push origin main
```

### Workflow B — Start Locally (No GitHub repo yet)
```bash
# 1. Create a folder and initialize Git
mkdir my-project
cd my-project
git init

# 2. Create files, write code...

# 3. Stage and commit
git add .
git commit -m "Initial files added"

# 4. Create empty repo on GitHub (DO NOT add README)

# 5. Link local repo to remote
git remote add origin https://github.com/username/repo-name.git

# 6. Verify the link
git remote -v

# 7. Push to GitHub
git push -u origin main
# -u sets upstream — future pushes can just be: git push
```

### Hidden `.git` Folder
```bash
ls -a    # shows hidden files including .git
```
- The `.git` folder is created by `git init` and stores all version history.
- **Never manually edit or delete this folder.**
- If it's present, Git is tracking that directory.

---

## 8. Branching in Git

### What is a Branch?
- A **branch** is an independent copy of the codebase where you can work without affecting the main code.
- Like growing a new branch from a tree trunk — you can experiment, break things, and it won't affect the trunk until you choose to merge.
- Real-world use: different teams (features, bug fixes, testing) each work on their own branch simultaneously.

```
main ────────────────────────────────→
         \                   /
          feature-branch ──→
```

### Default Branch
- The default branch used to be called `master`, now it's called `main`.
- Both are just names — the change happened due to more inclusive naming conventions.

### Branch Commands

```bash
# List all branches (current branch highlighted with *)
git branch

# Create a new branch
git branch feature-login

# Switch to a branch
git checkout feature-login

# Create AND switch in one command (modern way)
git checkout -b feature-login

# Rename current branch
git branch -m new-name

# Delete a branch (safe — won't delete unmerged changes)
git branch -d branch-name

# Force delete a branch
git branch -D branch-name

# Push a branch to GitHub
git push origin feature-login

# Switch back to main
git checkout main
```

### Important Rule
> You cannot delete the branch you are currently on. Switch to another branch first.

### Typical Branching Workflow
```bash
# 1. Create and switch to new branch
git checkout -b feature/user-auth

# 2. Write code, make changes

# 3. Stage and commit
git add .
git commit -m "Add user authentication module"

# 4. Push branch to GitHub
git push origin feature/user-auth

# 5. Create Pull Request on GitHub to merge into main
```

---

## 9. Merging & Pull Requests

### What is Merging?
- **Merging** combines the code from one branch into another.
- Typically: merge `feature-branch` into `main` after the feature is complete.

### Method 1 — Command Line Merge
```bash
# Make sure you're on the branch you want to merge INTO
git checkout main

# Merge the feature branch into current branch
git merge feature-login

# Push the updated main to GitHub
git push origin main
```

### Method 2 — Pull Request (PR) on GitHub ✅ (Used in Teams)
1. Push your feature branch to GitHub: `git push origin feature-branch`
2. Go to your GitHub repo → you'll see a prompt **"Compare & pull request"**.
3. Click it → write a **title and description** for the PR.
4. Click **"Create Pull Request"**.
5. Senior developer / project manager **reviews** the code.
   - They can approve, request changes, or leave comments.
6. If no conflicts → click **"Merge Pull Request"** → **"Confirm Merge"**.
7. The feature branch code is now part of `main`.

### Why PRs are important in real teams:
- Code review catches bugs before they hit production.
- Creates a history of what changed and why.
- Prevents unauthorized or broken code from entering the main branch.
- Every company uses PRs — get comfortable with them.

### Comparing Branches
```bash
# See differences between two branches
git diff branch-name

# See differences between two branches (detailed)
git diff main feature-login
```

### Pulling Remote Changes
After a PR is merged on GitHub, your local `main` is outdated. Sync it:
```bash
git checkout main
git pull origin main    # fetches + merges remote changes into local
```

---

## 10. Resolving Merge Conflicts

### What is a Merge Conflict?
- A **conflict** occurs when **two branches have changed the same lines** in the same file.
- Git can't decide which version to keep automatically — it needs your help.

### When does it happen?
```
main branch:    <p>Hello World</p>
feature branch: <p>Hello Everyone</p>
```
Both changed the same line → conflict on merge.

### Conflict Markers in Code
When a conflict occurs, Git adds markers to the file:
```html
<<<<<<< HEAD (current branch — main)
<p>Hello World</p>
=======
<p>Hello Everyone</p>
>>>>>>> feature-login (incoming branch)
```

### Resolving the Conflict
1. Open the conflicted file in VS Code.
2. VS Code shows options: **"Accept Current Change" | "Accept Incoming Change" | "Accept Both Changes"**.
3. Choose the correct version (or manually write the merged version).
4. **Remove all conflict markers** (`<<<<<<<`, `=======`, `>>>>>>>`).
5. Save the file.
6. Stage and commit the resolution:
```bash
git add .
git commit -m "Resolved merge conflict in index.html"
git push origin main
```

### Pro Tip
- Good branch management and communication within a team **prevents most conflicts**.
- Conflicts are normal — don't panic when they happen.

---

## 11. Undoing Changes in Git

### Scenario 1 — Undo Unstaged Changes (before `git add`)
```bash
# Discard changes to a specific file (restore to last commit)
git checkout -- filename.html

# Discard ALL unstaged changes
git checkout -- .
```

### Scenario 2 — Unstage a File (after `git add`, before `git commit`)
```bash
# Remove a specific file from staging area (keep changes in working dir)
git reset filename.html

# Unstage ALL files
git reset
```

### Scenario 3 — Undo the Last Commit (after `git commit`)
```bash
# Undo last commit but KEEP the changes in working directory (soft reset)
git reset HEAD~1

# Undo last 2 commits
git reset HEAD~2
```

### Scenario 4 — Hard Reset (DANGER — loses changes permanently)
```bash
# Reset to a specific commit — DELETES all changes after that point
git reset --hard <commit-hash>

# Get commit hash from log
git log --oneline
# Example output:
# a1b2c3d (HEAD -> main) Fixed login bug
# f4e5d6c Added user auth
# 9g8h7i6 Initial commit

git reset --hard f4e5d6c   # goes back to this commit, loses everything after
```

> ⚠️ **`--hard` is destructive.** Use only when you're absolutely sure. Prefer `git stash` or `git revert` for safer alternatives.

### Scenario 5 — Revert a Commit (Safe — creates a new "undo" commit)
```bash
# Creates a new commit that undoes the specified commit (safe for shared branches)
git revert <commit-hash>
```

### Reset vs Revert
| | `git reset` | `git revert` |
|---|---|---|
| Effect | Moves HEAD backward (rewrites history) | Creates a new undo commit (preserves history) |
| Safe for shared branches? | ❌ No | ✅ Yes |
| Use case | Local undo | Undoing pushed commits |

---

## 12. Git Stash

### What is Stash?
- **Stash** temporarily saves your uncommitted work so you can switch to another task (like fixing an urgent bug) without committing half-done code.
- Think of it as putting your work-in-progress in a **drawer** to handle later.

### When to Use It
- You're in the middle of a feature when your boss says "fix this bug NOW."
- You need to switch branches but have uncommitted changes.
- You don't want to make a messy commit just to save your work.

### Stash Commands
```bash
# Save current uncommitted changes to stash
git stash

# Save with a descriptive name
git stash save "WIP: adding login form"

# See all stashed entries
git stash list
# Output:
# stash@{0}: WIP: adding login form
# stash@{1}: On main: fixing header

# Apply the most recent stash (keeps it in stash list)
git stash apply

# Apply a specific stash entry
git stash apply stash@{1}

# Apply the most recent stash AND remove it from the list
git stash pop

# Delete a specific stash entry
git stash drop stash@{0}

# Delete ALL stashes
git stash clear
```

### Stash Workflow Example
```bash
# 1. Working on feature, halfway done
git stash save "WIP: half-done login form"

# 2. Switch to main, fix the urgent bug
git checkout main
# ... fix bug ...
git add . && git commit -m "Hotfix: login redirect bug"

# 3. Return to feature branch, restore your work
git checkout feature-login
git stash pop
# Continue working on the feature
```

---

## 13. Forking & Contributing to Open Source

### What is Forking?
- **Fork**: Creates your own **personal copy** of someone else's repository on your GitHub account.
- Unlike cloning (which just downloads to your computer), forking gives you your own remote copy you can push to.
- The original repo is called the **upstream** repository.

### Fork vs Clone
| | Fork | Clone |
|---|---|---|
| Location | GitHub (remote copy on your account) | Local (on your computer) |
| Can push changes? | ✅ Yes (to your fork) | Only if you have write access |
| Use case | Contributing to open source | Working on your own or team projects |

### Open Source Contribution Workflow
```
1. Find a repo you want to contribute to

2. Fork it → Your GitHub now has a copy

3. Clone YOUR fork locally
   git clone https://github.com/YOUR-USERNAME/repo-name.git

4. Create a feature branch
   git checkout -b fix/typo-in-readme

5. Make changes, stage, and commit
   git add . && git commit -m "Fix typo in README"

6. Push to YOUR fork
   git push origin fix/typo-in-readme

7. Go to GitHub → create a Pull Request from your fork
   to the ORIGINAL (upstream) repository

8. Project maintainer reviews and merges your PR
```

### Keeping Your Fork Updated
```bash
# Add original repo as a remote called "upstream"
git remote add upstream https://github.com/original-owner/repo.git

# Fetch latest changes from upstream
git fetch upstream

# Merge upstream's main into your local main
git merge upstream/main

# Push updated main to your fork
git push origin main
```

---

## 14. .gitignore

### What is .gitignore?
- A file that tells Git to **ignore specific files or folders** — they won't be tracked or pushed.
- Critical for keeping secrets (API keys, passwords) and unnecessary files out of your repo.

### Common Files to Ignore
- `node_modules/` — thousands of dependency files (can be regenerated from `package.json`)
- `.env` — environment variables (passwords, API keys)
- `*.log` — log files
- `.DS_Store` — macOS system files
- `build/` or `dist/` — compiled output files
- `*.pyc` — Python compiled files

### Creating a .gitignore File
Create a file named `.gitignore` in the root of your project:

```gitignore
# Dependencies
node_modules/
vendor/

# Environment variables — NEVER commit this
.env
.env.local
.env.production

# Build output
build/
dist/
*.min.js

# OS files
.DS_Store
Thumbs.db

# Log files
*.log
logs/

# IDE files
.vscode/
.idea/

# Python
__pycache__/
*.pyc
*.pyo

# Java
*.class
*.jar
```

### Important Note
- If a file was **already committed** to Git, adding it to `.gitignore` won't retroactively hide it. You'd need to remove it from tracking:
```bash
git rm --cached filename.env
git commit -m "Remove .env from tracking"
```

### GitHub's .gitignore Templates
When creating a new repository on GitHub, you can choose a language-specific `.gitignore` template (Node, Python, Java, etc.) that handles the common cases automatically.

---

## 15. Git Rebase

### What is Rebase?
- **Rebase** moves or replays your commits on top of another branch's latest commit.
- Result: a **cleaner, linear commit history** (no messy merge commits).
- Alternative to `git merge`.

### Merge vs Rebase

```
--- MERGE ---
main:    A → B → C → → → → → M  (merge commit)
                  \          /
feature:           D → E → F

--- REBASE ---
main:    A → B → C → D' → E' → F'   (linear, no merge commit)
```

```bash
# From your feature branch, rebase onto main
git checkout feature-login
git rebase main

# This replays your feature commits on top of the latest main
```

### Interactive Rebase (Advanced)
```bash
# Rebase and edit last 3 commits
git rebase -i HEAD~3

# Opens editor with options:
# pick   — keep the commit
# squash — combine with previous commit
# reword — change commit message
# drop   — delete the commit
```

### ⚠️ Golden Rule of Rebase
> **Never rebase a branch that others are working on** (i.e., never rebase public/shared branches like `main`). Rebase rewrites history, which will break other developers' local copies.
> Use rebase only on **your own local feature branches** before merging.

---

## 16. Complete Command Reference Cheatsheet

### Setup
```bash
git config --global user.name "Name"
git config --global user.email "email@example.com"
git config --list
```

### Repository Operations
```bash
git init                         # initialize new repo
git clone <url>                  # clone remote repo locally
git remote add origin <url>      # link local repo to remote
git remote -v                    # view remote URLs
```

### Day-to-Day Workflow
```bash
git status                       # check file statuses
git add <file>                   # stage specific file
git add .                        # stage all changes
git commit -m "message"          # commit with message
git push origin main             # push to remote
git pull origin main             # pull from remote
git log --oneline                # view commit history
```

### Branching
```bash
git branch                       # list branches
git branch <name>                # create branch
git checkout <name>              # switch branch
git checkout -b <name>           # create + switch
git branch -m <new-name>         # rename branch
git branch -d <name>             # delete branch
git push origin <branch>         # push branch to remote
```

### Merging
```bash
git merge <branch>               # merge branch into current
git diff <branch>                # compare branches
git pull origin main             # pull + merge remote into local
```

### Undoing
```bash
git checkout -- <file>           # discard unstaged changes
git reset <file>                 # unstage file
git reset HEAD~1                 # undo last commit (keep changes)
git reset --hard <hash>          # undo to commit (DESTRUCTIVE)
git revert <hash>                # safe undo (creates new commit)
```

### Stash
```bash
git stash                        # stash current work
git stash list                   # list all stashes
git stash pop                    # apply + remove latest stash
git stash apply                  # apply latest stash (keep in list)
git stash drop stash@{0}         # delete specific stash
git stash clear                  # delete all stashes
```

### Advanced
```bash
git rebase main                  # rebase current branch onto main
git rebase -i HEAD~3             # interactive rebase (last 3 commits)
git cherry-pick <hash>           # apply a specific commit to current branch
git rm --cached <file>           # stop tracking a file (keep locally)
ls -a                            # show hidden files (including .git)
```

---

## Key Mental Models

**Git = A Time Machine for Your Code**
Every `commit` is a checkpoint you can return to. You're never "stuck" with a bad state.

**The Staging Area = Your Preparation Zone**
`git add` = "I want to include this in my next commit"
`git commit` = "Lock in everything I staged"

**Branches = Parallel Universes**
Each branch is an independent timeline. Merge them when ready.

**GitHub = The Cloud Backup + Collaboration Layer**
Your local Git repo is your private copy. GitHub is the shared version that your team syncs with.

---

*Notes compiled from: Complete Git and GitHub Tutorial for Beginners by Apna College (Shraddha Khapra)*