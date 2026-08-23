# Chapter 7 — Collaboration and Pull Requests

## Learning Objectives

By the end of this chapter you will be able to:

- Explain what a pull request is and why teams use them
- Open a pull request on GitHub
- Review and comment on a pull request
- Request changes and approve a pull request
- Merge a pull request using different strategies
- Understand GitHub Flow as a team workflow
- Fork a repository and contribute to open source
- Set up branch protection rules
- Close and delete branches after merging

---

## 7.1 Why Pull Requests Exist

In Chapter 5 you learned to merge branches locally on your
own machine. That works perfectly when you are working alone.

But in a team, merging directly is dangerous:
- No one reviews the changes before they go live
- Broken code can reach the main branch
- There is no discussion or documentation of why changes were made
- Junior developers can accidentally overwrite important code

The solution is the **Pull Request** — GitHub's most important
collaboration feature.

A pull request is a formal proposal to merge one branch into another.
It creates a space for:
- Code review
- Discussion and comments
- Automated testing
- Approval before merging

Nothing reaches `main` without going through this process.

---

## 7.2 What is a Pull Request?

A pull request (PR) is NOT a Git command. It is a GitHub feature.

Without pull requests: With pull requests:

Branch --> merge --> main Branch --> PR --> Review --> Approve --> merge --> main
(direct, no review) (reviewed, discussed, tested, approved)


When you open a pull request you are saying:

> "I have made changes on this branch. Please review them,
> discuss them, test them, and if they look good — merge them
> into main."

### What a pull request contains

- The list of commits being proposed
- A diff showing exactly what lines changed
- A title and description explaining the changes
- A conversation thread for comments
- A status showing if automated checks pass or fail
- An approval system for reviewers

---

## 7.3 The GitHub Flow

GitHub Flow is the most widely used team workflow in the world.
It is simple, clean and works for teams of any size.

Step 1: Create a branch from main
git switch -c feature/my-feature

Step 2: Make commits on the branch
git add .
git commit -m "Add my feature"

Step 3: Push the branch to GitHub
git push origin feature/my-feature

Step 4: Open a pull request on GitHub

Step 5: Team reviews the code
Comments, suggestions, approvals

Step 6: Merge the pull request into main

Step 7: Delete the branch
git branch -d feature/my-feature


Main is always deployable. Every change goes through review.
No exceptions.

---

## 7.4 Opening a Pull Request

### Step 1 — Push your branch

git switch -c feature/add-contact-page


Make your changes, commit them, then push:

git push origin feature/add-contact-page


### Step 2 — Go to GitHub

After pushing, GitHub shows a yellow banner:

feature/add-contact-page had recent pushes — Compare & pull request


Click **Compare & pull request**.

Alternatively go to the **Pull requests** tab and click
**New pull request**.

### Step 3 — Fill in the PR form

**Title:** A clear, concise description of what changed

Add contact page with email form


**Description:** Explain what you did and why. Use this template:
What this PR does

Adds a contact page with a working email form.

Why

Users need a way to get in touch. Currently there is no contact
information on the site.

How to test
Navigate to /contact
Fill in the form
Submit — you should see a confirmation message
Screenshots

[attach before/after screenshots if relevant]


**Reviewers:** Tag teammates who should review this

**Labels:** Tag the type of change (feature, bug fix, docs)

### Step 4 — Click Create pull request

The PR is now open. Reviewers are notified.

---

## 7.5 Reviewing a Pull Request

As a reviewer your job is to ensure the code is correct,
readable and safe before it merges into main.

### What to look for

| Area | Questions to ask |
|------|-----------------|
| Correctness | Does the code do what the PR description says? |
| Logic | Are there any bugs or edge cases not handled? |
| Readability | Is the code clear and well named? |
| Tests | Are there tests for the new functionality? |
| Security | Does this introduce any security risks? |
| Style | Does it follow the team's coding conventions? |

### Leaving comments

Click on any line in the **Files changed** tab to leave
an inline comment on that specific line.

Types of feedback:

| Type | When to use |
|------|------------|
| Praise | Something done particularly well |
| Question | You need clarification before approving |
| Suggestion | An improvement that is not blocking |
| Request changes | Something must be fixed before merging |

### Submitting your review

After reviewing click **Review changes** and choose:

- **Comment** — general feedback, no formal approval
- **Approve** — you are happy with the changes, ready to merge
- **Request changes** — changes must be made before merging

---

## 7.6 Responding to Review Feedback

As the author of a pull request, when a reviewer requests changes:

### Step 1 — Read the feedback carefully

Understand exactly what the reviewer wants changed.
If unclear, ask for clarification in the comment thread.

### Step 2 — Make the changes locally

git switch feature/add-contact-page


Make the requested changes, then commit:

git add .
git commit -m "Address review feedback - improve form validation"


### Step 3 — Push the update

git push origin feature/add-contact-page


The pull request automatically updates with the new commits.
You do not need to open a new PR.

### Step 4 — Reply to the reviewer

In the PR comment thread, reply to let the reviewer know
you have addressed their feedback. Tag them with `@username`.

### Step 5 — Re-review

The reviewer checks the changes and approves if satisfied.

---

## 7.7 Merging a Pull Request

Once approved, the PR is ready to merge. GitHub offers three
merge strategies:

### Strategy 1 — Create a merge commit

Creates a merge commit preserving full history


Keeps a complete record of the branch and when it was merged.
The history shows the branching clearly.

**Best for:** Teams that want full history preservation.

### Strategy 2 — Squash and merge

Combines all commits into one before merging


If your branch has 15 messy "work in progress" commits, squash
combines them all into one clean commit on main.

**Best for:** Keeping main history clean and readable.

### Strategy 3 — Rebase and merge

Replays commits on top of main — linear history


Creates a linear history without a merge commit.

**Best for:** Teams that prefer linear history.

### After merging

GitHub shows:

Pull request successfully merged and closed


Click **Delete branch** to remove the feature branch from GitHub.

Then locally:

git switch main
git pull
git branch -d feature/add-contact-page


This syncs your local main with the merged changes and
cleans up the local branch.

---

## 7.8 Draft Pull Requests

A draft pull request signals that the work is in progress
and not ready for review yet.

Use a draft PR when:
- You want early feedback on direction before completing the work
- You want automated tests to run while you continue working
- You are working on a long feature and want visibility

To open a draft PR:
- Click the dropdown arrow next to **Create pull request**
- Select **Create draft pull request**

When ready for review, click **Ready for review** to convert
it to a regular PR.

---

## 7.9 Forking and Contributing to Open Source

Forking allows you to contribute to any public repository on
GitHub even without write access to the original.

### The open source contribution workflow

**Step 1 — Fork the repository**

On the original repo's GitHub page, click **Fork**.
GitHub creates your own copy at:

https://github.com/YOUR-USERNAME/original-repo-name


**Step 2 — Clone YOUR fork**

git clone https://github.com/YOUR-USERNAME/original-repo-name.git
cd original-repo-name


**Step 3 — Add the original as upstream**

git remote add upstream https://github.com/ORIGINAL-OWNER/original-repo-name.git


Now you have two remotes:

origin — your fork (you can push here)
upstream — the original repo (you can only pull from here)


**Step 4 — Create a branch**

git switch -c fix/spelling-error


**Step 5 — Make your changes and commit**

git add .
git commit -m "Fix spelling error in README"


**Step 6 — Push to YOUR fork**

git push origin fix/spelling-error


**Step 7 — Open a pull request**

Go to the ORIGINAL repo on GitHub. GitHub detects your
recent push and offers:

Compare & pull request


Open the PR from your fork's branch to the original repo's main.

**Step 8 — Wait for review**

The original repo's maintainers review your PR. They may:
- Merge it as is
- Request changes
- Ask questions
- Close it if it does not fit the project

### Keeping your fork up to date

While you work, the original repo continues to get new commits.
Keep your fork in sync:

git fetch upstream
git switch main
git merge upstream/main
git push origin main


---

## 7.10 Branch Protection Rules

Branch protection rules prevent accidental or unauthorised
changes to important branches like `main`.

### Setting up branch protection

On GitHub:

Repository → Settings → Branches → Add branch protection rule


Branch name pattern: `main`

Common protection rules:

| Rule | What it does |
|------|-------------|
| Require pull request before merging | Nobody can push directly to main |
| Require approvals | PR needs X approvals before merging |
| Require status checks to pass | CI tests must pass before merging |
| Require linear history | No merge commits — squash or rebase only |
| Include administrators | Even repo owners must follow the rules |

### Why branch protection matters

Without protection:
- Anyone can push broken code directly to main
- One mistake can break the entire application
- There is no audit trail of who approved what

With protection:
- Every change is reviewed before merging
- Automated tests must pass
- Main is always in a working state

---

## 7.11 GitHub Issues

Issues are GitHub's built-in task tracker. They work alongside
pull requests to organise your project.

### Creating an issue

Click **Issues** → **New issue**

A good issue contains:
- A clear title
- A description of the problem or feature request
- Steps to reproduce (for bugs)
- Expected vs actual behaviour (for bugs)
- Screenshots if relevant

### Linking issues to pull requests

In your PR description, reference the issue:

Closes #42


When the PR merges, GitHub automatically closes issue #42.

Other keywords:

Fixes #42
Resolves #42


### Issue labels

Organise issues with labels:

| Label | Use |
|-------|-----|
| `bug` | Something is broken |
| `feature` | New functionality |
| `documentation` | Docs need updating |
| `good first issue` | Suitable for new contributors |
| `help wanted` | Maintainers need assistance |

---

## Chapter Summary

| Concept | Key point |
|---------|-----------|
| Pull request | A proposal to merge a branch — with review |
| GitHub Flow | Branch → commit → push → PR → review → merge |
| Code review | Check correctness, logic, readability, security |
| Merge strategies | Merge commit, squash, rebase — choose based on team preference |
| Draft PR | Work in progress — not ready for review yet |
| Fork | Your own copy of someone else's repo |
| Upstream | The original repo you forked from |
| Branch protection | Rules preventing direct pushes to important branches |
| Issues | GitHub's built-in task and bug tracker |

---

## Assessment — Test Yourself

**Question 1**
What is the difference between merging locally with `git merge`
and merging via a GitHub pull request?

**Question 2**
Your team uses the squash and merge strategy.
You made 8 commits on your feature branch before opening a PR.
How many commits appear on main after the PR is merged?

**Question 3**
A reviewer leaves this comment on your PR:
"This function will crash if the input is empty.
Please add input validation."
Describe the exact steps you take to respond.

**Question 4**
You want to contribute a bug fix to a popular open source
project. You do not have write access to the repo.
Describe the complete workflow from start to finish.

**Question 5**
Your team has set up branch protection requiring 2 approvals
before merging. You are a repository administrator.
Can you merge a PR with only 1 approval?

**Question 6**
What does writing `Closes #15` in a pull request description do?

---

## Answers

**Answer 1**
`git merge` combines branches locally with no review process —
changes go directly into the branch with no discussion or approval.
A GitHub pull request creates a formal review process where
teammates can examine every changed line, leave comments,
request modifications, run automated tests and formally approve
before anything merges. Pull requests also create a permanent
record of what was changed, who reviewed it, what was discussed
and when it was merged — invaluable for team accountability.

**Answer 2**
One commit. Squash and merge combines all commits on the feature
branch into a single commit before merging into main.
The 8 individual commits disappear from main's history — only
one clean commit representing the entire feature appears.
The original commits still exist in the branch history and
in `git reflog` but are not visible in main's linear history.

**Answer 3**
Step 1 — Read the comment carefully and understand exactly what
validation is needed.
Step 2 — Switch to the feature branch locally:
`git switch feature/my-feature`
Step 3 — Make the requested changes — add input validation
Step 4 — Commit the fix:
`git add .`
`git commit -m "Add input validation to handle empty input"`
Step 5 — Push the update:
`git push origin feature/my-feature`
Step 6 — Reply to the reviewer in the PR comment thread:
"@reviewer — added input validation in the latest commit.
The function now returns an error message if input is empty."
Step 7 — Wait for the reviewer to check the changes and re-approve.

**Answer 4**
1. Go to the original repo on GitHub and click Fork
2. Clone YOUR fork: `git clone https://github.com/YOU/repo.git`
3. Add upstream: `git remote add upstream https://github.com/OWNER/repo.git`
4. Create a branch: `git switch -c fix/bug-description`
5. Make the fix and commit: `git commit -m "Fix bug description"`
6. Push to your fork: `git push origin fix/bug-description`
7. Go to the ORIGINAL repo on GitHub
8. Open a pull request from your fork's branch to original/main
9. Fill in the PR description explaining the fix
10. Wait for maintainers to review and respond

**Answer 5**
It depends on how the branch protection rule is configured.
If the rule includes **Include administrators**, then no —
even administrators must follow the rules and cannot merge
with only 1 approval.
If **Include administrators** is not checked, then yes —
administrators can bypass the requirement.
Best practice is to include administrators in the rules to
ensure consistent standards across the entire team.

**Answer 6**
Writing `Closes #15` in a pull request description creates a
link between the PR and issue number 15. When the PR is merged
into the default branch, GitHub automatically closes issue 15
and shows the PR as the resolution. This keeps your project
board clean and creates a clear connection between the problem
(the issue) and the solution (the pull request).

---

## What is Next

In Chapter 8 we learn the advanced Git commands that rescue
you when things go wrong — stash, reset, revert, reflog
and cherry-pick. These are the commands that separate
confident Git users from beginners.

*Proceed to Chapter 8 — Advanced Git — Rescue Commands*

 