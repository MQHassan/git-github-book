# Annex D — Glossary

## Complete Git and GitHub Terminology

All terms used in this book listed alphabetically.
Use this as a quick reference when you encounter
an unfamiliar term.

---

## A

### Alias
A custom shortcut for a longer Git command. Set with
`git config --global alias.name "command"`. For example
`git config --global alias.st status` lets you type
`git st` instead of `git status`.

### Annotated tag
A tag that stores extra metadata — tagger name, email,
date and a message. Created with `git tag -a v1.0 -m "msg"`.
Recommended for release versions. Compare with lightweight tag.

### Artifact
In GitHub Actions, a file or collection of files produced
during a workflow run and saved for later use. Created with
the `actions/upload-artifact` action.

---

## B

### Base branch
In a pull request, the branch that changes will be merged
INTO. Usually `main`. Compare with compare branch.

### Binary search
The algorithm used by `git bisect` to find a bad commit.
Halves the search space at each step — 200 commits takes
maximum 8 steps to find the culprit.

### Bisect
A Git command (`git bisect`) that uses binary search to
find exactly which commit introduced a bug. You mark
commits as good or bad and Git narrows it down.

### Blame
A Git command (`git blame`) that shows who last changed
each line of a file, when, and in which commit.

### Branch
A lightweight pointer to a specific commit — an independent
line of development. Creating a branch takes milliseconds
because Git only creates a pointer, not a copy of files.

### Branch protection
GitHub rules preventing direct pushes to important branches.
Configured in Repository → Settings → Branches. Can require
pull requests, approvals and passing checks before merging.

---

## C

### Cherry-pick
A Git command (`git cherry-pick SHA`) that copies the
changes from one specific commit and applies them as
a new commit on the current branch.

### CI/CD
Continuous Integration and Continuous Deployment.
Automated processes that run tests on every push (CI)
and deploy code automatically when tests pass (CD).

### Clone
Downloading a complete copy of a remote repository —
including all files, all commits and all branches —
to your local machine. Done with `git clone URL`.

### Commit
A permanent snapshot of your project at a specific moment.
Contains a unique SHA hash, author, timestamp, message
and a record of all changes.

### Commit message
A description of what a commit does and why. Written with
`git commit -m "message"`. Good messages complete the
sentence "If applied, this commit will..."

### Compare branch
In a pull request, the branch containing the new changes
being proposed. Compare with base branch.

### Conflict markers
The symbols Git inserts into a file during a merge conflict:
`<<<<<<<`, `=======` and `>>>>>>>`. You must remove these
markers after resolving the conflict.

### Conventional Commits
A commit message specification using a structured format:
`type(scope): description`. Types include `feat`, `fix`,
`docs`, `refactor`, `test` and `chore`.

---

## D

### Default branch
The primary branch of a repository. Modern standard is `main`.
Older repositories may use `master`. Configured with
`git config --global init.defaultBranch main`.

### Detached HEAD
A state where HEAD points directly to a commit rather than
to a branch. Commits made in detached HEAD state may be lost
unless you create a branch. Fix with `git switch -c name`.

### Diff
The difference between two versions of a file or two commits.
Lines starting with `+` were added (green), lines starting
with `-` were removed (red). Shown with `git diff`.

### Draft pull request
A pull request marked as work in progress — not ready for
review yet. Converted to a regular PR when ready by clicking
"Ready for review".

---

## E

### Environment variable
A configuration value passed to a program. In GitHub Actions,
used to pass configuration to workflow steps. Never hardcode
secrets — use GitHub Secrets instead.

---

## F

### Fast-forward merge
A merge that occurs when the base branch has no new commits
since the feature branch was created. Git simply moves the
branch pointer forward — no new commit is created.

### Fetch
Downloading changes from a remote repository without merging
them. Your working files remain unchanged. Use `git fetch`
to review incoming changes before integrating.

### Fork
A personal copy of someone else's GitHub repository, created
on GitHub's servers. You have full write access to your fork.
Used for contributing to projects you do not own.

### Force push
Pushing to a remote even when the histories have diverged.
Done with `git push --force`. Dangerous on shared branches —
rewrites remote history and causes problems for teammates.

---

## G

### Git
A free, open-source distributed version control system
created by Linus Torvalds in 2005. Runs on your local machine.
Tracks changes to files over time.

### GitHub
A web platform that hosts Git repositories online. Owned
by Microsoft. Adds collaboration features like pull requests,
issues and GitHub Actions on top of Git.

### GitHub Actions
GitHub's built-in CI/CD platform. Runs automated workflows
defined in YAML files when Git events occur — such as pushing
or opening a pull request.

### GitHub Flow
The most widely used team workflow. Branch from main →
commit → push → pull request → review → merge → delete branch.
Main is always deployable.

### GitHub Pages
A free static website hosting service built into GitHub.
Serves HTML, CSS and JavaScript files directly from a
repository at `username.github.io`.

### .gitignore
A file in the root of a repository listing files and
patterns that Git should never track. Commonly used to
exclude secrets, dependencies and build output.

### GitFlow
A branching strategy for versioned software with scheduled
releases. Uses separate `main`, `develop`, `feature`,
`release` and `hotfix` branches.

---

## H

### HEAD
A special pointer showing your current position in the
repository. Usually points to the tip of your current branch.
Moves forward automatically when you commit.

### History
The complete record of all commits in a repository —
who made them, when, and what changed. Viewed with `git log`.

### Hook
A script that runs automatically at specific points in
the Git workflow — before committing, after pushing etc.
Stored in `.git/hooks/`. Used to enforce standards.

### Hotfix
An emergency fix applied directly to a production branch.
In GitFlow, created from `main` and merged back to both
`main` and `develop`. Should be small and targeted.

---

## I

### Index
Another name for the staging area. The intermediate zone
between your working tree and the repository where changes
are prepared before committing.

### Interactive rebase
A rebase mode (`git rebase -i HEAD~N`) that lets you
rewrite, squash, reorder or delete commits before pushing.
Used to clean up messy history before opening a pull request.

---

## J

### Job
In GitHub Actions, a set of steps that run on one virtual
machine. Multiple jobs in a workflow run in parallel by
default. Use `needs` to create dependencies between jobs.

---

## L

### Lightweight tag
A simple named pointer to a commit with no extra metadata.
Created with `git tag v1.0`. Compare with annotated tag.

### Local repository
The copy of a repository on your own machine. Contains
the full history. You commit here before pushing to remote.

---

## M

### main
The default primary branch in modern Git repositories.
Replaced `master` as the standard default. Always kept
in a deployable state in professional workflows.

### Markdown
A lightweight formatting language that converts plain text
to formatted output. GitHub renders `.md` files automatically.
Used for README files and documentation.

### Merge
Combining the changes from one branch into another.
Creates a merge commit in a 3-way merge, or simply moves
a pointer in a fast-forward merge.

### Merge commit
A special commit created during a 3-way merge that has
two parent commits — one from each branch being merged.
Preserves the branching history.

### Merge conflict
When two branches have made different changes to the same
lines of the same file and Git cannot automatically merge
them. Must be resolved manually.

### master
The old default branch name, replaced by `main` as the
modern standard. Still used in older repositories.
Functionally identical to `main`.

---

## O

### Origin
The conventional name for the primary remote — the one
you cloned from or connected to. Just a shortcut name
for the remote URL. Set with `git remote add origin URL`.

---

## P

### Patch mode
An interactive staging mode (`git add -p`) that lets you
stage individual chunks of a file rather than the whole file.
Useful when a file contains multiple unrelated changes.

### Personal Access Token (PAT)
A token used instead of a password for GitHub authentication
in the terminal. Created at GitHub → Settings → Developer
settings → Personal access tokens.

### Pull
Downloading changes from a remote repository AND merging
them into the current branch. Equivalent to `git fetch`
followed by `git merge`. Done with `git pull`.

### Pull request (PR)
A GitHub feature proposing to merge one branch into another.
Creates a space for code review, discussion, automated checks
and approval before any merge happens.

### Push
Uploading local commits to a remote repository. Done with
`git push`. Requires authentication for private repos or
repos you own.

---

## R

### Rebase
Re-applying commits on top of another branch, creating
a cleaner linear history. Rewrites commit history.
Never rebase commits already pushed to shared branches.

### Reflog
A local log of every movement of HEAD — commits, resets,
merges, branch switches. Kept for 90 days. The ultimate
safety net for recovering apparently lost work.

### Remote
A version of a repository hosted on another server —
typically GitHub. You push to and pull from remotes.
The primary remote is conventionally named `origin`.

### Repository (repo)
A folder tracked by Git. Contains all project files plus
a hidden `.git` folder storing the entire history.

### Revert
Creating a new commit that undoes the changes of a specific
previous commit. Safe for shared branches — does not rewrite
history. Done with `git revert SHA`.

### Runner
In GitHub Actions, the virtual machine that executes
workflow jobs. GitHub provides Ubuntu, Windows and macOS
runners. Also supports self-hosted runners.

---

## S

### Secret
An encrypted value stored securely in GitHub settings.
Used in workflow files as `${{ secrets.NAME }}`. Never
visible in logs. Used for passwords, tokens and API keys.

### SHA hash
The unique 40-character identifier of every commit.
Generated by a cryptographic hash of the commit contents.
Usually shown as the first 7 characters, e.g. `a3f9c2b`.

### Squash
Combining multiple commits into one. Used in interactive
rebase to clean up messy history before merging a PR.
Also a merge strategy on GitHub — "Squash and merge".

### SSH key
A cryptographic key pair used for authenticating with
GitHub without typing a password. The private key stays
on your machine. The public key is added to your GitHub account.

### Staging area
The preparation zone between your working tree and the
repository. Changes are moved here with `git add` before
being permanently saved with `git commit`. Also called
the index.

### Stash
Temporarily shelving uncommitted changes to get a clean
working tree. Done with `git stash`. Restored with
`git stash pop`. Useful when switching branches mid-work.

### Step
In GitHub Actions, a single task within a job. Either
runs a shell command (`run`) or a marketplace action (`uses`).
Steps run sequentially within a job.

### Submodule
A Git repository embedded inside another Git repository.
Used to include external dependencies at a specific commit.
Managed with `git submodule` commands.

---

## T

### Tag
A named reference to a specific commit. Unlike branches,
tags do not move when new commits are made. Used to mark
release versions like `v1.0.0`. Created with `git tag`.

### Tracking branch
A local branch set up to follow a remote branch. Git knows
which remote branch to push to and pull from automatically.
Set up with `git push -u origin main`.

### Trigger
In GitHub Actions, the event that starts a workflow —
such as a push, pull request, schedule or manual dispatch.
Defined in the `on:` section of the workflow file.

### Trunk-Based Development
A branching strategy where everyone commits to the main
branch (trunk) directly or through very short-lived branches.
Requires strong automated testing and feature flags.

---

## U

### Untracked file
A file in the working tree that Git has never seen before —
it has not been added or committed. Shown in `git status`
under "Untracked files". Add with `git add filename`.

### Upstream
The original repository that a fork was created from.
Also used to describe the remote branch that a local branch
tracks. Add with `git remote add upstream URL`.

---

## V

### Version control
A system that records changes to files over time so you
can recall specific versions later. Git is the world's
most popular version control system.

---

## W

### Working tree
The actual files on your disk as they currently exist —
what you see in your file explorer and edit in your editor.
Changes here are not saved to Git history until staged
and committed.

### Workflow
In GitHub Actions, an automated process defined in a YAML
file in `.github/workflows/`. Triggered by Git events.
Contains one or more jobs which contain steps.

---

## Y

### YAML
Yet Another Markup Language. The file format used for
GitHub Actions workflow files. Uses indentation to define
structure. Files end in `.yml` or `.yaml`.

---

## Quick Term Lookup

| Term | Where explained |
|------|----------------|
| Alias | Chapter 3, Annex B |
| Bisect | Chapter 8 |
| Blame | Chapter 8 |
| Branch | Chapter 5 |
| Cherry-pick | Chapter 8 |
| CI/CD | Chapter 9 |
| Clone | Chapter 6 |
| Commit | Chapter 4 |
| Detached HEAD | Chapter 5 |
| Fetch | Chapter 6 |
| Fork | Chapter 7 |
| GitHub Actions | Chapter 9 |
| GitHub Flow | Chapter 7 |
| GitHub Pages | Chapter 10 |
| .gitignore | Chapter 4 |
| HEAD | Chapter 2 |
| Interactive rebase | Chapter 8 |
| Merge | Chapter 5 |
| Merge conflict | Chapter 5 |
| Origin | Chapter 6 |
| Pull | Chapter 6 |
| Pull request | Chapter 7 |
| Push | Chapter 6 |
| Rebase | Chapter 5 |
| Reflog | Chapter 8 |
| Remote | Chapter 6 |
| Repository | Chapter 4 |
| Revert | Chapter 8 |
| Reset | Chapter 8 |
| SHA hash | Chapter 2 |
| Squash | Chapter 7, Chapter 8 |
| Staging area | Chapter 4 |
| Stash | Chapter 8 |
| Tag | Chapter 2 |
| Tracking branch | Chapter 6 |
| Working tree | Chapter 4 |

---

*End of Annex D*

*If you encounter a term not listed here, it may be specific
to a particular tool or framework built on top of Git.
The official Git documentation at https://git-scm.com/doc
is the authoritative reference for all Git terminology.*

 