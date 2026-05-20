# Git — Beyond Basics

Git is not “just commands”.

Git is fundamentally:

```text
A content tracking system
+
A graph database
+
A distributed version control system
+
A history rewriting tool
+
A collaboration protocol
```

Most beginners learn:

```bash
git add
git commit
git push
git pull
```

But real-world Git mastery is about understanding:

* Working tree
* Staging area
* Commit graph
* References (refs)
* HEAD
* Branch pointers
* Remote tracking
* Rebasing
* History rewriting
* Conflict resolution
* Recovery mechanisms
* Safe collaboration practices

---

# PART 1 — THE CORE MENTAL MODEL

Before commands, understand this:

---

# The 3 Git Areas

```text
1. Working Directory
   ↓
2. Staging Area (Index)
   ↓
3. Repository (.git)
```

---

## 1. Working Directory

Actual files you edit.

Example:

```bash
index.js
app.css
README.md
```

You modify them normally.

---

## 2. Staging Area (Index)

A “preparation area”.

You choose WHAT goes into the next commit.

Example:

```bash
git add app.js
```

Only app.js is staged.

---

## 3. Repository

Permanent commit history.

```bash
git commit -m "Add login feature"
```

Now history contains the snapshot.

---

# The MOST IMPORTANT Git Mental Model

Git stores:

```text
Snapshots
NOT diffs
```

Each commit is a complete snapshot reference.

---

# HEAD

HEAD means:

```text
Current checked out commit/branch
```

Example:

```bash
HEAD -> main
```

---

# Branches Are Just Pointers

This surprises many people.

A branch is NOT a folder.

A branch is:

```text
A movable pointer to a commit
```

Example:

```text
A --- B --- C (main)
             ^
            HEAD
```

New commit:

```text
A --- B --- C --- D (main)
                   ^
                  HEAD
```

---

# PART 2 — COMPLETE GIT COMMAND SHEET

# Repository Initialization

| Command     | Purpose          | Common Variations             |
| ----------- | ---------------- | ----------------------------- |
| `git init`  | Initialize repo  | `git init`, `git init <name>` |
| `git clone` | Copy remote repo | `git clone <url>`             |

---

# File Tracking

| Command       | Purpose              | Variations             |
| ------------- | -------------------- | ---------------------- |
| `git status`  | Show changes         | `-s`, `--short`, `-b`  |
| `git add`     | Stage files          | `.`, `-A`, `-p`        |
| `git restore` | Restore files        | `--staged`, `--source` |
| `git rm`      | Remove tracked files | `-r`, `--cached`       |
| `git mv`      | Rename/move files    | standard               |

---

# Commits

| Command      | Purpose         | Variations                       |
| ------------ | --------------- | -------------------------------- |
| `git commit` | Create commit   | `-m`, `--amend`                  |
| `git log`    | View history    | `--oneline`, `--graph`, `--stat` |
| `git show`   | Inspect commit  | `<hash>`, `HEAD~1`               |
| `git diff`   | Compare changes | `--staged`, branch compare       |

---

# Branching

| Command        | Purpose            |
| -------------- | ------------------ |
| `git branch`   | Manage branches    |
| `git switch`   | Change branches    |
| `git checkout` | Old switch/restore |
| `git merge`    | Combine branches   |
| `git rebase`   | Reapply commits    |

---

# Remote Operations

| Command      | Purpose                 |
| ------------ | ----------------------- |
| `git remote` | Manage remotes          |
| `git fetch`  | Download remote changes |
| `git pull`   | Fetch + merge/rebase    |
| `git push`   | Upload commits          |

---

# Undo / Recovery

| Command      | Purpose               |
| ------------ | --------------------- |
| `git reset`  | Move HEAD/index       |
| `git revert` | Reverse commit safely |
| `git reflog` | Recover lost commits  |
| `git stash`  | Temporary save        |

---

# Inspection

| Command      | Purpose          |
| ------------ | ---------------- |
| `git blame`  | See line authors |
| `git bisect` | Find bad commit  |
| `git tag`    | Create versions  |

---

# PART 3 — EVERY IMPORTANT COMMAND IN DEPTH

# 1. git init

## Purpose

Creates a Git repository.

---

## Example

```bash
git init
```

Creates:

```text
.git/
```

This folder stores all Git internals.

---

## Variations

### Initialize named repo

```bash
git init my-project
```

---

# Bad Practices

## ❌ Initializing inside another repo accidentally

This causes nested repositories.

Problem:

```text
Outer repo ignores inner repo history
```

---

# 2. git clone

## Purpose

Download existing repository.

---

## Example

```bash
git clone https://github.com/user/project.git
```

---

## Variations

### Clone specific folder name

```bash
git clone <url> my-folder
```

---

### Shallow clone

```bash
git clone --depth=1 <url>
```

Useful for:

* CI/CD
* Huge repos

Bad for:

* full history analysis

---

# Issues

## ❌ Using shallow clone for development

You may later fail during:

* rebases
* history lookups
* blame

---

# 3. git status

## Purpose

Shows current repo state.

---

## Example

```bash
git status
```

---

## Short format

```bash
git status -s
```

Output:

```text
M app.js
?? newfile.js
```

Meaning:

| Symbol | Meaning   |
| ------ | --------- |
| M      | Modified  |
| ??     | Untracked |
| A      | Added     |
| D      | Deleted   |

---

# 4. git add

MOST misunderstood command.

---

## Purpose

Stage content.

NOT “save file”.

---

## Examples

### Add single file

```bash
git add app.js
```

---

### Add all tracked/untracked files

```bash
git add .
```

---

### Add EVERYTHING including deletions

```bash
git add -A
```

Difference:

| Command      | Behavior               |
| ------------ | ---------------------- |
| `git add .`  | Current directory only |
| `git add -A` | Entire repo            |

---

## Interactive staging

### One of the MOST important advanced features

```bash
git add -p
```

Lets you stage chunks interactively.

Example:

```text
y = stage
n = skip
```

Extremely useful for:

* clean commits
* separating unrelated changes

---

# Bad Practice

## ❌ Huge mixed commits

Bad:

```text
"fix stuff"
```

Containing:

* bug fix
* formatting
* feature
* refactor

Use:

```bash
git add -p
```

---

# 5. git commit

## Purpose

Create snapshot.

---

## Example

```bash
git commit -m "Add authentication middleware"
```

---

# Good Commit Message Pattern

```text
<type>: <summary>

Examples:
feat: add login API
fix: handle null user response
refactor: simplify auth logic
```

---

## Amend Last Commit

```bash
git commit --amend
```

Used for:

* fixing commit message
* adding forgotten files

---

# DANGER

## ❌ Amending pushed commits

This rewrites history.

Can break teammates.

---

# 6. git log

## Purpose

View history.

---

## Best Practical Variations

---

### Compact

```bash
git log --oneline
```

---

### Graph visualization

```bash
git log --oneline --graph --all
```

One of the most important commands.

---

### File changes

```bash
git log --stat
```

---

### Show patch

```bash
git log -p
```

---

# 7. git diff

## Purpose

View changes.

---

# Important Variations

---

## Working vs staged

```bash
git diff
```

---

## Staged vs last commit

```bash
git diff --staged
```

---

## Compare branches

```bash
git diff main feature-auth
```

---

# Common Confusion

People think:

```bash
git diff
```

shows everything.

It DOES NOT.

It only shows unstaged changes.

---

# 8. git branch

## List branches

```bash
git branch
```

---

## Create branch

```bash
git branch feature/login
```

---

## Delete branch

```bash
git branch -d feature/login
```

---

## Force delete

```bash
git branch -D feature/login
```

Dangerous if unmerged.

---

# Naming Best Practices

Good:

```text
feature/auth
bugfix/payment-timeout
hotfix/prod-crash
```

Bad:

```text
new
test
final-final
```

---

# 9. git switch

Modern alternative to checkout.

---

## Switch branch

```bash
git switch main
```

---

## Create and switch

```bash
git switch -c feature/navbar
```

---

# 10. git checkout

Old multipurpose command.

---

## Checkout branch

```bash
git checkout dev
```

---

## Restore file

```bash
git checkout app.js
```

Danger:

* overwrites changes

---

# Why checkout became problematic

Because it did TOO MANY THINGS:

* switch branches
* restore files
* detach HEAD

Modern Git split it into:

* switch
* restore

---

# 11. git merge

## Purpose

Combine histories.

---

# Fast Forward Merge

```text
A---B main
     \
      C feature
```

After merge:

```text
A---B---C main
```

---

# Merge Commit

```text
A---B main
     \
      C---D feature
```

After merge:

```text
A---B------M
     \    /
      C--D
```

---

## Command

```bash
git merge feature
```

---

# Bad Practice

## ❌ Massive long-lived branches

Causes:

* huge conflicts
* impossible reviews

Prefer:

* smaller PRs
* frequent merges

---

# 12. git rebase

MOST dangerous + powerful.

---

# Mental Model

Merge says:

```text
Combine histories
```

Rebase says:

```text
Rewrite my branch as if it started later
```

---

# Example

Before:

```text
A---B---C main
     \
      D---E feature
```

After:

```text
A---B---C main
         \
          D'---E'
```

---

## Command

```bash
git rebase main
```

---

# GOLDEN RULE

## ❌ NEVER rebase shared/public branches

Why?

Because:

* commit hashes change
* teammates history breaks

Safe:

* your local feature branch

Unsafe:

* shared main/dev branches

---

# 13. git fetch

## Purpose

Download remote changes ONLY.

Safe operation.

---

```bash
git fetch origin
```

---

# Difference

| Command | Does                 |
| ------- | -------------------- |
| fetch   | download only        |
| pull    | download + integrate |

---

# 14. git pull

## Default Behavior

```bash
git fetch
git merge
```

combined.

---

## Rebase pull

```bash
git pull --rebase
```

Cleaner history.

---

# Common Problem

## ❌ Pulling with dirty working tree

Can create:

* merge conflicts
* partial states

Always:

```bash
git status
```

before pull.

---

# 15. git push

## Upload commits

```bash
git push origin main
```

---

# First push tracking

```bash
git push -u origin feature/auth
```

Sets upstream.

Future:

```bash
git push
```

works automatically.

---

# Dangerous Command

## Force push

```bash
git push --force
```

Can erase teammate commits.

---

# Safer Alternative

```bash
git push --force-with-lease
```

Protects against overwriting others unexpectedly.

---

# 16. git reset

VERY misunderstood.

---

# Three Modes

| Mode  | Moves HEAD | Staging | Working Files |
| ----- | ---------- | ------- | ------------- |
| soft  | YES        | NO      | NO            |
| mixed | YES        | YES     | NO            |
| hard  | YES        | YES     | YES           |

---

# Soft Reset

```bash
git reset --soft HEAD~1
```

Undo commit, keep staged.

---

# Mixed Reset (default)

```bash
git reset HEAD~1
```

Undo commit, keep files unstaged.

---

# Hard Reset

```bash
git reset --hard HEAD~1
```

DESTROYS changes.

---

# Dangerous Mistake

## ❌ Using hard reset casually

People permanently lose work.

Before hard reset:

```bash
git stash
```

or:

* backup branch

---

# 17. git revert

SAFE undo for shared branches.

---

## Example

```bash
git revert <commit>
```

Creates NEW commit reversing changes.

---

# Difference from reset

| reset  | rewrite history  |
| ------ | ---------------- |
| revert | preserve history |

---

# Use revert when:

* commit already pushed
* team branch
* production branches

---

# 18. git stash

Temporary storage.

---

## Save changes

```bash
git stash
```

---

## Named stash

```bash
git stash push -m "WIP auth refactor"
```

---

## List stashes

```bash
git stash list
```

---

## Restore

```bash
git stash pop
```

---

# Common Problem

## ❌ Forgetting stash exists

Developers lose track.

Always name stashes.

---

# 19. git reflog

LIFESAVER COMMAND.

---

# Purpose

Shows ALL HEAD movements.

Even deleted commits.

---

## Example

```bash
git reflog
```

---

# Recover deleted commit

```bash
git checkout <hash>
```

or:

```bash
git reset --hard <hash>
```

---

# This saves people after:

* bad reset
* bad rebase
* deleted branch
* accidental force operations

---

# 20. git cherry-pick

Apply specific commit elsewhere.

---

## Example

```bash
git cherry-pick abc123
```

---

# Use Cases

* hotfix transfer
* selective feature migration

---

# Bad Practice

## ❌ Excessive cherry-picking

Can create:

* duplicate commits
* confusing history

---

# PART 4 — GIT CONFLICTS

# Why conflicts happen

Git cannot decide automatically.

Example:

Branch A:

```js
const name = "John"
```

Branch B:

```js
const name = "Jane"
```

Git asks human.

---

# Conflict Markers

```text
<<<<<<< HEAD
John
=======
Jane
>>>>>>> feature
```

---

# Proper Conflict Resolution

1. Understand BOTH sides
2. Decide correct final state
3. Remove markers
4. Test application
5. Commit merge

---

# BAD PRACTICE

## ❌ Blindly accepting incoming/current

Many developers:

* break logic
* remove fixes accidentally

---

# PART 5 — ADVANCED COMMANDS

# git bisect

Binary search for bugs.

---

## Start

```bash
git bisect start
```

---

## Mark bad commit

```bash
git bisect bad
```

---

## Mark known good

```bash
git bisect good <hash>
```

Git automatically searches history.

---

# git blame

Who changed line.

```bash
git blame app.js
```

Useful:

* debugging
* context gathering

NOT for:

* blaming humans socially

---

# git tag

Release markers.

---

## Create tag

```bash
git tag v1.0.0
```

---

## Annotated tag

```bash
git tag -a v1.0.0 -m "Production release"
```

---

# PART 6 — PROFESSIONAL WORKFLOW

# Ideal Team Flow

```text
main
 ├── feature/auth
 ├── feature/payment
 └── hotfix/login
```

---

# Recommended Flow

```bash
git checkout main
git pull

git switch -c feature/auth

# work

git add -p
git commit

git fetch origin
git rebase origin/main

git push
```

---

# PART 7 — WHAT PROFESSIONALS AVOID

# NEVER DO THESE

---

## ❌ Commit secrets

Examples:

* API keys
* passwords
* .env

Use:

```gitignore
.env
```

---

## ❌ Giant commits

Makes:

* review impossible
* debugging harder

---

## ❌ Force push shared branches

Can destroy work.

---

## ❌ Commit generated files unnecessarily

Examples:

* node_modules
* build outputs

---

## ❌ Use vague commit messages

Bad:

```text
fix
changes
update
```

---

# PART 8 — THE MOST IMPORTANT REAL-WORLD COMMANDS

If you master ONLY these deeply, you're already above many developers:

```bash
git status
git add -p
git diff
git log --graph --oneline --all
git rebase
git stash
git reflog
git cherry-pick
git reset
git revert
```

---

# PART 9 — THE TRUE GIT SKILL GAP

Beginner Git:

```text
memorizing commands
```

Advanced Git:

```text
understanding commit graph manipulation
```

Elite Git:

```text
recovering from disasters confidently
```

---

# FINAL MENTAL MODEL

Git is basically:

```text
A graph of immutable snapshots
+
movable pointers
+
history manipulation tools
```

Everything else becomes easier once this clicks.

---

# Suggested Next Deep Topics

You should next study:

1. Interactive Rebase
2. Git Internals (.git folder)
3. Reflog Recovery
4. Merge Strategies
5. Detached HEAD
6. Git Hooks
7. Submodules
8. Worktrees
9. Sparse Checkout
10. Git Object Model
11. Packfiles
12. Advanced Conflict Resolution
13. Gerrit vs GitHub workflows
14. Monorepo Git strategies
15. CI/CD Git patterns
16. Semantic Versioning + Tags
17. Conventional Commits
18. Git Flow vs Trunk-Based Development
19. Rebase vs Merge philosophy
20. Signed commits and GPG
