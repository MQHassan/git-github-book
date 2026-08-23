# Chapter 5 — Branching

## Learning Objectives

By the end of this chapter you will be able to:

- Explain what a branch is and how it works internally
- Create and switch between branches
- Understand what happens to your files when you switch branches
- Merge branches using fast-forward and 3-way merge
- Identify, understand and resolve merge conflicts
- Delete branches safely after merging
- Visualise your branch history with git log --graph

---

## 5.1 What is a Branch?

A branch is one of Git's most powerful and most misunderstood features.

### The common misconception

Most beginners imagine a branch as a copy of the project in a
separate folder — like saving a duplicate of your files with a
different name. This is wrong.

### What a branch actually is

A branch is simply a **lightweight pointer to a commit**.

That is all. A label. A sticky note attached to a specific commit.
When you make a new commit on a branch, the pointer moves forward
automatically to point at the new commit.

Before new commit: After new commit:

C1 --> C2 --> C3 C1 --> C2 --> C3 --> C4
^ ^
main main


This is why creating a branch in Git is nearly instant — Git is
not copying any files. It is just creating a new pointer.

### Why branches matter

Branches let you:
- Work on a new feature without touching the main codebase
- Fix a bug while someone else builds a feature simultaneously
- Experiment freely — if it fails, just delete the branch
- Review changes before they go into the main codebase

---

## 5.2 The Default Branch

When you run `git init`, Git creates a default branch.

In modern Git the default branch is called `main`.
In older repos you may see `master` — they work identically,
only the name is different.

### HEAD and branches

Remember HEAD from Chapter 2? It is the pointer that shows
where you are right now.

C1 --> C2 --> C3
^
main <-- HEAD


HEAD points to main. Main points to C3.
You are currently at C3 on the main branch.

When you create a new branch, HEAD can move to point at it:

C1 --> C2 --> C3
^
main
^
feature <-- HEAD


---

## 5.3 Creating and Switching Branches

### See all branches

git branch


The branch with `*` is your current branch:
main

### Create a new branch

git branch feature-login


This creates the branch but does NOT switch to it.
You are still on main.

git branch

feature-login

main

### Switch to the new branch

**Modern way (Git 2.23+):**

git switch feature-login


**Classic way (still works):**

git checkout feature-login


After switching:

git branch

feature-login
main

The `*` has moved to `feature-login`. You are now on the new branch.

### Create and switch in one command

This is what you will use most often:

git switch -c feature-login


The `-c` flag means "create". This creates the branch AND switches
to it in a single command.

---

## 5.4 Working on a Branch

Once you are on a branch, every commit you make belongs to
that branch. The main branch is completely unaffected.

### Practical example

Start on main with 3 commits:

git log --oneline

c3f9a2b (HEAD -> main) Add about page
b8d2f1a Add homepage
a3f9c2b Add readme


Create and switch to a feature branch:

git switch -c feature-contact-page


Create a new file:

echo Contact us at info@example.com > contact.txt
git add contact.txt
git commit -m "Add contact page"


View the log:

git log --oneline

d4e5f6a (HEAD -> feature-contact-page) Add contact page
c3f9a2b (main) Add about page
b8d2f1a Add homepage
a3f9c2b Add readme


Notice:
- `HEAD -> feature-contact-page` — you are on the feature branch
- `main` is still at `c3f9a2b` — completely untouched
- The feature branch has one commit that main does not

### What happens when you switch back to main

git switch main


Now open your file explorer and look at the project folder.
`contact.txt` has disappeared.

This is not a bug. Git has restored your working tree to match
exactly what main looks like. The file exists on the feature branch
— it simply does not belong to main yet.

Switch back to the feature branch:

git switch feature-contact-page


`contact.txt` reappears. This is Git managing your working tree
automatically as you move between branches.

---

## 5.5 Merging Branches

When the work on a branch is complete and tested, you merge it
into main. There are two types of merge.

### Type 1 — Fast-forward merge

A fast-forward merge happens when main has no new commits since
the branch was created. Git simply moves the main pointer forward.

Before merge: After merge:

C1 --> C2 --> C3 --> C4 C1 --> C2 --> C3 --> C4
^ ^ ^
main feature main, feature


No new commit is created. Git just moves the label.

**How to do it:**

First switch to the branch you want to merge INTO:

git switch main


Then merge:

git merge feature-contact-page


Output:

Updating c3f9a2b..d4e5f6a
Fast-forward
contact.txt | 1 +
1 file changed, 1 insertion(+)
create mode 100644 contact.txt


The word `Fast-forward` confirms this type of merge happened.

### Type 2 — 3-way merge

A 3-way merge happens when both main and the feature branch have
new commits since the branch was created. Git cannot simply move
a pointer — it must create a new merge commit that combines both.

Before merge:

C1 --> C2 --> C3 --> C4 (main)

C5 --> C6 (feature)

After merge:

C1 --> C2 --> C3 --> C4 -----> M (merge commit)
\ /
C5 --> C6 ---


Git uses three points to calculate the merge:
1. The common ancestor (C3 — where the branch started)
2. The tip of main (C4)
3. The tip of the feature branch (C6)

**How to do it:**

git switch main
git merge feature-branch


Git opens your editor for a merge commit message.
Accept the default message and save.

---

## 5.6 Merge Conflicts

A merge conflict occurs when two branches have made different
changes to the same lines of the same file. Git cannot decide
which version is correct — it stops and asks you to decide.

### How to create a conflict (for practice)

**Step 1** — Start on main and edit a file:

echo Git is a version control system > readme.txt
git add readme.txt
git commit -m "Update readme on main"


**Step 2** — Create a branch and make a different edit to the same line:

git switch -c fix-readme
echo Git is a powerful version control tool > readme.txt
git add readme.txt
git commit -m "Update readme on fix-readme branch"


**Step 3** — Switch to main and merge:

git switch main
git merge fix-readme


Git stops with:

Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.


### Reading the conflict markers

Open `readme.txt` and you will see:
<<<<<<< HEAD
Git is a version control system

Git is a powerful version control tool

fix-readme


**Understanding each part:**

| Marker | Meaning |
|--------|---------|
| `<<<<<<< HEAD` | Start of YOUR version (current branch) |
| `=======` | Dividing line between the two versions |
| `>>>>>>> fix-readme` | End of THEIR version (branch being merged) |

### Resolving the conflict

You have three choices:
1. Keep your version (delete their version and the markers)
2. Keep their version (delete your version and the markers)
3. Write something new that combines both

For this example, write a combined version:

Edit the file so it contains only this:

Git is a powerful version control system


Remove all three marker lines completely. The file must contain
only the content you want — no `<<<<<<<`, `=======` or `>>>>>>>`.

### Completing the merge

After editing:

git add readme.txt
git commit -m "Resolve merge conflict in readme"


The merge is complete.

### Checking status during a conflict

git status


Output during a conflict:

On branch main
You have unmerged paths.
(fix conflicts and run "git commit")

Unmerged paths:
(use "git add <file>..." to mark resolution)
both modified: readme.txt


`both modified` means both branches changed the same file.

### Aborting a merge

If you want to cancel the merge and go back to where you were:

git merge --abort


This restores your working tree to the state before the merge started.

---

## 5.7 Visualising Branch History

The most useful command for understanding your branch history:

git log --oneline --graph --all


Example output:
1def4a8 (HEAD -> main) Resolve merge conflict in readme
|
| * daf02c2 (fix-readme) Update readme on fix-readme branch
| 53838be Update readme on main
|/
debfd04 Add contact page
c3f9a2b Add about page
a3f9c2b Add readme

**Reading this graph:**

| Symbol | Meaning |
|--------|---------|
| `*` | A commit |
| `\|` | A branch line |
| `\|\ ` | Branch splitting apart |
| `\|/` | Branch merging back together |

Set up a short alias for this command:

git config --global alias.lg "log --oneline --graph --all"


Then just type `git lg` to see the full visual history.

---

## 5.8 Deleting Branches

After a branch has been merged, it has served its purpose.
Delete it to keep your branch list clean.

### Delete a merged branch

git branch -d feature-contact-page


Git checks that the branch has been fully merged before deleting.
If it has not been merged, Git refuses and shows a warning.

### Force delete an unmerged branch

git branch -D feature-contact-page


Capital `-D` skips the merge check and deletes regardless.
Use this when you want to abandon work on a branch entirely.

### View all branches including deleted ones

Deleted branches are gone locally but their commits remain in
`git reflog` for 90 days. If you accidentally delete a branch
you can recover it:

git reflog


Find the commit SHA of the branch tip, then:

git switch -c recovered-branch a3f9c2b


---

## 5.9 Branch Naming Conventions

Good branch names make history readable and teams efficient.

### Common naming patterns

| Pattern | Example | Used for |
|---------|---------|---------|
| `feature/` | `feature/user-login` | New features |
| `fix/` | `fix/navbar-bug` | Bug fixes |
| `hotfix/` | `hotfix/payment-error` | Urgent production fixes |
| `docs/` | `docs/update-readme` | Documentation only |
| `refactor/` | `refactor/clean-database` | Code restructuring |

### Rules for branch names

- Use lowercase letters
- Use hyphens not spaces (`feature-login` not `feature login`)
- Be descriptive but concise
- Avoid special characters except `/` and `-`

---

## 5.10 Common Branching Workflows

### GitHub Flow (recommended for most teams)
Create a branch from main
Make commits on the branch
Open a pull request
Review and discuss
Merge into main
Delete the branch

Simple, clean, works for teams of any size.

### Personal workflow (solo developer)
Create a branch for each feature or fix
Work on the branch
Merge back to main when done
Delete the branch

---

## Chapter Summary

| Command | What it does |
|---------|-------------|
| `git branch` | List all branches |
| `git branch name` | Create a new branch |
| `git switch name` | Switch to a branch |
| `git switch -c name` | Create and switch in one step |
| `git merge name` | Merge a branch into current branch |
| `git branch -d name` | Delete a merged branch |
| `git branch -D name` | Force delete any branch |
| `git merge --abort` | Cancel a merge in progress |
| `git log --oneline --graph --all` | Visual branch history |

---

## Assessment — Test Yourself

**Question 1**
What is a branch in Git? Correct the following statement:
"A branch is a copy of the project files in a separate folder."

**Question 2**
You are on `main` and run `git branch feature-x`.
What is the state of your branches now?
Where is HEAD pointing?

**Question 3**
What is the difference between a fast-forward merge and
a 3-way merge? When does each occur?

**Question 4**
You open a file during a merge conflict and see:
<<<<<<< HEAD
colour = blue

colour = green

design-branch

Explain what each section means and describe three ways
you could resolve this conflict.

**Question 5**
You run `git branch -d old-feature` and Git refuses,
saying the branch is not fully merged. What are your options
and what should you consider before proceeding?

**Question 6**
Why does `contact.txt` disappear from your folder when you
switch from `feature-contact-page` back to `main`?
Is the file lost?

---

## Answers

**Answer 1**
A branch is a lightweight pointer to a commit — it is just a label
that moves forward as new commits are made. It is NOT a copy of files.
Creating a branch takes milliseconds and almost no disk space because
Git only creates a new pointer, not new files. The corrected statement:
"A branch is a lightweight pointer to a specific commit that allows
independent lines of development without copying any files."

**Answer 2**
After running `git branch feature-x`:
- Two branches exist: `main` and `feature-x`
- Both point to the same commit
- HEAD still points to `main` — you have NOT switched branches
- The `*` in `git branch` output is still next to `main`
You must run `git switch feature-x` to move to the new branch.

**Answer 3**
A fast-forward merge occurs when the base branch (main) has no new
commits since the feature branch was created. Git simply moves the
main pointer forward to the tip of the feature branch. No new commit
is created.
A 3-way merge occurs when both branches have new commits since they
diverged. Git cannot simply move a pointer — it must calculate the
differences between both branches and their common ancestor, then
create a new merge commit that combines them.

**Answer 4**
- `<<<<<<< HEAD` to `=======`: the current branch version — `colour = blue`
- `=======` to `>>>>>>>`: the incoming branch version — `colour = green`
- `>>>>>>> design-branch`: end of the conflict block

Three ways to resolve:
1. Keep HEAD version: delete everything except `colour = blue`
2. Keep incoming version: delete everything except `colour = green`
3. Combine: write `colour = blue-green` or any other resolution
In all cases remove the three marker lines completely, then
`git add` the file and `git commit`.

**Answer 5**
Options:
1. Merge the branch first: `git merge old-feature` then delete with `-d`
2. Force delete: `git branch -D old-feature` — permanently loses
   any unmerged commits

Before force deleting, consider:
- Does this branch contain work worth keeping?
- Has anyone else worked on this branch?
- Can the commits be cherry-picked to another branch first?
If the work is truly unwanted, force delete is safe. Remember that
`git reflog` can recover the commits for up to 90 days if you
change your mind.

**Answer 6**
`contact.txt` has not been lost. Git manages your working tree
automatically when you switch branches. When you switch to `main`,
Git restores your folder to exactly match the state of main —
which does not include `contact.txt` because that file was only
committed on `feature-contact-page`.

Switch back to `feature-contact-page` and `contact.txt` reappears
instantly. The file is safe in Git's history. It will appear in
your folder on any branch that contains the commit that added it.

---

## What is Next

In Chapter 6 we connect your local Git repository to the world.
You will create a GitHub account, push your project online,
and learn to collaborate with others through remotes.

*Proceed to Chapter 6 — GitHub and Remotes*

  


