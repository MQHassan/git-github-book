# Chapter 3 — Installation and Setup

## Learning Objectives

By the end of this chapter you will be able to:

- Install Git on Windows, Mac or Linux
- Verify the installation was successful
- Configure Git with your identity
- Set your preferred text editor
- Understand what each configuration setting does
- Fix the most common installation problems

---

## 3.1 Before You Install

Git is free software. You do not need to pay for it, register for
anything or provide personal information to download it.

The official source for Git is:

https://git-scm.com


Always download from this official source. Never from third-party
websites.

### What you will install

| Component | What it is |
|-----------|-----------|
| Git | The core version control software |
| Git Bash (Windows only) | A terminal that lets you use Unix commands on Windows |
| Git GUI (optional) | A basic graphical interface for Git |

---

## 3.2 Installing Git on Windows

### Step 1 — Download

Go to:

https://git-scm.com/download/win


The download starts automatically. Look for a file named:

Git-2.x.x-64-bit.exe


Download the 64-bit version for modern Windows machines.

### Step 2 — Run the installer

Double-click the downloaded `.exe` file. Windows may ask:
"Do you want to allow this app to make changes?"
Click **Yes**.

### Step 3 — Key installer choices

You will see approximately 10 screens. Most can be left at default.
These three screens matter:

**Screen: Choose default editor**
- Change from Vim to **Visual Studio Code**
- Vim is extremely difficult for beginners to exit

**Screen: Override the default branch name**
- Select **Override to main**
- This matches GitHub's default and is the modern standard

**Screen: Adjusting your PATH environment**
- Select **Git from the command line and also from 3rd-party software**
- This lets you use `git` in Command Prompt, not just Git Bash

All other screens — leave at recommended defaults and click Next.

### Step 4 — Complete the installation

Click **Install**. The process takes about 30 to 60 seconds.

On the final screen:
- Uncheck **View Release Notes**
- Click **Finish**

### Step 5 — Verify the installation

**Critical:** Close your current Command Prompt completely and open
a brand new one. Windows only recognises new programs in new windows.

In the new Command Prompt type:

git --version


You should see something like:

git version 2.45.0.windows.1


If you see this — Git is installed correctly.

### Common Windows installation problems

| Problem | Cause | Fix |
|---------|-------|-----|
| `git` is not recognised | Old Command Prompt still open | Close all windows, open a new one |
| `git` still not recognised | PATH not set correctly | Restart your computer |
| Download fails | Network issue | Try a different browser or network |

---

## 3.3 Installing Git on Mac

Mac users have two options:

### Option 1 — Xcode Command Line Tools (recommended for beginners)

Open Terminal (search for Terminal in Spotlight) and type:

git --version


If Git is not installed, Mac will automatically offer to install
the Xcode Command Line Tools. Click **Install** and wait for it
to complete. This installs Git automatically.

### Option 2 — Homebrew (recommended for power users)

If you have Homebrew installed:

brew install git


### Verify on Mac

git --version


You should see:

git version 2.x.x (Apple Git-xxx)


---

## 3.4 Installing Git on Linux

### Ubuntu / Debian

sudo apt update
sudo apt install git


### Fedora / Red Hat

sudo dnf install git


### Arch Linux

sudo pacman -S git


### Verify on Linux

git --version


---

## 3.5 First-Time Configuration

After installation, Git needs to know who you are. Every commit you
make will be permanently stamped with this information.

You only need to do this once per machine.

### Set your name

git config --global user.name "Your Full Name"


### Set your email

git config --global user.email "you@example.com"


### Set the default branch name

git config --global init.defaultBranch main


### Set your default editor

**For Visual Studio Code:**

git config --global core.editor "code --wait"


**For Notepad++ (Windows):**

git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"


**For Nano (Linux/Mac):**

git config --global core.editor nano


### What does --global mean?

The `--global` flag means this setting applies to ALL repositories
on your machine. Without it the setting only applies to the current
repository.

| Flag | Scope |
|------|-------|
| `--global` | All repos on your machine |
| `--local` (default) | Current repo only |
| `--system` | All users on the machine |

---

## 3.6 Verifying Your Configuration

After setup, confirm everything saved correctly:

git config --list


You should see output similar to:

user.name=Dr M Quamrul Hassan
user.email=mqh.pt@google.com
init.defaultbranch=main
core.editor=code --wait


To check a single setting:

git config user.name


---

## 3.7 Understanding the Configuration File

Your global Git configuration is stored in a file called `.gitconfig`
in your home directory.

On Windows:

C:\Users\YourName.gitconfig


On Mac/Linux:

~/.gitconfig


You can open and edit this file directly:

git config --global --edit


A typical `.gitconfig` file looks like:

[user]
name = Dr M Quamrul Hassan
email = mqh.pt@google.com
[init]
defaultBranch = main
[core]
editor = code --wait


---

## 3.8 Setting Up Visual Studio Code

Visual Studio Code (VS Code) is the recommended editor for working
with Git. It is free, powerful and integrates deeply with Git.

### Download VS Code

https://code.visualstudio.com


### Why VS Code for Git?

| Feature | Benefit |
|---------|---------|
| Built-in Git panel | Visual view of changes, staging and commits |
| Conflict resolution | Visual merge conflict editor |
| Markdown preview | See formatted output as you write |
| Integrated terminal | Run Git commands without leaving the editor |
| UTF-8 default | Files saved with correct encoding for GitHub |
| Extensions | Thousands of add-ons for every language |

### The `code` command

After installing VS Code, you can open it from the terminal:

code . ← open current folder in VS Code
code README.md ← open a specific file
code myproject/ ← open a specific folder


If `code` is not recognised in your terminal:
1. Open VS Code
2. Press `Ctrl+Shift+P`
3. Type: `Shell Command: Install 'code' command in PATH`
4. Press Enter
5. Close and reopen your terminal

---

## 3.9 Useful Git Aliases

Aliases are shortcuts for long commands. Set them once and use
them forever.

git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --all"


After setting these:

| Instead of | You can type |
|-----------|-------------|
| `git status` | `git st` |
| `git checkout` | `git co` |
| `git branch` | `git br` |
| `git log --oneline --graph --all` | `git lg` |

The `git lg` alias is particularly useful — it shows a beautiful
visual graph of your entire branch history.

---

## 3.10 Installation Checklist

Before proceeding to Chapter 4, confirm all of these:

[ ] git --version shows a version number
[ ] git config user.name returns your name
[ ] git config user.email returns your email
[ ] git config init.defaultBranch returns main
[ ] VS Code is installed
[ ] code . opens VS Code from the terminal


If any item is unchecked, go back to the relevant section
and complete it before proceeding.

---

## Chapter Summary

| Topic | Key point |
|-------|-----------|
| Official source | Always download Git from git-scm.com |
| Windows installer | 3 key choices: editor, branch name, PATH |
| Verification | Run `git --version` in a NEW terminal window |
| Configuration | Set name, email, branch and editor once with --global |
| VS Code | Recommended editor — UTF-8, integrated Git, terminal |
| Aliases | Shortcuts that save time on common commands |

---

## Assessment — Test Yourself

**Question 1**
After installing Git on Windows, you open your existing Command Prompt
and type `git --version`. You see `git is not recognised`.
What is the most likely cause and how do you fix it?

**Question 2**
What is the difference between `git config --global` and
`git config --local`?

**Question 3**
Why is it important to use the same email address for Git configuration
as the one you use for your GitHub account?

**Question 4**
You set up Git on your work laptop last year. Today you get a new
personal laptop. Do you need to run `git config --global` again?
Explain your answer.

**Question 5**
What does the command `git config --list` do and when would you use it?

---

## Answers

**Answer 1**
The most likely cause is that the existing Command Prompt window was
open before Git was installed. Windows only loads the updated PATH
(which includes Git) when a new terminal window is opened.
Fix: Close all Command Prompt windows completely and open a brand
new one. If the problem persists, restart the computer.

**Answer 2**
`git config --global` sets a configuration that applies to ALL
repositories on your machine. It is stored in a `.gitconfig` file
in your home directory. `git config --local` (the default when no
flag is specified) sets a configuration that only applies to the
current repository. It is stored in `.git/config` inside the repo.

**Answer 3**
GitHub uses your email address to connect your commits to your
GitHub profile. If the email in your Git config matches your GitHub
account email, your commits appear on your GitHub profile with
your avatar and username. If they do not match, commits appear
as unlinked contributions from an unknown author.

**Answer 4**
Yes. The `--global` configuration is stored per machine, not per
account. Your new laptop has no Git configuration. You must run
the `git config --global` commands again on every new machine you
use. However, your repositories on GitHub are unaffected — they
contain the full history from all your previous machines.

**Answer 5**
`git config --list` displays all current Git configuration settings —
name, email, editor, branch defaults, aliases and more. Use it to
verify your setup after configuring Git for the first time, or to
diagnose problems when Git behaves unexpectedly.

---

## What is Next

In Chapter 4 we create our very first Git repository from scratch.
Every concept from Chapters 1 and 2 becomes real as you type
your first commands and make your first commit.

*Proceed to Chapter 4 — Your First Repository*




