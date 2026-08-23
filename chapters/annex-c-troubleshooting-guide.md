# Annex C — Troubleshooting Guide

## About This Guide

This guide covers the 25 most common Git problems —
the exact error messages you will see and the exact
steps to fix them.

When something goes wrong, find your error message here
and follow the steps. Every problem has a solution.

---

## How to Use This Guide

1. Find your error message in the section headings
2. Read the cause — understand why it happened
3. Follow the fix — step by step
4. Read the prevention — avoid it next time

---

## Problem 1 — git is not recognised

**Error message:**

'git' is not recognized as an internal or external command,
operable program or batch file.


**Cause:**
Git is not installed, or the terminal window was open
before Git was installed.

**Fix:**

Step 1: Close ALL terminal windows completely
Step 2: Open a brand new terminal
Step 3: Type: git --version


If still not recognised:

Step 4: Restart your computer
Step 5: Type: git --version again


If still failing — Git was not installed correctly.
Reinstall from https://git-scm.com

**Prevention:**
Always open a fresh terminal after installing new software.

---

## Problem 2 — Push rejected (fetch first)

**Error message:**

! [rejected] main -> main (fetch first)
error: failed to push some refs to 'https://github.com/...'
hint: Updates were rejected because the remote contains
hint: work that you do not have locally.


**Cause:**
Someone else (or you on another machine) pushed commits
to GitHub since your last pull. Your local history is
behind the remote.

**Fix:**

git pull
git push


If pull creates a merge conflict — resolve it, then push.

**Prevention:**
Always `git pull` before starting work each session.

---

## Problem 3 — Push rejected (non-fast-forward)

**Error message:**

! [rejected] main -> main (non-fast-forward)
error: failed to push some refs
hint: Updates were rejected because the tip of your
hint: current branch is behind its remote counterpart.


**Cause:**
Same as Problem 2 — remote has commits you do not have.

**Fix:**

git pull --rebase
git push


`--rebase` replays your commits on top of the remote
commits — keeps history cleaner than a merge.

---

## Problem 4 — Remote origin already exists

**Error message:**

error: remote origin already exists.


**Cause:**
You ran `git remote add origin` but a remote called
`origin` already exists for this repo.

**Fix:**

git remote remove origin
git remote add origin YOUR-CORRECT-URL
git remote -v


Verify the new remote is correct before pushing.

---

## Problem 5 — Authentication failed

**Error message:**

remote: Invalid username or password.
fatal: Authentication failed for 'https://github.com/...'


**Cause:**
GitHub no longer accepts your account password for
terminal operations. You need a Personal Access Token.

**Fix:**

Step 1: Go to GitHub → Profile → Settings → Developer settings
Step 2: Personal access tokens → Tokens (classic)
Step 3: Generate new token (classic)
Step 4: Select 'repo' scope → Generate
Step 5: Copy the token (starts with ghp_)
Step 6: When Git asks for password — paste the token


**Prevention:**
Set up Git Credential Manager to store your token:

git config --global credential.helper manager


---

## Problem 6 — Repository not found

**Error message:**

ERROR: Repository not found.
fatal: Could not read from remote repository.


**Cause:**
Wrong URL, the repository is private and you are not
authenticated, or the repository was deleted.

**Fix:**

Step 1: Check the URL is correct
git remote -v
Step 2: If wrong URL:
git remote remove origin
git remote add origin CORRECT-URL
Step 3: If private repo — authenticate properly
(see Problem 5 for token setup)


---

## Problem 7 — Merge conflict

**Error message:**

CONFLICT (content): Merge conflict in filename.txt
Automatic merge failed; fix conflicts and then commit the result.


**Cause:**
Two branches made different changes to the same lines
of the same file.

**Fix:**

Step 1: Open the conflicted file
Step 2: Find the conflict markers:
<<<<<<< HEAD
your version
=======
their version
>>>>>>> branch-name
Step 3: Edit the file — keep what you want, remove markers
Step 4: git add filename.txt
Step 5: git commit


To cancel and go back to before the merge:

git merge --abort


**Prevention:**
Pull frequently. Small focused branches reduce conflicts.

---

## Problem 8 — Detached HEAD

**Error message:**

HEAD detached at a3f9c2b


**Cause:**
You checked out a specific commit SHA instead of a branch.
You are not on any branch — commits made here may be lost.

**Fix:**
If you want to keep work done in detached HEAD state:

git switch -c new-branch-name


If you just want to go back to main:

git switch main


**Prevention:**
Always check out branch names, not commit SHAs, unless
you are specifically browsing history.

---

## Problem 9 — Nothing to commit

**Message:**

nothing to commit, working tree clean


**Cause:**
This is not an error — it means everything is committed.
No uncommitted changes exist.

**If you expected changes:**

Step 1: git status Check what Git sees
Step 2: git diff Check for unstaged changes
Step 3: Check you saved the file in your editor
Step 4: Check you are in the right folder


---

## Problem 10 — Untracked files after adding to .gitignore

**Problem:**
You added a file to `.gitignore` but `git status` still
shows it as modified or tracked.

**Cause:**
`.gitignore` only ignores files that have never been
tracked. Once a file is committed, adding it to
`.gitignore` does not un-track it.

**Fix:**

git rm --cached filename
git commit -m "Stop tracking filename"


The file stays on your disk but Git stops watching it.

**Prevention:**
Add entries to `.gitignore` before creating the files.

---

## Problem 11 — Accidentally committed to main

**Problem:**
You made commits directly to main that should have been
on a feature branch.

**Fix (commits not yet pushed):**

Step 1: Create the correct branch pointing here
git switch -c feature/correct-branch
Step 2: Go back to main
git switch main
Step 3: Remove the commits from main
git reset --hard origin/main
Step 4: Your commits are now safely on feature/correct-branch


**Fix (commits already pushed):**

Step 1: Revert each bad commit
git revert SHA
Step 2: Push the reversions
git push


---

## Problem 12 — Wrong commit message

**Problem:**
You typed the wrong commit message.

**Fix (last commit, not yet pushed):**

git commit --amend -m "Correct message here"


**Fix (last commit, already pushed):**

git revert SHA
git commit -m "Correct action with right message"


Never amend pushed commits — it causes problems for others.

---

## Problem 13 — Accidentally deleted a branch

**Problem:**
You ran `git branch -d` or `git branch -D` and deleted
a branch you still needed.

**Fix:**

Step 1: Find the last commit on the deleted branch
git reflog
Step 2: Look for the commit SHA before the deletion
Step 3: Recreate the branch pointing to that commit
git switch -c recovered-branch SHA


**Prevention:**
Always verify a branch is merged before deleting.
Use `-d` (safe delete) not `-D` (force delete) by default.

---

## Problem 14 — Lost work after git reset --hard

**Problem:**
You ran `git reset --hard` and lost commits you needed.

**Fix:**

Step 1: git reflog
Step 2: Find the SHA of the commit before the reset
(look for HEAD@{1} or similar)
Step 3: git reset --hard THAT-SHA


Your commits are restored.

**Prevention:**
Before any destructive operation, note the current SHA:

git log --oneline -1


---

## Problem 15 — Cannot switch branches (unsaved changes)

**Error message:**

error: Your local changes to the following files would be
overwritten by checkout:
filename.txt
Please commit your changes or stash them before you switch branches.


**Cause:**
You have uncommitted changes that would be overwritten
by switching branches.

**Fix:**
Option 1 — Stash the changes:

git stash
git switch other-branch
[do your work]
git switch back
git stash pop


Option 2 — Commit the changes:

git add .
git commit -m "WIP - switching branches"
git switch other-branch


Option 3 — Discard the changes:

git restore .
git switch other-branch


---

## Problem 16 — Diverged branches

**Error message:**

Your branch and 'origin/main' have diverged,
and have 1 and 1 different commits each, respectively.


**Cause:**
Your local branch and the remote branch both have commits
the other does not have. They have diverged.

**Fix:**

git pull --rebase
git push


Or if you prefer a merge commit:

git pull
git push


---

## Problem 17 — File too large for GitHub

**Error message:**

remote: error: File filename is 150.00 MB;
this exceeds GitHub's file size limit of 100 MB.


**Cause:**
GitHub rejects files larger than 100MB.

**Fix:**

Step 1: Remove the file from the last commit
git rm --cached large-file
Step 2: Add it to .gitignore
echo large-file >> .gitignore
Step 3: Commit the removal
git commit --amend --no-edit
Step 4: Push
git push


For very large files use Git LFS (Large File Storage):

git lfs track "*.psd"
git add .gitattributes


---

## Problem 18 — SSL certificate error

**Error message:**

SSL certificate problem: unable to get local issuer certificate


**Cause:**
Your network or VPN is intercepting HTTPS connections.
Common in corporate environments.

**Fix (temporary):**

git config --global http.sslVerify false


**Better fix:**
Contact your IT department for the correct SSL certificate
to add to Git's trust store.

**Prevention:**
Only disable SSL verification as a last resort and
re-enable it when done.

---

## Problem 19 — Line ending warnings

**Warning message:**

warning: LF will be replaced by CRLF in filename.
The file will have its original line endings in your working directory.


**Cause:**
Windows uses CRLF line endings. Mac/Linux uses LF.
Git is warning you about the conversion.

**Fix (Windows — recommended):**

git config --global core.autocrlf true


**Fix (Mac/Linux — recommended):**

git config --global core.autocrlf input


This is a warning not an error — your files will work correctly.

---

## Problem 20 — Accidentally staged everything

**Problem:**
You ran `git add .` and staged files you did not want
to include in this commit.

**Fix — unstage everything:**

git restore --staged .


**Fix — unstage one file:**

git restore --staged filename


Your changes are preserved in the working tree —
only the staging is undone.

---

## Problem 21 — Cannot push to protected branch

**Error message:**

remote: error: GH006: Protected branch update failed
remote: error: Required status check "ci" is failing.


**Cause:**
The branch has protection rules — direct pushes are
blocked, or required checks have not passed.

**Fix:**

Step 1: Create a feature branch
git switch -c feature/my-change
Step 2: Push the feature branch
git push origin feature/my-change
Step 3: Open a pull request on GitHub
Step 4: Wait for required checks to pass
Step 5: Get required approvals
Step 6: Merge via GitHub


---

## Problem 22 — Submodule errors

**Error message:**

fatal: No url found for submodule path 'subfolder'


**Cause:**
The repository contains submodules that have not been
initialised.

**Fix:**

git submodule update --init --recursive


When cloning a repo with submodules:

git clone --recurse-submodules URL


---

## Problem 23 — git log shows nothing

**Problem:**
`git log` returns immediately with no output.

**Cause:**
No commits have been made yet in this repository.

**Fix:**
Make your first commit:

git add .
git commit -m "Initial commit"


---

## Problem 24 — Wrong email in commits

**Problem:**
Your commits show the wrong email address — they do not
link to your GitHub profile.

**Fix for future commits:**

git config --global user.email "correct@email.com"


**Fix for the last commit:**

git commit --amend --reset-author --no-edit


**Fix for multiple past commits:**
Use `git rebase -i` to rewrite history.
Warning: Only do this for commits not yet pushed.

---

## Problem 25 — Workflow file not running

**Problem:**
You pushed a `.github/workflows/main.yml` file but
the workflow is not appearing in the Actions tab.

**Cause:**
YAML syntax error, wrong file location, or wrong trigger.

**Fix:**

Step 1: Check the file is in exactly:
.github/workflows/main.yml
(not .github/workflow/ or github/workflows/)

Step 2: Validate YAML syntax at:
https://yamlint.com

Step 3: Check the trigger matches your branch name:
on:
push:
branches: [ main ]
(not master if your branch is main)

Step 4: Check the Actions tab for any error messages
from GitHub about the workflow file


---

## Quick Diagnosis Checklist

When anything goes wrong run these in order:

git status What is the current state?
git log --oneline Where is HEAD?
git remote -v Is the remote correct?
git branch Which branch am I on?
git diff What has changed?
git reflog What has HEAD done recently?


These six commands will reveal the cause of almost
every Git problem.

---

## Getting More Help

### GitHub documentation

https://docs.github.com/en/get-started/using-git


### Oh Shit Git — plain English fixes

https://ohshitgit.com


### Stack Overflow
Search for your exact error message — someone has
almost certainly solved it before.

### Git official documentation

https://git-scm.com/doc


---

*End of Annex C*

*Remember: In Git, almost nothing is truly lost.
If you committed it, reflog can find it.
Stay calm, diagnose carefully, fix methodically.*

 