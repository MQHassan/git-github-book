# Annex B — Git Cheat Sheet

## Your One-Page Git Reference

Print this page and keep it at your desk.
Every command you will ever need — organised by what you are trying to do.

---

## SETUP — Do this once per machine

git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --list


---

## START — Beginning a project

git init Start tracking a folder with Git
git clone URL Download a repo from GitHub
git remote add origin URL Connect local repo to GitHub
git remote -v Verify remote connection


---

## DAILY WORKFLOW — What you do every day

git status What has changed?
git diff Show exact line changes (unstaged)
git diff --staged Show staged changes
git add filename Stage one file
git add . Stage everything
git commit -m "message" Save a snapshot
git log --oneline View history (compact)
git log --oneline --graph --all Visual branch history
git push Upload commits to GitHub
git pull Download and apply changes


---

## BRANCHES — Parallel lines of work

git branch List all branches
git branch name Create a branch
git switch name Move to a branch
git switch -c name Create AND move in one step
git merge name Merge branch into current
git branch -d name Delete a merged branch
git branch -D name Force delete any branch
git push origin name Push a branch to GitHub
git merge --abort Cancel a merge in progress


---

## UNDOING — Fixing mistakes

git restore filename Discard working tree changes
git restore --staged filename Unstage a file
git commit --amend -m "msg" Fix last commit message
git revert SHA Safe undo — adds new commit
git reset --soft SHA Erase commits, keep staged
git reset --mixed SHA Erase commits, keep unstaged
git reset --hard SHA Erase commits AND changes


---

## RESCUE — When things go wrong

git reflog Full history of HEAD movements
git stash Shelve uncommitted work
git stash pop Restore shelved work
git stash list Show all stashes
git cherry-pick SHA Copy one commit to current branch
git bisect start Start binary search for a bug
git bisect good SHA Mark a known good commit
git bisect bad Mark current commit as bad
git bisect reset End bisect session
git blame filename Who changed each line


---

## REMOTE — Working with GitHub

git remote add origin URL Connect to GitHub
git remote -v Show remotes
git remote remove name Remove a remote
git push -u origin main First push (sets tracking)
git push Push to tracked remote
git pull Fetch and merge
git fetch Fetch without merging
git clone URL Download a complete repo
git branch -vv Show tracking relationships


---

## HISTORY — Reading the past

git log Full commit history
git log --oneline Compact history
git log --oneline --graph --all Visual branch graph
git log --author="Name" Commits by one author
git log --since="2 weeks ago" Recent commits
git log -- filename History of one file
git diff SHA1 SHA2 Compare two commits
git show SHA Show one commit in full


---

## ADVANCED — Power user commands

git rebase -i HEAD~N Interactive rebase — clean history
git rebase branch Rebase onto another branch
git tag v1.0.0 Create a tag
git tag -a v1.0.0 -m "Release" Annotated tag
git push origin --tags Push all tags
git rm --cached filename Stop tracking a file
git clean -fd Remove untracked files
git submodule add URL Add a submodule


---

## SHORTCUTS — Save time every day

Set these aliases once and use them forever:

git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.last "log -1 HEAD"
git config --global alias.unstage "restore --staged"
git config --global alias.undo "reset HEAD~1 --mixed"


After setting:

git st instead of git status
git lg instead of git log --oneline --graph --all
git last instead of git log -1 HEAD
git unstage instead of git restore --staged
git undo instead of git reset HEAD~1 --mixed


---

## THE THREE ZONES — Never forget this

Zone 1 Zone 2 Zone 3
Working Tree --> Staging Area --> Repository
(your files) (the basket) (permanent history)

  git add                 git commit

  git restore             git restore --staged
  (discard)               (unstage)

---

## COMMIT MESSAGE FORMAT

Good: Add user login page
Good: Fix null pointer in payment processing
Good: Update README with installation steps
Good: Remove deprecated getUserData function

Bad: fix
Bad: update
Bad: wip
Bad: changes
Bad: asdfgh


**The rule:** Complete this sentence — "If applied, this commit will..."

---

## WHEN THINGS GO WRONG — Quick fixes

| Problem | Fix |
|---------|-----|
| Wrong commit message | `git commit --amend -m "correct message"` |
| Committed to wrong branch | `git reset HEAD~1` then switch branch |
| Need to undo last commit | `git reset --soft HEAD~1` |
| Accidentally deleted work | `git reflog` then `git reset --hard SHA` |
| Push rejected | `git pull` then `git push` |
| Merge conflict | Edit files, remove markers, `git add`, `git commit` |
| Forgot to stage a file | `git add file` then `git commit --amend --no-edit` |
| Stashed work disappeared | `git stash list` then `git stash apply stash@{N}` |

---

## THE EVERYDAY WORKFLOW IN 6 STEPS
git status Check where you are
git diff See what changed
git add . Stage your work
git diff --staged Verify what will be committed
git commit -m "msg" Save the snapshot
git push Send to GitHub

Repeat. Every day. It becomes muscle memory.

---

## GITHUB FLOW IN 7 STEPS
git switch -c feature/name Create a branch
[make your changes] Do the work
git add . && git commit Commit regularly
git push origin feature/name Push the branch
[open pull request on GitHub] Request review
[review and approve] Team checks the work
[merge and delete branch] Ship it

---

*Keep committing. Keep pushing. Keep learning.*

*— Git and GitHub: From Zero to Professional*
*— Dr M Quamrul Hassan*

 


