# Chapter 6 — GitHub and Remotes

## Learning Objectives

By the end of this chapter you will be able to:

- Explain the difference between Git and GitHub clearly
- Create a GitHub account and set up your profile
- Create a remote repository on GitHub
- Connect your local repository to GitHub
- Push your commits to GitHub
- Pull changes from GitHub to your local machine
- Clone any repository from GitHub
- Understand the difference between fetch and pull
- Use personal access tokens for authentication

---

## 6.1 From Local to Global

So far everything you have done has lived on your own machine.
Your commits, your branches, your history — all local.

This chapter connects your local Git to the world.

Before this chapter: After this chapter:

Your machine only Your machine <--> GitHub
(the internet)


GitHub gives you:
- A backup of your entire project history in the cloud
- A URL you can share with anyone
- A platform for collaboration with other developers
- A public portfolio of your work
- Free website hosting (GitHub Pages)

---

## 6.2 Git vs GitHub — Final Clarification

By now you know the difference, but it is worth stating one
final time clearly:

Git GitHub

Software on your computer Website on the internet
Tracks changes locally Hosts repositories online
Works without internet Needs internet to access
Free, open source Free tier + paid plans
Created by Linus Torvalds Owned by Microsoft


You use Git to manage your code. You use GitHub to share it.

---

## 6.3 Creating a GitHub Account

### Step 1 — Go to GitHub

https://github.com


### Step 2 — Sign up

Click **Sign up** and provide:
- A valid email address
- A password
- A username

**Choosing your username:**
Your username becomes part of every repo URL you create:

https://github.com/YOUR-USERNAME/repo-name


Choose something professional — your name or a variation of it.
This will appear on your public profile and in your commit history.

### Step 3 — Verify your email

GitHub sends a verification email. Click the link to activate
your account.

### Step 4 — Complete your profile

Go to your profile settings and add:
- Your full name
- A professional photo
- A bio describing what you do
- Your website or portfolio URL
- Your location and company

A complete profile looks professional and builds credibility.

---

## 6.4 Creating a Repository on GitHub

### Step 1 — Click the + icon

In the top right corner of GitHub, click `+` then
**New repository**.

### Step 2 — Fill in the details

| Field | What to enter |
|-------|--------------|
| Repository name | Short, descriptive, use hyphens not spaces |
| Description | One sentence explaining what this repo is |
| Public / Private | Public for portfolio work, Private for sensitive projects |
| Initialize with README | Leave UNCHECKED if you have an existing local repo |
| Add .gitignore | Leave as None if you have an existing local repo |
| Choose a license | Optional — MIT is the most common open source licence |

### Step 3 — Click Create repository

GitHub creates an empty repository and shows you setup instructions.

> Important: If you already have a local repository with commits,
> do NOT check "Initialize with README". An initialised GitHub repo
> and your local repo will conflict when you try to push.

---

## 6.5 Connecting Local to Remote

After creating an empty GitHub repo, connect your local repo to it.

### The remote add command

git remote add origin https://github.com/USERNAME/repo-name.git


**Breaking this down:**

| Part | Meaning |
|------|---------|
| `git remote add` | Add a new remote connection |
| `origin` | The name for this remote (conventional name) |
| `https://...` | The URL of the GitHub repository |

### Verify the connection

git remote -v


Output:

origin https://github.com/USERNAME/repo-name.git (fetch)
origin https://github.com/USERNAME/repo-name.git (push)


Two lines appear — one for fetching (downloading) and one
for pushing (uploading). Both point to the same URL.

---

## 6.6 Pushing to GitHub

### Your first push

git push -u origin main


**Breaking this down:**

| Part | Meaning |
|------|---------|
| `git push` | Upload commits to the remote |
| `-u` | Set up tracking — remember this remote for future pushes |
| `origin` | The remote to push to |
| `main` | The branch to push |

The `-u` flag is only needed the first time. After that,
simply type:

git push


Git remembers where to push.

### Authentication

When you push for the first time, GitHub asks you to authenticate.

**Option 1 — Git Credential Manager (Windows)**
A window appears asking for your GitHub username and password.
Use your GitHub password here — or sign in via browser.

**Option 2 — Personal Access Token**
GitHub no longer accepts your account password directly in the
terminal on some systems. Instead you use a token.

To create a token:

GitHub → Profile photo → Settings → Developer settings
→ Personal access tokens → Tokens (classic)
→ Generate new token (classic)


Settings:
- Note: give it a descriptive name
- Expiration: 90 days
- Scopes: tick the `repo` checkbox

Click **Generate token**. Copy it immediately — GitHub shows
it only once.

When Git asks for your password in the terminal, paste the token.

### What happens after pushing

Visit your GitHub repo URL and refresh. You will see:
- All your files listed
- Your commit messages next to each file
- Your README rendered at the bottom
- Your commit count
- The time of your last commit

---

## 6.7 Understanding Remotes

### What is origin?

`origin` is just a name — a shortcut for the full GitHub URL.
Instead of typing the full URL every time you push or pull,
you use the name `origin`.

You could call it anything:

git remote add github https://github.com/...
git remote add myrepo https://github.com/...


But `origin` is the universal convention. Every Git tutorial,
every team, every company uses `origin` for the primary remote.
Stick with it.

### Multiple remotes

You can have more than one remote:

git remote add origin https://github.com/USERNAME/repo.git
git remote add backup https://gitlab.com/USERNAME/repo.git


Push to a specific remote:

git push origin main
git push backup main


This is useful for mirroring a repo to multiple platforms.

### View all remotes

git remote -v


### Remove a remote

git remote remove origin


### Rename a remote

git remote rename origin github


---

## 6.8 Pulling from GitHub

When changes exist on GitHub that you do not have locally,
you need to download them.

### git pull

git pull


This does two things in one step:
1. Downloads new commits from GitHub (fetch)
2. Merges them into your current branch (merge)

If there are no conflicts, the merge happens automatically.

### git fetch

git fetch


This only downloads the changes — it does NOT merge them.
Your working files are unchanged. You can review what arrived
before deciding to merge.

To see what was fetched:

git log origin/main --oneline


To merge after fetching:

git merge origin/main


### fetch vs pull — when to use each

| Situation | Use |
|-----------|-----|
| Trust the incoming changes, want them now | `git pull` |
| Want to review changes before merging | `git fetch` then inspect |
| Working in a team with frequent changes | `git fetch` first |
| Solo project, your own remote | `git pull` is fine |

The professional habit is `git fetch` first, check what changed,
then `git merge`. But `git pull` is perfectly acceptable for
most situations.

---

## 6.9 Cloning a Repository

Cloning downloads a complete copy of any repository — including
all commits, all branches and all history.

### Clone a public repo

git clone https://github.com/USERNAME/repo-name.git


This creates a new folder called `repo-name` on your machine
with everything inside it.

### Clone to a specific folder name

git clone https://github.com/USERNAME/repo-name.git my-folder


### What clone does automatically

| Action | What Git does |
|--------|--------------|
| Downloads all files | Complete working tree |
| Downloads all history | Every commit ever made |
| Sets up origin | Points to the cloned URL |
| Sets up tracking | Local main tracks origin/main |

After cloning you can immediately start working — `git add`,
`git commit`, `git push`.

### Cloning vs Forking

| | Clone | Fork |
|---|---|---|
| Where | Creates local copy | Creates GitHub copy |
| Push access | Only if you own the repo | Always — it is your fork |
| Use case | Work on your own repos | Contribute to others' repos |

---

## 6.10 Tracking Branches

When you push with `-u` or clone a repo, Git sets up
**tracking branches** — a link between your local branch
and the remote branch.

git push -u origin main


After this:
- Local `main` tracks `origin/main`
- `git push` knows to push to `origin/main`
- `git pull` knows to pull from `origin/main`

### Seeing tracking relationships

git branch -vv


Output:
main a3f9c2b [origin/main] Add readme file

`[origin/main]` shows which remote branch `main` is tracking.

---

## 6.11 The Complete Remote Workflow

### Starting a new project

Step 1: Create repo on GitHub (empty, no README)
Step 2: Create local folder and initialise Git
mkdir my-project
cd my-project
git init
Step 3: Create files and make commits
git add .
git commit -m "Initial commit"
Step 4: Connect to GitHub
git remote add origin https://github.com/USERNAME/my-project.git
Step 5: Push
git push -u origin main


### Daily workflow with a remote

Morning — get latest changes:
git pull

During the day — work normally:
git add .
git commit -m "What I did"

End of day — push your work:
git push


### Joining an existing project

Step 1: Clone the repo
git clone https://github.com/USERNAME/project.git
Step 2: Work on a branch
git switch -c my-feature
Step 3: Commit your work
git add .
git commit -m "Add my feature"
Step 4: Push your branch
git push origin my-feature
Step 5: Open a pull request on GitHub


---

## 6.12 Common Remote Problems and Fixes

### Problem: Push rejected

! [rejected] main -> main (fetch first)
error: failed to push some refs


**Cause:** Someone else pushed to the remote since your last pull.
Your local history is behind the remote.

**Fix:**

git pull
git push


### Problem: Authentication failed

remote: Invalid username or password


**Fix:** Use a Personal Access Token instead of your password.
See section 6.6 for how to create one.

### Problem: Remote origin already exists

error: remote origin already exists


**Fix:** Remove the existing remote and add the correct one:

git remote remove origin
git remote add origin https://github.com/USERNAME/repo.git


### Problem: Repository not found

ERROR: Repository not found


**Cause:** Wrong URL, or the repo is private and you are not
authenticated.

**Fix:** Check the URL carefully. Ensure you are logged in
and have access to the repo.

---

## Chapter Summary

| Command | What it does |
|---------|-------------|
| `git remote add origin URL` | Connect local repo to GitHub |
| `git remote -v` | Show all remote connections |
| `git remote remove name` | Remove a remote connection |
| `git push -u origin main` | First push — sets up tracking |
| `git push` | Push commits to tracked remote |
| `git pull` | Download and merge remote changes |
| `git fetch` | Download remote changes without merging |
| `git clone URL` | Download a complete repo from GitHub |
| `git branch -vv` | Show tracking branch relationships |

---

## Assessment — Test Yourself

**Question 1**
What is the difference between `git push` and `git push -u origin main`?
When do you use each?

**Question 2**
You run `git pull` and Git tells you there is a merge conflict.
What happened and what do you do next?

**Question 3**
What is the difference between `git fetch` and `git pull`?
Describe a situation where you would prefer `git fetch`.

**Question 4**
Your colleague emails you a GitHub repository link and asks
you to work on it. You do not have push access to their repo.
What is the correct workflow?

**Question 5**
You run `git push` and get the error:

! [rejected] main -> main (fetch first)

What caused this error and how do you fix it?

**Question 6**
Why should you NOT check "Initialize with README" on GitHub
when you already have a local repository with commits?

---

## Answers

**Answer 1**
`git push -u origin main` pushes your commits to the `main` branch
on the `origin` remote AND sets up tracking — telling Git that
your local `main` branch should track `origin/main` for future
pushes and pulls. Use this only the first time you push a branch.

After the first push, simply use `git push`. Git already knows
the tracking relationship and pushes to the correct remote
and branch automatically.

**Answer 2**
A merge conflict during `git pull` means the remote has changes
to the same lines of the same files that you have also changed
locally. Git cannot automatically merge them.

What to do:
1. Open the conflicted files — look for `<<<<<<<`, `=======`,
   `>>>>>>>` markers
2. Edit each file to resolve the conflict — keep the correct version
3. Remove all conflict markers
4. Stage the resolved files: `git add filename`
5. Complete the merge: `git commit`

**Answer 3**
`git fetch` downloads new commits from the remote but leaves your
working files and current branch completely unchanged.
`git pull` downloads AND immediately merges the remote changes
into your current branch — it is `git fetch` + `git merge` combined.

Prefer `git fetch` when:
- Working in a team and you want to review incoming changes first
- You are mid-way through work and not ready to merge
- You want to compare remote changes against your local work
  before deciding to integrate

**Answer 4**
The correct workflow is:
1. Fork the repository — creates your own copy on GitHub
2. Clone YOUR fork to your local machine
3. Create a branch for your changes
4. Make commits on your branch
5. Push your branch to YOUR fork
6. Open a pull request from your fork to the original repo
7. The repo owner reviews and merges your contribution

**Answer 5**
This error means the remote branch has commits that your local
branch does not have. Someone else pushed changes after your
last pull, so your local history is behind the remote.

Fix:

git pull

This downloads and merges the remote changes. If there are no
conflicts, then:

git push

If there are conflicts, resolve them first then push.

**Answer 6**
If you check "Initialize with README", GitHub creates an initial
commit in the remote repo. Your local repo also has commits.
These two histories have no common ancestor — Git cannot merge them.

When you try to push you get:

! [rejected] main -> main (non-fast-forward)


To avoid this entirely, always create an empty GitHub repo
(no README, no .gitignore, no licence) when connecting to an
existing local repo.

---

## What is Next

In Chapter 7 we explore the most important collaboration
feature on GitHub — Pull Requests. You will learn how teams
review, discuss and safely merge code changes.

*Proceed to Chapter 7 — Collaboration and Pull Requests*

 


