# Chapter 4 — Your First Repository

## Learning Objectives

By the end of this chapter you will be able to:

- Create a new Git repository from scratch
- Understand the three zones of Git in practice
- Stage and commit files with confidence
- Read and interpret git status output
- Read and interpret git diff output
- View your commit history with git log
- Undo changes safely before committing
- Create and use a .gitignore file

---

## 4.1 The Three Zones — A Quick Reminder

Before typing a single command, fix this picture in your mind.
Every Git operation moves changes between these three zones:

Zone 1 Zone 2 Zone 3
Working Tree --> Staging Area --> Repository
(your files) (the basket) (permanent history)

git add git commit


**Working tree** — files on your disk, actively being edited  
**Staging area** — changes queued up for the next commit  
**Repository** — permanent history, safe forever  

Everything in this chapter is about moving changes between
these three zones.

---

## 4.2 Creating Your First Repository

Open your terminal (Command Prompt on Windows, Terminal on Mac/Linux).

### Step 1 — Navigate to your Desktop

**Windows:**

cd C:\Users\YourName\Desktop


**Mac/Linux:**

cd ~/Desktop


### Step 2 — Create a project folder

mkdir my-first-repo
cd my-first-repo


### Step 3 — Initialise Git

git init


You will see:

Initialized empty Git repository in /Desktop/my-first-repo/.git/


**What just happened:**
Git created a hidden `.git` folder inside `my-first-repo`.
That folder IS Git's entire database for this project.
It stores every commit, every branch, every piece of history.

> Warning: Never delete the `.git` folder. Deleting it permanently
> destroys your entire project history. The files remain but Git
> forgets everything.

### Step 4 — Verify

git status


You will see:

On branch main
No commits yet
nothing to commit (create/copy files and use "git add" to track)


Read this output carefully:
- `On branch main` — you are on the main branch
- `No commits yet` — this repo has zero history
- `nothing to commit` — no files exist yet

Git is even helpful enough to tell you what to do next.

---

## 4.3 Your First File and Commit

### Create a file

**Windows:**

echo Hello from my first Git repo > readme.txt


**Mac/Linux:**

echo "Hello from my first Git repo" > readme.txt


### Check the status

git status


Output:

On branch main

No commits yet

Untracked files:
(use "git add <file>..." to include in what will be committed)
readme.txt

nothing added to commit but untracked files present


**Reading this output:**

| Line | Meaning |
|------|---------|
| `Untracked files` | Git sees the file but is not watching it |
| `readme.txt` | The specific file that is untracked |
| `use "git add"` | Git tells you exactly what to do next |

The file is in Zone 1 (working tree) but Git is not tracking it.

### Stage the file

git add readme.txt


### Check status again

git status


Output:

On branch main

No commits yet

Changes to be committed:
(use "git restore --staged <file>..." to unstage)
new file: readme.txt


**What changed:**
- `Changes to be committed` — the file moved to Zone 2 (staging area)
- `new file: readme.txt` — Git will record this as a brand new file

### Make your first commit

git commit -m "Add readme file"


Output:

[main (root-commit) a3f9c2b] Add readme file
1 file changed, 1 insertion(+)
create mode 100644 readme.txt


**Reading this output:**

| Part | Meaning |
|------|---------|
| `main` | The branch this commit is on |
| `root-commit` | This is the very first commit — no parent |
| `a3f9c2b` | The short SHA hash — unique ID |
| `Add readme file` | Your commit message |
| `1 file changed` | One file was added |
| `1 insertion(+)` | One line was added |

**Congratulations — you have made your first Git commit.**

---

## 4.4 Viewing History

git log


This shows the full commit history. Press `q` to exit.

For a cleaner view:

git log --oneline


Output:

a3f9c2b (HEAD -> main) Add readme file


Read this:
- `a3f9c2b` — short SHA hash
- `HEAD -> main` — you are here, on main branch
- `Add readme file` — your commit message

As you make more commits the log grows upward — newest at the top.

---

## 4.5 Making More Commits

Let us practice the full workflow again.

### Add a second line to the file

**Windows:**

echo This is my Git practice project >> readme.txt


**Mac/Linux:**

echo "This is my Git practice project" >> readme.txt


Note: `>>` appends to the file. `>` would overwrite it.

### See what changed

git diff


Output:

diff --git a/readme.txt b/readme.txt
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1,2 @@
Hello from my first Git repo
+This is my Git practice project


**Reading git diff:**

| Symbol | Meaning |
|--------|---------|
| `---` | The old version of the file |
| `+++` | The new version of the file |
| ` ` (space) | This line is unchanged |
| `+` | This line was added (shown in green) |
| `-` | This line was removed (shown in red) |

### Stage and commit

git add readme.txt
git commit -m "Add project description to readme"


### View the updated history

git log --oneline


Output:

b8d2f1a (HEAD -> main) Add project description to readme
a3f9c2b Add readme file


Two commits. Your history is growing.

---

## 4.6 Staging Selected Changes

One of Git's most powerful features is that you do not have to
commit everything at once. You can choose exactly which changes
go into each commit.

### Stage a specific file

git add readme.txt


### Stage all changed files at once

git add .


The dot means "everything in the current folder and subfolders."

### Stage parts of a file (patch mode)

git add -p readme.txt


This enters interactive mode where Git shows you each change
individually and asks if you want to stage it. Type:
- `y` to stage this change
- `n` to skip this change
- `q` to quit

**Why this matters:**
Imagine you made two unrelated changes in one file — fixed a typo
AND added a new feature. Good commits are focused on one thing.
Patch mode lets you put the typo fix in one commit and the feature
in another, even though they are in the same file.

---

## 4.7 Reading git status Like a Pro

`git status` is the command you will run more than any other.
Here is how to read every possible output:

### Clean state

On branch main
nothing to commit, working tree clean

Meaning: Everything is committed. No changes anywhere.

### Untracked files

Untracked files:
newfile.txt

Meaning: Git sees this file but has never tracked it.
Action: `git add newfile.txt`

### Modified file (not staged)

Changes not staged for commit:
modified: readme.txt

Meaning: A tracked file has been changed but not staged.
Action: `git add readme.txt`

### Staged file

Changes to be committed:
modified: readme.txt

Meaning: Change is in the staging area, ready to commit.
Action: `git commit -m "your message"`

### Mixed state (common in real work)

Changes to be committed:
modified: readme.txt

Changes not staged for commit:
modified: style.css

Untracked files:
newpage.html

Meaning: readme.txt is staged, style.css is modified but not staged,
newpage.html has never been tracked.

---

## 4.8 Undoing Changes

Git provides several ways to undo changes depending on where
in the three zones you are.

### Undo changes in the working tree (before staging)

Restore a file to its last committed state:

git restore readme.txt


Restore ALL files to their last committed state:

git restore .


> Warning: This permanently discards uncommitted changes.
> There is no undo for this command. Use with care.

### Unstage a file (move back from staging to working tree)

git restore --staged readme.txt


This removes the file from the staging area but keeps your changes.
The file goes back to Zone 1 (working tree).

### Fix the last commit message (before pushing)

git commit --amend -m "Corrected commit message"


> Warning: Only use this on commits you have NOT yet pushed.
> Amending a pushed commit causes problems for teammates.

---

## 4.9 The .gitignore File

Some files should never be tracked by Git:
- Password and API key files (`.env`)
- Dependency folders (`node_modules/`)
- Build output (`dist/`, `build/`)
- Editor config files (`.vscode/`, `.idea/`)
- Operating system files (`.DS_Store`, `Thumbs.db`)

### Creating a .gitignore file

Create a file called `.gitignore` in the root of your repo:

**Windows:**

echo .env > .gitignore


Or open it in your editor and add entries:
Dependencies

node_modules/

Environment secrets

.env
.env.local

Build output

dist/
build/

Editor files

.vscode/
.idea/

OS files

.DS_Store
Thumbs.db


Lines starting with `#` are comments — they are ignored by Git.

### Testing it works

Create a secret file:

echo my_password > secret.txt


Add `secret.txt` to `.gitignore`:

echo secret.txt >> .gitignore


Now run:

git status


`secret.txt` will not appear — Git is completely ignoring it.

### Committing .gitignore

Always commit your `.gitignore` file:

git add .gitignore
git commit -m "Add gitignore to exclude sensitive files"


This ensures everyone working on the project shares the same
ignore rules.

### What if I already committed a file I want to ignore?

Adding a file to `.gitignore` after it has been committed does
not un-track it. You must explicitly tell Git to stop tracking it:

git rm --cached secret.txt
git commit -m "Stop tracking secret.txt"


The file stays on your disk but Git no longer watches it.

---

## 4.10 The Complete Everyday Workflow

This is the rhythm you will repeat hundreds of times.
Memorise it. It becomes muscle memory very quickly.

Step 1: Check where you are and what has changed
git status

Step 2: Review the exact changes
git diff

Step 3: Stage the changes you want in this commit
git add filename.txt
(or git add . for everything)

Step 4: Verify what is staged
git diff --staged

Step 5: Commit with a clear message
git commit -m "Describe what changed and why"

Step 6: Confirm it is in history
git log --oneline


### Writing good commit messages

A commit message should complete this sentence:
**"If applied, this commit will..."**

| Good message | Bad message |
|-------------|-------------|
| Add user login page | stuff |
| Fix navigation bar alignment bug | fixed it |
| Update readme with installation steps | update |
| Remove deprecated payment function | changes |

**Format:**
- Start with a verb in present tense
- Keep it under 72 characters
- Be specific about what changed

---

## 4.11 Practical Exercise — Build a Mini Project

Follow these steps exactly. Do not skip ahead.

**1.** Create a new folder called `practice-repo` on your Desktop

**2.** Initialise Git inside it

**3.** Create a file called `index.html` with this content:

My first webpage


**4.** Check status — what do you see?

**5.** Stage `index.html`

**6.** Check status again — what changed?

**7.** Commit with the message `"Add homepage"`

**8.** Add a second line to `index.html`:

Built with Git


**9.** Run `git diff` — read the output carefully

**10.** Stage and commit with message `"Add subtitle to homepage"`

**11.** Create a file called `passwords.txt` with content `secret123`

**12.** Create a `.gitignore` file that ignores `passwords.txt`

**13.** Run `git status` — confirm `passwords.txt` does not appear

**14.** Commit the `.gitignore`

**15.** Run `git log --oneline` — you should see 3 commits

---

## Chapter Summary

| Command | What it does |
|---------|-------------|
| `git init` | Initialise a new repository |
| `git status` | Show current state of all three zones |
| `git add filename` | Stage a specific file |
| `git add .` | Stage all changed files |
| `git add -p` | Stage changes interactively |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |
| `git commit -m "msg"` | Save staged changes to history |
| `git log --oneline` | Show compact commit history |
| `git restore filename` | Discard working tree changes |
| `git restore --staged` | Unstage a file |
| `git commit --amend` | Fix last commit message |
| `git rm --cached` | Stop tracking a file |

---

## Assessment — Test Yourself

**Question 1**
You have modified three files — `index.html`, `style.css` and
`script.js`. You only want to commit the changes to `index.html`
right now. What commands do you run?

**Question 2**
You run `git status` and see:

Changes not staged for commit:
modified: readme.txt

What does this mean exactly and what are your options?

**Question 3**
What is the difference between `git diff` and `git diff --staged`?

**Question 4**
You accidentally typed the wrong commit message.
The commit has not been pushed yet. How do you fix it?

**Question 5**
You created a file called `api-keys.txt` and accidentally committed
it. You then added it to `.gitignore`. Is it now safe?
Explain your answer and describe what you should do.

**Question 6**
What does the `>>` operator do that `>` does not?
Why does this matter when adding content to a file?

---

## Answers

**Answer 1**

git add index.html
git commit -m "Your message about index.html changes"

Only `index.html` is staged and committed. The changes to
`style.css` and `script.js` remain in the working tree, unaffected.

**Answer 2**
It means `readme.txt` is a file Git is already tracking (it has
been committed before) and changes have been made to it since the
last commit. Those changes are in Zone 1 (working tree) but have
not been moved to Zone 2 (staging area) yet.
Options:
- Stage it: `git add readme.txt` then commit
- Discard the changes: `git restore readme.txt`
- Leave it as is and work on something else first

**Answer 3**
`git diff` shows changes between the working tree and the staging
area — changes you have made but not yet staged.
`git diff --staged` shows changes between the staging area and
the last commit — changes you have staged but not yet committed.
Use `git diff --staged` as a final check before committing to
confirm exactly what will be saved.

**Answer 4**

git commit --amend -m "Corrected commit message"

This replaces the last commit message. Only use this before pushing.
Once a commit is pushed to a shared branch, amending it rewrites
history and causes problems for anyone who has already pulled that commit.

**Answer 5**
No — it is NOT safe. Adding a file to `.gitignore` after it has
already been committed does not remove it from Git history.
The file and its contents are permanently recorded in past commits.
Anyone who clones the repo can see the historical commits and
read the file contents.
What you should do:
1. `git rm --cached api-keys.txt` — stop tracking the file
2. `git commit -m "Remove api-keys.txt from tracking"`
3. Consider rotating (changing) the exposed API keys immediately
4. If the repo is public, assume the keys are compromised

**Answer 6**
`>` creates a new file or completely overwrites an existing file.
`>>` appends to the end of a file without destroying existing content.
This matters because using `>` on an existing file destroys all
previous content — a dangerous mistake if you intended to add a line.
Always use `>>` when adding to existing files from the terminal.

---

## What is Next

In Chapter 5 we explore one of Git's most powerful features —
branching. You will create parallel timelines, work independently
on features, and merge your work back together.

*Proceed to Chapter 5 — Branching*




