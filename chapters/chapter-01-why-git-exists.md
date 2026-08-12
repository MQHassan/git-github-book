# Chapter 1 — Why Git Exists

## Learning Objectives

By the end of this chapter you will be able to:

- Explain the problem that Git was created to solve
- Describe what version control means in plain English
- Understand the difference between Git and GitHub
- Identify real-world situations where Git is essential
- Explain why Git became the world standard for version control

---

## 1.1 The Problem — Life Before Git

Imagine you are writing an important research paper. You save it as:

research_final.docx

Then you make some changes. Not sure if they are good, so you save a copy:

research_final_v2.docx

Your supervisor suggests edits. You save another copy:

research_final_v2_supervisor_edits.docx

You make more changes. You save:

research_final_ACTUAL_FINAL.docx

Then you realise you preferred version 2. But which one was version 2?
And what exactly did you change between versions?

Now imagine this same chaos — but with 10 people all editing the same
document simultaneously. Who changed what? When? Why? Which version
is the real one?

This is the problem Git solves.

---

## 1.2 What is Version Control?

Version control is a system that:

- Records every change ever made to a file
- Remembers who made each change
- Remembers when each change was made
- Remembers why each change was made (through messages)
- Allows you to go back to any previous version at any time
- Allows multiple people to work on the same files without conflict

Think of it like this:

> A version control system is like a time machine combined with
> a detailed diary for your files.

---

## 1.3 What is Git?

Git is the world's most popular version control system. It was created
in 2005 by **Linus Torvalds** — the same person who created the Linux
operating system — to manage the development of Linux, which involved
thousands of developers around the world.

### Key facts about Git

| Fact | Detail |
|------|--------|
| Created | 2005 |
| Creator | Linus Torvalds |
| Type | Distributed version control system |
| Cost | Free and open source |
| Used by | Over 100 million developers worldwide |
| Runs on | Windows, Mac, Linux |

### What makes Git special

Before Git, version control systems were **centralised** — one server
held the master copy and everyone had to connect to it to work. If the
server went down, work stopped.

Git changed everything by being **distributed** — every developer has
a complete copy of the entire history on their own machine. You can
work on a plane with no internet, make dozens of commits, and sync up
later.

Centralised (old way): Distributed (Git's way):

[Server]                   [Your machine — full history]
/  |  \                         |

Dev1 Dev2 Dev3 [Team member — full history]
|
Everyone depends [Team member — full history]
on the server

---

## 1.4 What is GitHub?

Git and GitHub are **not the same thing**. This confuses almost every
beginner. Here is the clear distinction:

| | Git | GitHub |
|---|---|---|
| What it is | Software on your computer | A website on the internet |
| What it does | Tracks changes to your files | Hosts your Git repositories online |
| Who made it | Linus Torvalds (2005) | GitHub Inc, now owned by Microsoft (2008) |
| Cost | Always free | Free for public repos, paid plans for private |
| Can you use one without the other? | Yes — Git works without GitHub | No — GitHub needs Git |

A simple analogy:

> Git is like Microsoft Word — software that runs on your computer.
> GitHub is like Google Drive — a place to store and share your files online.
> You use Word to create and edit documents. You use Google Drive to back
> them up and share them with others.

---

## 1.5 Real World Use Cases

Git is not just for software developers. Here are real situations where
Git makes a significant difference:

### For researchers and academics
- Version control your research papers through multiple drafts
- Track every change to your data analysis scripts
- Collaborate with co-authors without emailing files back and forth
- Maintain a public portfolio of your research on GitHub

### For medical professionals
- Manage clinical guidelines that get updated regularly
- Track changes to protocols and standard operating procedures
- Maintain teaching materials that evolve over time
- Build a professional online presence for your practice

### For software developers
- Track every line of code ever written
- Work on new features without breaking the main product
- Review and approve changes before they go live
- Automate testing and deployment

### For writers and content creators
- Version control books, articles and scripts
- Experiment with different structures without losing the original
- Collaborate with editors and co-authors
- Publish content directly from a repository

### For business professionals
- Track changes to important documents and reports
- Maintain organised project histories
- Collaborate across teams and time zones

---

## 1.6 Why Git Won

There were other version control systems before Git — CVS, SVN,
Perforce, Mercurial. Git beat them all. Here is why:

| Reason | Explanation |
|--------|-------------|
| **Speed** | Git is incredibly fast — even on huge projects |
| **Distributed** | Every copy is complete — no single point of failure |
| **Branching** | Creating parallel versions is instant and cheap |
| **Free** | No cost, no licence, no restrictions |
| **Community** | GitHub built a social layer that made Git the standard |
| **Industry adoption** | Every major tech company uses Git |

Today Git is not optional for anyone working in technology. It is the
air the industry breathes.

---

## 1.7 The Mental Model to Remember

Before moving to the next chapter, fix this picture in your mind:
Git = time machine + parallel universe machine for your files

Time machine:
Go back to any point in your project's history
See exactly what changed between any two points
Recover anything that was ever saved

Parallel universe machine (branching):
Create an alternate version of your project
Experiment freely without affecting the original
Merge the best version back when ready

GitHub is where these timelines meet and are shared with the world.

---

## Chapter Summary

| Concept | Key point |
|---------|-----------|
| Version control | Records every change, who made it, when and why |
| Git | Free distributed version control system created in 2005 |
| GitHub | Website that hosts Git repositories online |
| Git vs GitHub | Git is the tool, GitHub is the platform |
| Why Git | Speed, distribution, branching, free, universal adoption |

---

## Assessment — Test Yourself

Answer these questions before checking the answers below.

**Question 1**
What problem does Git solve that saving files with different names
(v1, v2, final, ACTUAL_FINAL) does not?

**Question 2**
What is the difference between a centralised and a distributed
version control system?

**Question 3**
Your colleague says "I use GitHub to track my code changes."
What is technically incorrect about this statement?

**Question 4**
Name three professions outside software development that can
benefit from using Git.

**Question 5**
Linus Torvalds created Git in 2005. What was his original reason
for creating it?

---

## Answers

**Answer 1**
Git records the complete history of every change with the exact lines
that changed, who changed them, when, and a written message explaining
why. With named files you lose all this context — you cannot see what
changed between versions, who made changes, or why decisions were made.
Git also allows multiple people to work on the same files simultaneously
without overwriting each other's work.

**Answer 2**
A centralised system has one server that holds the master copy —
everyone must connect to it to work. If the server goes down, work stops.
A distributed system (like Git) gives every user a complete copy of
the entire history on their own machine. Work continues even without
internet access. There is no single point of failure.

**Answer 3**
GitHub does not track code changes — Git does. GitHub is a website that
hosts Git repositories online. The correct statement would be:
"I use Git to track my code changes and GitHub to host and share them."

**Answer 4**
Any three from: researchers, academics, medical professionals, writers,
content creators, business professionals, lawyers, educators, journalists,
data scientists, designers.

**Answer 5**
Linus Torvalds created Git to manage the development of the Linux kernel,
which involved thousands of developers around the world contributing code.
The existing version control systems were too slow, centralised and
limited for a project of that scale.

---

## What is Next

In Chapter 2 we learn the language of Git — every important term
explained in plain English with analogies. Understanding the vocabulary
makes everything that follows much easier.

*Proceed to Chapter 2 — The Language of Git*