# Chapter 11 — Best Practices

## Learning Objectives

By the end of this chapter you will be able to:

- Write commit messages that communicate clearly
- Choose the right branching strategy for your team
- Structure repositories professionally
- Use Git aliases to work faster
- Understand common team conventions
- Avoid the most costly Git mistakes
- Build a GitHub profile that impresses employers
- Plan your next steps for continued learning

---

## 11.1 The Mark of a Professional Git User

There is a significant difference between someone who
knows Git commands and someone who uses Git professionally.

The commands are the easy part. The hard part is judgment —
knowing when to commit, what to write, how to structure
your history, and how to work with others without causing
problems.

This chapter is about developing that judgment.

---

## 11.2 Writing Great Commit Messages

The commit message is a letter to your future self
and your teammates. It will be read months or years
from now when someone is trying to understand why
a decision was made.

### The golden rule

A commit message should complete this sentence:

> "If applied, this commit will..."

### The format

Short summary (under 72 characters)

Optional longer description if needed.
Explain WHY this change was made, not WHAT changed
(the diff shows what changed).

Include any relevant context:

Issue number being fixed
Why this approach was chosen
What alternatives were considered
Any side effects or limitations

### Good vs bad commit messages

| Bad | Good |
|-----|------|
| fix | Fix null pointer error in login validation |
| update | Update user dashboard to show recent activity |
| wip | Add draft of payment processing — not yet tested |
| changes | Remove deprecated getUserData function |
| stuff | Refactor database queries to use connection pooling |

### The seven rules of great commit messages

1. Separate subject from body with a blank line
2. Limit the subject line to 72 characters
3. Capitalise the subject line
4. Do not end the subject line with a period
5. Use the imperative mood ("Add" not "Added" or "Adding")
6. Wrap the body at 72 characters
7. Use the body to explain what and why, not how

### Conventional Commits

Many teams use the Conventional Commits specification —
a structured format for commit messages:

type(scope): description

feat(auth): add OAuth2 login with Google
fix(nav): correct mobile menu alignment
docs(readme): update installation instructions
refactor(db): simplify query builder
test(auth): add unit tests for login flow
chore(deps): update dependencies to latest


Types:
- `feat` — new feature
- `fix` — bug fix
- `docs` — documentation only
- `style` — formatting, no code change
- `refactor` — code restructuring
- `test` — adding tests
- `chore` — maintenance tasks

---

## 11.3 Branching Strategies

### GitHub Flow (recommended for most teams)

main is always deployable
Every change goes through a branch and PR
Simple, effective, works at any scale

main
|
+---> feature/user-auth -----> PR --> main
|
+---> fix/login-bug ----------> PR --> main
|
+---> docs/update-readme -----> PR --> main


**Best for:** Web applications, continuous deployment,
teams that deploy frequently.

### GitFlow (for versioned software)

main — production releases only
develop — integration branch
feature/* — new features
release/* — release preparation
hotfix/* — emergency production fixes


**Best for:** Mobile apps, libraries, versioned software
with scheduled releases.

### Trunk-Based Development

Everyone commits directly to main (or short-lived branches)
Feature flags hide incomplete features in production
Requires strong automated testing


**Best for:** Large experienced teams with strong CI/CD.

### Which to choose

| Team size | Deployment frequency | Recommended strategy |
|-----------|---------------------|---------------------|
| Solo | Whenever ready | GitHub Flow |
| Small (2-5) | Continuous | GitHub Flow |
| Medium (5-20) | Continuous | GitHub Flow |
| Large (20+) | Scheduled releases | GitFlow |
| Large (20+) | Continuous | Trunk-based |

---

## 11.4 Repository Structure Best Practices

### The essential files

Every professional repository should have:

repo/
README.md ← required — explains the project
.gitignore ← required — excludes unwanted files
LICENSE ← important for open source
CONTRIBUTING.md ← optional — how to contribute
CHANGELOG.md ← optional — history of changes


### Writing a great README

A README should answer five questions:

1. **What is this?** — One sentence description
2. **Why should I care?** — The problem it solves
3. **How do I install it?** — Step-by-step setup
4. **How do I use it?** — Basic usage examples
5. **How do I contribute?** — For open source projects

### README template

```markdown
# Project Name

One sentence describing what this project does.

## Why

The problem this project solves and why it matters.

## Installation

Step by step instructions to get this running.

## Usage

Basic usage examples with code snippets.

## Contributing

How others can contribute to this project.

## Licence

MIT Licence — see LICENSE file for details.
```

---

## 11.5 .gitignore Best Practices

### What to always ignore
Dependencies — regeneratable, often huge

node_modules/
vendor/
venv/

Build output — regeneratable

dist/
build/
*.pyc
pycache/

Environment and secrets — NEVER commit these

.env
.env.local
.env.production
*.pem
*.key
config/secrets.yml

Editor files — personal preference, not project files

.vscode/
.idea/
*.swp
.DS_Store
Thumbs.db

Logs — generated at runtime

*.log
logs/

Test coverage — generated

coverage/
.coverage


### Using gitignore.io

Generate a perfect `.gitignore` for your stack:

https://www.gitignore.io


Enter your languages and tools — it generates the complete
ignore file automatically.

### If you accidentally committed a secret

1. Rotate the secret immediately — assume it is compromised
2. Remove from tracking: `git rm --cached secret-file`
3. Commit the removal
4. Rewrite history to remove it from all past commits
   (use `git filter-branch` or BFG Repo Cleaner)
5. Force push the rewritten history
6. Notify your team

---

## 11.6 Git Aliases — Work Faster

Aliases save time on commands you type dozens of times
per day. Set them once and use them forever.

### Essential aliases

git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.last "log -1 HEAD"
git config --global alias.unstage "restore --staged"
git config --global alias.undo "reset HEAD~1 --mixed"


### Using aliases

git st instead of git status
git lg instead of git log --oneline --graph --all
git last instead of git log -1 HEAD
git unstage instead of git restore --staged
git undo instead of git reset HEAD~1 --mixed


### Viewing all aliases

git config --global --list | grep alias


---

## 11.7 The Most Costly Git Mistakes

Learn from others' mistakes so you do not make them yourself.

### Mistake 1 — Committing secrets

**What happens:** API keys, passwords or tokens committed
to a public repo are scraped by bots within minutes.
The credentials are compromised even after you delete them
because the history remains.

**Prevention:**
- Always add secrets to `.gitignore` before creating them
- Use environment variables, not hardcoded values
- Use tools like `git-secrets` to scan commits automatically

### Mistake 2 — Force pushing to shared branches

**What happens:** `git push --force` rewrites history on
the remote. Everyone who has pulled those commits now
has a diverged history. Their next push or pull fails.

**Prevention:**
- Never force push to `main` or any shared branch
- Use `git push --force-with-lease` instead — it fails
  if someone else has pushed since your last fetch
- Protect branches in GitHub settings

### Mistake 3 — Huge commits

**What happens:** One massive commit with 50 files changed
is impossible to review, impossible to revert cleanly,
and impossible to understand months later.

**Prevention:**
- Commit early and often
- Each commit should do one thing
- If you struggle to write a message, the commit is too big

### Mistake 4 — Committing to the wrong branch

**What happens:** You intended to work on `feature/x` but
forgot to switch — your commits went to `main`.

**Prevention:**
- Always run `git status` before committing to verify
  which branch you are on
- Fix: use `git reset` to move commits to the right branch

### Mistake 5 — Not pulling before pushing

**What happens:** You push and get rejected because the
remote has new commits. You pull, get merge conflicts,
panic and make things worse.

**Prevention:**
- Always `git pull` before starting work each day
- Pull before pushing at the end of the day

### Mistake 6 — Ignoring merge conflicts

**What happens:** During a conflict, the developer accepts
all changes without reading them. Broken code or duplicated
content reaches main.

**Prevention:**
- Read every conflict carefully
- Understand both versions before choosing
- Test the code after resolving

---

## 11.8 Code Review Best Practices

### As the author

- Keep pull requests small and focused
- Write a clear description explaining the why not the what
- Link to the relevant issue
- Add screenshots for UI changes
- Respond to all comments — even to say "good point, fixed"
- Do not take feedback personally — the code is being reviewed, not you

### As the reviewer

- Be specific — quote the line you are commenting on
- Be constructive — suggest improvements, not just criticism
- Distinguish blocking vs non-blocking comments
- Approve when satisfied — do not leave PRs in limbo
- Review promptly — do not let PRs wait more than 24 hours
- Praise good work — acknowledge clever solutions

### Comment tone

| Instead of | Say |
|------------|-----|
| "This is wrong" | "This could cause X issue when Y happens" |
| "Why did you do this?" | "Could you help me understand the approach here?" |
| "Fix this" | "Consider using X pattern here — it handles the edge case of Y" |
| "Bad code" | "I think we could simplify this by..." |

---

## 11.9 Building a GitHub Profile That Impresses

Your GitHub profile is your professional portfolio.
It works for you 24 hours a day.

### Profile essentials

- **Professional photo** — clear, well-lit headshot
- **Real name** — not a username
- **Bio** — one sentence: what you do and what you are
  passionate about
- **Website** — link to your portfolio or professional site
- **Location** — optional but builds trust
- **Company** — current employer or organisation

### Pinned repositories

Pin your best 6 repositories:

Profile → Customize your pins


Choose repos that show:
- Range of skills
- Real completed projects
- Good README files
- Recent activity

### The contribution graph

The green squares on your profile show your daily
Git activity. A consistent contribution graph signals
an active, engaged developer.

**How to keep it green:**
- Commit something every day — even documentation
- Work on personal projects
- Contribute to open source
- Write blog posts in a repo

### The profile README

Create a repo named exactly `YOUR-USERNAME` and add
a `README.md`. It appears at the top of your profile.

Use it to:
- Introduce yourself
- List your skills and tools
- Show your stats
- Link to your best work

Example profile README:
```markdown
# Hi, I am Dr M Quamrul Hassan

Senior Consultant Paediatrician & Neonatologist
Evercare Hospital Dhaka, Bangladesh

## What I do
- Paediatric and neonatal medicine since 1992
- Research in infectious diseases and child nutrition
- Medical education and teaching
- Building tools to improve patient care

## My work
- [Professional website](https://mqhassan.github.io)
- [Research portfolio](https://github.com/MQHassan/research-portfolio)
- [CV and Biodata](https://github.com/MQHassan/cv-biodata)

## Currently learning
- Python for medical data analysis
- GitHub Actions for automation
```

---

## 11.10 Your Next Steps

You have completed this book. Here is how to continue growing.

### Immediate actions (this week)

[ ] Create your GitHub profile README
[ ] Pin your best 6 repositories
[ ] Complete your GitHub profile
[ ] Push something every day this week
[ ] Open your first pull request on an open source project


### Short term (next month)

[ ] Learn the basics of one programming language
(Python is recommended for its versatility)
[ ] Build one complete project and deploy it
[ ] Contribute to one open source project
[ ] Read other people's code on GitHub
[ ] Set up CI/CD on one of your projects


### Medium term (next 6 months)

[ ] Build 3-5 portfolio projects
[ ] Learn SQL for data management
[ ] Understand basic web development (HTML, CSS, JavaScript)
[ ] Participate in Hacktoberfest (October)
[ ] Start a technical blog using GitHub Pages


### Resources for continued learning

| Resource | What you will learn |
|----------|---------------------|
| `https://learngitbranching.js.org` | Interactive Git branching visualiser |
| `https://ohshitgit.com` | How to fix common Git disasters |
| `https://www.atlassian.com/git` | Comprehensive Git tutorials |
| `https://docs.github.com` | Official GitHub documentation |
| `https://github.com/EbookFoundation/free-programming-books` | Free programming books |
| `https://exercism.org` | Practice programming with mentorship |

---

## 11.11 The Complete Git Command Reference

Everything you have learned in one place.

### Setup

git config --global user.name "Name"
git config --global user.email "email"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --list


### Starting a repository

git init Create a new repository
git clone URL Clone a remote repository


### Daily workflow

git status Check current state
git diff Show unstaged changes
git diff --staged Show staged changes
git add filename Stage a file
git add . Stage all changes
git commit -m "message" Commit staged changes
git log --oneline View commit history
git log --oneline --graph --all Visual branch history


### Undoing things

git restore filename Discard working tree changes
git restore --staged filename Unstage a file
git commit --amend Fix last commit
git revert SHA Safe undo — adds new commit
git reset --soft SHA Erase commits, keep staged
git reset --mixed SHA Erase commits, keep unstaged
git reset --hard SHA Erase commits and changes
git reflog View full HEAD history


### Branching

git branch List branches
git branch name Create a branch
git switch name Switch to a branch
git switch -c name Create and switch
git merge name Merge branch into current
git branch -d name Delete merged branch
git branch -D name Force delete branch
git merge --abort Cancel a merge


### Remotes

git remote add origin URL Connect to remote
git remote -v Show remotes
git push -u origin main First push with tracking
git push Push commits
git pull Fetch and merge
git fetch Fetch without merging


### Advanced

git stash Shelve changes
git stash pop Restore shelved changes
git cherry-pick SHA Copy a commit
git bisect start Start binary search
git blame filename Who changed each line
git rebase -i HEAD~N Interactive rebase


---

## Chapter Summary

| Practice | Why it matters |
|----------|---------------|
| Great commit messages | Future you and your team can understand the history |
| Small focused commits | Easy to review, revert and understand |
| Branch for everything | Main stays clean and deployable |
| Pull before pushing | Avoid rejected pushes and conflicts |
| Never commit secrets | Secrets in repos cannot be truly erased |
| Review code carefully | Catch bugs before they reach production |
| Build your profile | GitHub is your portfolio — keep it active |

---

## Final Assessment — The Complete Book Review

**Question 1**
Describe the complete GitHub Flow from creating a branch
to the code being live in production.

**Question 2**
You join a new team and notice their commit messages look like:
`feat(auth): add JWT token refresh endpoint`
What convention are they using and what are its benefits?

**Question 3**
A junior developer on your team accidentally committed their
AWS credentials to a public GitHub repository 10 minutes ago.
What is the correct sequence of actions?

**Question 4**
Your team has been working on a feature for three weeks.
The feature branch has 47 commits — most of them are "wip",
"fix", "more fixes". Before merging the PR, how do you clean
this up?

**Question 5**
You are a solo developer building a personal project.
Make the case for STILL using branches and pull requests
even though no one else will review your code.

---

## Final Answers

**Answer 1**
Complete GitHub Flow:
1. Create a branch from main: `git switch -c feature/name`
2. Make commits on the branch: `git add . && git commit -m "..."`
3. Push the branch: `git push origin feature/name`
4. Open a pull request on GitHub with title and description
5. CI runs automatically — tests and checks must pass
6. Teammates review the code — leave comments, request changes
7. Author addresses feedback — pushes new commits to the branch
8. Reviewers approve the PR
9. PR is merged into main (squash, merge commit or rebase)
10. Branch is deleted on GitHub and locally
11. GitHub Actions detects the push to main
12. Deployment workflow runs automatically
13. Code is live in production within minutes

**Answer 2**
They are using the Conventional Commits specification.

Benefits:
- Structured, machine-readable commit history
- Automated changelog generation from commit types
- Clear communication of the nature of each change
- Semantic versioning can be automated from commit types
- `feat` = minor version bump, `fix` = patch, breaking change = major
- Easier to filter and search commit history by type

**Answer 3**
Immediate actions in order:
1. Rotate the credentials immediately in AWS — generate new keys
   and deactivate the exposed ones. Do this FIRST — every second counts.
2. Delete the file from the working tree and commit the removal
3. Add the file to `.gitignore`
4. Rewrite Git history to remove the file from all commits
   (use BFG Repo Cleaner — faster and safer than filter-branch)
5. Force push the rewritten history to GitHub
6. Contact GitHub support to request a cache clear
7. Audit AWS logs for any unauthorized access during the exposure window
8. Notify the team and document what happened and how it was resolved

**Answer 4**
Use interactive rebase to squash the 47 commits:

git rebase -i HEAD~47

In the editor:
- Keep the first commit as `pick` with a proper message
- Change all remaining 46 commits to `fixup`

This combines all 47 commits into one clean commit with
a professional message that describes the complete feature.

If 47 is too many for one rebase, do it in stages:
rebase 20 at a time, squashing as you go.

**Answer 5**
The case for branches and PRs as a solo developer:

Safety — broken code on a branch cannot break your live site.
You can abandon a branch without any consequences to main.

Review opportunity — switching from a branch to the PR view
on GitHub forces you to look at your changes with fresh eyes.
You will catch mistakes you missed while writing the code.

History — the commit log becomes a journal of decisions.
Six months later you will understand why you made each change.

Habit building — the developers who thrive in teams are those
who practiced good habits before joining one. Solo projects
are the training ground.

Rollback — if a feature causes problems, reverting a single
merged PR is clean and simple. Reverting direct commits to
main is messier.

Portfolio — recruiters and collaborators look at how you work,
not just what you built. A clean branching history with
descriptive PR descriptions signals professional standards.

---

## Congratulations

You have completed Git and GitHub — From Zero to Professional.

You started with nothing and now you have:

- A complete understanding of how Git works internally
- Hands-on experience with every important Git command
- The ability to collaborate professionally using pull requests
- A working CI/CD pipeline using GitHub Actions
- A live website deployed on GitHub Pages
- A professional GitHub profile showcasing your work

Git is a tool. Like all tools, it gets easier with use.
The concepts in this book will become second nature as you
apply them every day.

Keep committing. Keep pushing. Keep learning.

*— End of Book —*

 


