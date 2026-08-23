# Chapter 8 — Advanced Git — Rescue Commands

## Learning Objectives

By the end of this chapter you will be able to:

- Use git stash to shelve work in progress
- Understand the difference between git revert and git reset
- Use git reset with soft, mixed and hard options
- Recover lost commits using git reflog
- Apply specific commits with git cherry-pick
- Find which commit introduced a bug with git bisect
- Identify who changed a specific line with git blame
- Use interactive rebase to clean up commit history

---

## 8.1 Why These Commands Matter

The commands in this chapter are what separate confident
Git users from beginners who panic when something goes wrong.

Every developer makes mistakes:
- Commits to the wrong branch
- Writes a bad commit message
- Accidentally deletes work
- Introduces a bug somewhere in 200 commits

These commands are your rescue toolkit. Learn them and
nothing in Git will ever truly frighten you.

---

## 8.2 git stash — The Emergency Drawer

### The problem it solves

You are mid-way through editing three files when your colleague
says: "There is an urgent bug on main — can you fix it now?"

You cannot switch branches with uncommitted changes that would
conflict. You are not ready to commit your half-finished work.
What do you do?

You stash it.

### How stash works

git stash


This takes all your uncommitted changes and saves them in a
temporary storage area — completely separate from your commits.
Your working tree becomes clean instantly. You can switch
branches freely.

Before stash: After stash:
readme.txt (modified) readme.txt (clean)
style.css (modified) style.css (clean)
new-page.html (untracked) new-page.html (still untracked)


Note: By default, stash saves tracked modified files.
Untracked files are not stashed unless you use `-u`:

git stash -u


### Retrieving your stash

After fixing the bug, get your work back:

git stash pop


Your changes reappear exactly as you left them.

### Stash commands

| Command | What it does |
|---------|-------------|
| `git stash` | Save current changes to stash |
| `git stash -u` | Stash including untracked files |
| `git stash pop` | Restore latest stash and remove it |
| `git stash apply` | Restore latest stash but keep it in stash |
| `git stash list` | Show all stashes |
| `git stash drop` | Delete the latest stash |
| `git stash clear` | Delete all stashes |

### Multiple stashes

You can have several stashes at once:

git stash list


Output:

stash@{0}: WIP on main: a3f9c2b Add readme
stash@{1}: WIP on feature: b8d2f1a Add homepage


Apply a specific stash:

git stash apply stash@{1}


### Stash with a message

Give your stash a descriptive name:

git stash push -m "Half-finished login form"


---

## 8.3 git revert — Safe Undo

### The problem it solves

You pushed a bad commit to a shared branch. Your teammates
have already pulled it. How do you undo it without
breaking their history?

### How revert works

`git revert` creates a NEW commit that undoes the changes
of a specific commit. It does NOT rewrite history —
it adds to it.

Before revert: After revert:

C1 -> C2 -> C3 (bad) C1 -> C2 -> C3 (bad) -> C4 (reverts C3)


C3 is still in history. C4 cancels out its effects.
Anyone who already has C3 can safely pull C4.

### How to use it

Find the commit you want to undo:

git log --oneline

a3f9c2b (HEAD -> main) Bad commit - broke the login
b8d2f1a Add user dashboard
c4d5e6f Add homepage


Revert the bad commit:

git revert a3f9c2b


Git opens your editor for a revert commit message.
Accept the default and save.

Output:

[main f7g8h9i] Revert "Bad commit - broke the login"
1 file changed, 1 deletion(-)


### Reverting without opening the editor

git revert a3f9c2b --no-edit


### Reverting multiple commits

git revert a3f9c2b b8d2f1a


This creates two revert commits — one for each.

### When to use revert

Use `git revert` when:
- The commit has already been pushed to a shared branch
- You want to preserve the full history
- Teammates have already pulled the commit

---

## 8.4 git reset — Rewrite Local History

### The problem it solves

You made several bad commits locally that you have NOT yet
pushed. You want to erase them as if they never happened.

### The golden rule of reset

NEVER use git reset on commits that have already been pushed
to a shared branch. It rewrites history and causes serious
problems for anyone who has pulled those commits.


### The three modes of reset

`git reset` moves HEAD (and your branch pointer) to a
previous commit. What happens to your changes depends
on which mode you use.

git reset --soft COMMIT Commits erased, changes stay STAGED
git reset --mixed COMMIT Commits erased, changes go to WORKING TREE
git reset --hard COMMIT Commits erased, changes DELETED forever


Visualised:

Zone 1 Zone 2 Zone 3
Working tree Staging area Repository (commits)

--hard --mixed HEAD moves back
changes changes to target commit
deleted unstaged

            --soft
            changes
            stay staged

### Practical example

You have these commits:

git log --oneline

d4e5f6a (HEAD -> main) Bad commit 3
c3b2a1f Bad commit 2
b8d2f1a Bad commit 1
a3f9c2b Good commit - last known good state


You want to go back to `a3f9c2b` and erase the three bad commits.

**Using --soft (keep changes staged):**

git reset --soft a3f9c2b

Result: Three commits erased. All changes are in the staging area.
You can review them and re-commit differently.

**Using --mixed (keep changes unstaged):**

git reset --mixed a3f9c2b

Result: Three commits erased. All changes are in the working tree.
You need to re-stage before committing.

**Using --hard (delete everything):**

git reset --hard a3f9c2b

Result: Three commits erased. ALL changes are permanently deleted.
Your working tree matches exactly what `a3f9c2b` looked like.

### Fixing just the last commit

If you only need to fix the most recent commit:

**Fix the commit message:**

git commit --amend -m "Corrected commit message"


**Add a forgotten file to the last commit:**

git add forgotten-file.txt
git commit --amend --no-edit


---

## 8.5 git reflog — The Black Box Recorder

### The problem it solves

You ran `git reset --hard` and deleted important work.
Or you accidentally deleted a branch. The commits seem gone.

`git reflog` is your last safety net.

### What reflog is

Reflog records every single movement of HEAD — every commit,
every reset, every branch switch, every merge. It keeps this
log for 90 days.

Even commits that appear deleted from `git log` are still
visible in reflog.

### How to use it

git reflog


Output:

a3f9c2b HEAD@{0}: reset: moving to a3f9c2b
d4e5f6a HEAD@{1}: commit: Bad commit 3
c3b2a1f HEAD@{2}: commit: Bad commit 2
b8d2f1a HEAD@{3}: commit: Bad commit 1
a3f9c2b HEAD@{4}: commit: Good commit


### Recovering lost commits

Find the SHA of the commit you want to recover — for example
`d4e5f6a` — then:

git reset --hard d4e5f6a


Your lost commits are back.

Or create a new branch pointing to the lost commit:

git switch -c recovered-work d4e5f6a


### Recovering a deleted branch

If you accidentally deleted a branch:

git reflog


Find the last commit that was on that branch, then:

git switch -c recovered-branch a3f9c2b


The branch is restored with all its commits.

---

## 8.6 git cherry-pick — Take One Commit

### The problem it solves

A specific commit on one branch contains a fix you need
on another branch — but you do not want to merge the
entire branch.

### How cherry-pick works

git cherry-pick COMMIT-SHA


This copies the changes from one specific commit and applies
them as a new commit on your current branch.

Before cherry-pick: After cherry-pick:

main: C1 -> C2 main: C1 -> C2 -> C5' (copy of C5)
feature: C1 -> C3 -> C4 -> C5


C5 is copied to main as C5' — same changes, new SHA hash.

### Practical example

You are on main and need a bug fix from the feature branch:

git log feature --oneline

f9e8d7c Fix critical authentication bug
a3f9c2b Add new dashboard feature
b8d2f1a Start feature branch


Cherry-pick just the bug fix:

git cherry-pick f9e8d7c


Output:

[main g1h2i3j] Fix critical authentication bug
1 file changed, 3 insertions(+), 1 deletion(-)


### Cherry-picking multiple commits

git cherry-pick f9e8d7c a3f9c2b


### Cherry-picking a range of commits

git cherry-pick f9e8d7c..a3f9c2b


### When cherry-pick conflicts

If the cherry-picked commit conflicts with your current branch:

1. Resolve the conflict as normal
2. `git add` the resolved files
3. `git cherry-pick --continue`

Or abort:

git cherry-pick --abort


---

## 8.7 git bisect — Find the Bug

### The problem it solves

Your application worked 3 months ago but has a bug now.
You have made 200 commits since then. Which commit
introduced the bug?

`git bisect` uses binary search to find it in as few
steps as possible.

### How binary search works

Instead of checking all 200 commits one by one (worst case:
200 checks), bisect halves the search space each step.
200 commits = maximum 8 steps to find the bad commit.

200 commits -> 100 -> 50 -> 25 -> 13 -> 7 -> 4 -> 2 -> 1


### How to use bisect

**Step 1 — Start bisect:**

git bisect start


**Step 2 — Mark the current commit as bad:**

git bisect bad


**Step 3 — Mark a known good commit:**

git bisect good a3f9c2b


Git checks out the commit halfway between good and bad.

**Step 4 — Test and mark:**

Test the application. Is the bug present?

If yes (bug present):

git bisect bad


If no (working correctly):

git bisect good


Git moves to the next midpoint. Repeat until Git says:

a3f9c2b is the first bad commit
commit a3f9c2b
Author: ...
Date: ...
Add payment processing feature


**Step 5 — End bisect:**

git bisect reset


This returns you to your original branch and commit.

---

## 8.8 git blame — Who Changed This Line?

### The problem it solves

You found a bug in a specific line of code. You need to know
who wrote it, when, and in which commit — so you can understand
why it was written and how to fix it properly.

### How to use blame

git blame filename.txt


Output:

a3f9c2b (Dr Hassan 2026-03-27 10:15:22 +0000 1) Git is a version control system
b8d2f1a (John Smith 2026-03-28 14:30:45 +0000 2) Created by Linus Torvalds in 2005
c4d5e6f (Dr Hassan 2026-03-29 09:00:11 +0000 3) Used by over 100 million developers


**Reading each column:**

| Column | Meaning |
|--------|---------|
| `a3f9c2b` | SHA of the commit that last changed this line |
| `Dr Hassan` | Author of that commit |
| `2026-03-27` | Date of that commit |
| `10:15:22` | Time of that commit |
| `1` | Line number |
| `Git is a...` | The actual line content |

### Blame for a specific section

git blame -L 10,25 filename.txt


Shows only lines 10 to 25.

### Opening the commit from blame

Once you find the SHA, see the full commit:

git show a3f9c2b


This shows everything about that commit — the message,
the author, the date, and every line that changed.

---

## 8.9 Interactive Rebase — Clean Up History

### The problem it solves

You made 8 commits while working on a feature. Looking back,
some have terrible messages, some are tiny fixups that
should be combined with earlier commits, and one was
committed in the wrong order.

Before opening a pull request you want the history to look
clean and professional.

### How interactive rebase works

git rebase -i HEAD~8


`HEAD~8` means "go back 8 commits from HEAD". This opens
your editor showing the last 8 commits:

pick a3f9c2b Add login page
pick b8d2f1a wip
pick c4d5e6f fix typo
pick d5e6f7a Add user profile
pick e6f7g8h more changes
pick f7g8h9i Actually fix the bug
pick g8h9i0j Add tests
pick h9i0j1k Final tweaks


### Rebase commands

Change `pick` to one of these:

| Command | Shortcut | What it does |
|---------|----------|-------------|
| pick | p | Keep this commit as is |
| reword | r | Keep commit but edit the message |
| edit | e | Stop here to amend the commit |
| squash | s | Combine with previous commit |
| fixup | f | Combine with previous, discard this message |
| drop | d | Delete this commit entirely |

### Example — cleaning up history

Before:

pick a3f9c2b Add login page
pick b8d2f1a wip
pick c4d5e6f fix typo
pick d5e6f7a Add user profile


After editing in the rebase editor:

pick a3f9c2b Add login page
fixup b8d2f1a wip
fixup c4d5e6f fix typo
reword d5e6f7a Add user profile


Result:
- `wip` and `fix typo` are absorbed into `Add login page`
- You get to rewrite the message for `Add user profile`
- History goes from 4 commits to 2 clean commits

### The golden rule of rebase

Never rebase commits that have already been pushed
to a shared branch.


Rebase rewrites history — it creates new commits with new
SHA hashes. If teammates have already pulled the original
commits, their history will diverge from yours and
cause serious conflicts.

Use interactive rebase only on LOCAL commits that have
not been pushed yet.

---

## 8.10 Choosing the Right Command

| Situation | Command to use |
|-----------|---------------|
| Need to switch branches but have unfinished work | `git stash` |
| Bad commit already pushed to shared branch | `git revert` |
| Bad commits NOT yet pushed, keep changes | `git reset --soft` |
| Bad commits NOT yet pushed, discard changes | `git reset --hard` |
| Lost commits after reset | `git reflog` |
| Need one commit from another branch | `git cherry-pick` |
| Need to find which commit broke something | `git bisect` |
| Need to know who wrote a specific line | `git blame` |
| Want to clean up commits before a PR | `git rebase -i` |

---

## Chapter Summary

| Command | Key point |
|---------|-----------|
| `git stash` | Temporarily shelve uncommitted work |
| `git stash pop` | Restore shelved work |
| `git revert` | Safe undo — adds a new commit, safe for shared branches |
| `git reset --soft` | Erase commits, keep changes staged |
| `git reset --mixed` | Erase commits, keep changes unstaged |
| `git reset --hard` | Erase commits AND changes permanently |
| `git reflog` | View full HEAD movement history — ultimate safety net |
| `git cherry-pick` | Copy a specific commit to current branch |
| `git bisect` | Binary search to find which commit broke something |
| `git blame` | Show who last changed each line of a file |
| `git rebase -i` | Interactively rewrite local commit history |

---

## Assessment — Test Yourself

**Question 1**
You are halfway through writing a new feature when your manager
asks you to fix an urgent bug on main immediately.
What is the exact sequence of commands?

**Question 2**
What is the fundamental difference between `git revert` and
`git reset`? When must you use revert instead of reset?

**Question 3**
You ran `git reset --hard HEAD~3` and lost three commits
of important work. Is this recoverable? If so, how?

**Question 4**
You find a bug on line 47 of `app.py`. You want to know
who introduced it and when. What command do you run and
how do you read the output?

**Question 5**
Your feature branch has these commits before you open a PR:

Add login feature
wip - halfway through
fix spacing
actually fix spacing
test test test
final version hopefully

How do you clean this up to one meaningful commit?

**Question 6**
You need to apply a security fix from your `hotfix` branch
to both `main` and `release-v2` without merging the entire
hotfix branch into either. What command do you use?

---

## Answers

**Answer 1**

git stash ← shelve current work
git switch main ← move to main
git switch -c fix/urgent-bug ← create fix branch
[make the fix]
git add .
git commit -m "Fix urgent bug"
git push origin fix/urgent-bug
[open PR and merge]
git switch feature/my-feature ← return to your feature
git stash pop ← restore your work


**Answer 2**
`git revert` creates a new commit that undoes the changes of
a previous commit. History is preserved — the original commit
stays. Safe for shared branches because it does not rewrite
history.

`git reset` moves HEAD backward to a previous commit, effectively
erasing commits from history. It rewrites history — the erased
commits disappear from `git log`.

You MUST use `git revert` (not reset) when:
- The commit has already been pushed to a shared branch
- Teammates have already pulled the commit
- You need to preserve the complete audit trail

**Answer 3**
Yes — recoverable using `git reflog`.

git reflog


Find the SHA of the commit that was HEAD before the reset —
it will show as `HEAD@{1}` or similar.

git reset --hard THAT-SHA


Your three commits and all their changes are restored.
This works because `git reset --hard` removes commits from
`git log` but reflog retains them for 90 days.

**Answer 4**

git blame app.py


Find line 47 in the output. Read across:
- First column: SHA hash of the commit that last changed this line
- Second column: author name
- Third column: date and time
- Fourth column: line number
- Remaining: the actual code

To see the full context of that commit:

git show SHA-FROM-BLAME


This shows the complete commit — message, author, date,
and every line that changed — giving full context for
why line 47 was written that way.

**Answer 5**
Use interactive rebase:

git rebase -i HEAD~6


In the editor change to:

pick Add login feature
fixup wip - halfway through
fixup fix spacing
fixup actually fix spacing
fixup test test test
fixup final version hopefully


`fixup` absorbs all 5 messy commits into the first one,
discarding their messages. Result: one clean commit
`Add login feature` appears on the PR — professional
and readable.

**Answer 6**
`git cherry-pick`

First apply to main:

git switch main
git cherry-pick HOTFIX-COMMIT-SHA
git push


Then apply to release-v2:

git switch release-v2
git cherry-pick HOTFIX-COMMIT-SHA
git push


The same fix is now applied to both branches without
merging the entire hotfix branch into either.

---

## What is Next

In Chapter 9 we explore GitHub Actions — the automation
engine built into GitHub. You will learn to run tests,
check code quality and deploy applications automatically
every time you push.

*Proceed to Chapter 9 — GitHub Actions and CI/CD*

 


