markdown
# Annex A — Terminal and Markdown Quick Reference

## About This Annex

This annex is a practical reference card for two essential
tools used throughout this book — the terminal and Markdown.

Bookmark this page. Return to it whenever you need a quick
reminder of a command or syntax.

---

# Part 1 — Terminal Commands

## What is the Terminal?

The terminal (also called Command Prompt on Windows, or
Terminal on Mac and Linux) is a text-based interface for
communicating with your computer.

Instead of clicking with a mouse, you type commands.
Git is used entirely through the terminal.

---

## 1.1 Windows Command Prompt

### Navigation

| Command | What it does | Example |
|---------|-------------|---------|
| `cd foldername` | Move into a folder | `cd Desktop` |
| `cd ..` | Go up one folder level | `cd ..` |
| `cd C:\full\path` | Go to a specific path | `cd C:\Users\Hassan\Desktop` |
| `cd ~` | Go to home directory | `cd ~` |
| `dir` | List files and folders | `dir` |
| `dir /a` | List including hidden files | `dir /a` |

### Creating and deleting

| Command | What it does | Example |
|---------|-------------|---------|
| `mkdir foldername` | Create a new folder | `mkdir my-project` |
| `echo text > file` | Create a file with content | `echo Hello > readme.txt` |
| `echo text >> file` | Append text to a file | `echo World >> readme.txt` |
| `del filename` | Delete a file | `del readme.txt` |
| `rmdir foldername` | Delete an empty folder | `rmdir old-folder` |
| `rmdir /s foldername` | Delete folder and contents | `rmdir /s old-folder` |

### Reading files

| Command | What it does | Example |
|---------|-------------|---------|
| `type filename` | Print file contents | `type readme.txt` |
| `more filename` | Print file page by page | `more longfile.txt` |

### Useful shortcuts

| Shortcut | What it does |
|----------|-------------|
| `Tab` | Auto-complete folder and file names |
| `Up arrow` | Repeat previous command |
| `Ctrl+C` | Stop a running command |
| `cls` | Clear the terminal screen |
| `Esc` | Clear the current line |
| `F7` | Show command history |

---

## 1.2 Mac / Linux Terminal

### Navigation

| Command | What it does | Example |
|---------|-------------|---------|
| `cd foldername` | Move into a folder | `cd Desktop` |
| `cd ..` | Go up one folder level | `cd ..` |
| `cd ~/path` | Go to path from home | `cd ~/Desktop/my-project` |
| `cd ~` | Go to home directory | `cd ~` |
| `ls` | List files and folders | `ls` |
| `ls -la` | List including hidden files | `ls -la` |
| `pwd` | Print current directory path | `pwd` |

### Creating and deleting

| Command | What it does | Example |
|---------|-------------|---------|
| `mkdir foldername` | Create a new folder | `mkdir my-project` |
| `mkdir -p path` | Create nested folders | `mkdir -p .github/workflows` |
| `echo "text" > file` | Create file with content | `echo "Hello" > readme.txt` |
| `echo "text" >> file` | Append text to file | `echo "World" >> readme.txt` |
| `touch filename` | Create an empty file | `touch index.html` |
| `rm filename` | Delete a file | `rm readme.txt` |
| `rm -rf foldername` | Delete folder and all contents | `rm -rf old-folder` |

### Reading files

| Command | What it does | Example |
|---------|-------------|---------|
| `cat filename` | Print file contents | `cat readme.txt` |
| `less filename` | Print file page by page | `less longfile.txt` |
| `head filename` | Print first 10 lines | `head readme.txt` |
| `tail filename` | Print last 10 lines | `tail readme.txt` |

### Useful shortcuts

| Shortcut | What it does |
|----------|-------------|
| `Tab` | Auto-complete folder and file names |
| `Up arrow` | Repeat previous command |
| `Ctrl+C` | Stop a running command |
| `clear` | Clear the terminal screen |
| `Ctrl+L` | Clear the terminal screen |
| `Ctrl+A` | Jump to start of line |
| `Ctrl+E` | Jump to end of line |

---

## 1.3 Understanding File Paths

### Windows paths

C:\Users\Hassan\Desktop\my-project\index.html
| | | | | |
| | | | | file
| | | | folder
| | | folder
| | username
| separator
drive letter


### Mac / Linux paths

/home/hassan/Desktop/my-project/index.html
| | | | |
| | | | file
| | | folder
| | folder
| username
root


### Relative vs absolute paths

Absolute — full path from root:
C:\Users\Hassan\Desktop\my-project (Windows)
/home/hassan/Desktop/my-project (Mac/Linux)

Relative — path from current location:
my-project (if you are already in Desktop)
./my-project (same thing, explicit)
../my-project (go up one level then into my-project)


---

## 1.4 The > and >> Operators

These operators redirect output to a file.

| Operator | Behaviour | Use case |
|----------|-----------|---------|
| `>` | Creates file or **overwrites** existing | Starting fresh |
| `>>` | Creates file or **appends** to existing | Adding content |

echo Line 1 > file.txt ← creates file with "Line 1"
echo Line 2 > file.txt ← OVERWRITES — now only "Line 2"
echo Line 3 >> file.txt ← APPENDS — now has "Line 2" and "Line 3"


---

## 1.5 PowerShell Differences (Windows VS Code Terminal)

VS Code on Windows opens PowerShell by default, not
Command Prompt. Most commands are the same but a few differ:

| Action | Command Prompt | PowerShell |
|--------|---------------|-----------|
| Create file | `echo text > file` | `New-Item filename` |
| List files | `dir` | `ls` or `dir` |
| Clear screen | `cls` | `clear` or `cls` |
| Print file | `type filename` | `cat filename` |

---

# Part 2 — Markdown Reference

## What is Markdown?

Markdown is a simple formatting language that converts
plain text to formatted output. GitHub renders Markdown
files (`.md`) as beautifully formatted pages automatically.

You write plain text with simple symbols. GitHub does
the rest.

---

## 2.1 Headings

````markdown
# Heading 1 — largest
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6 — smallest
````

Rendered as:
- `#` = large bold heading
- `##` = medium heading
- `###` = smaller heading

---

## 2.2 Text Formatting

| Markdown | Result |
|----------|--------|
| `**bold text**` | **bold text** |
| `*italic text*` | *italic text* |
| `***bold and italic***` | ***bold and italic*** |
| `~~strikethrough~~` | ~~strikethrough~~ |
| `` `inline code` `` | `inline code` |

---

## 2.3 Lists

### Unordered list

````markdown
- Item one
- Item two
- Item three
  - Nested item
  - Another nested item
````

Also works with `*` or `+` instead of `-`.

### Ordered list

````markdown
1. First item
2. Second item
3. Third item
````

### Task list (checkboxes)

````markdown
- [x] Completed task
- [ ] Incomplete task
- [ ] Another task
````

Renders as interactive checkboxes on GitHub.

---

## 2.4 Links

### Basic link

````markdown
[Link text](https://example.com)
````

### Link with title (tooltip on hover)

````markdown
[Link text](https://example.com "Tooltip text")
````

### Link to a section in the same document

````markdown
[Go to Chapter 2](#chapter-2-the-language-of-git)
````

### Bare URL (auto-linked)

````markdown
https://github.com
````

---

## 2.5 Images

````markdown
![Alt text](path/to/image.jpg)
![Alt text](https://example.com/image.jpg)
````

### Image with link

````markdown
[![Alt text](image.jpg)](https://example.com)
````

### Controlling image size (HTML in Markdown)

````html
<img src="image.jpg" alt="Description" width="300">
````

---

## 2.6 Code

### Inline code

````markdown
Use `git status` to check the state.
````

### Code block (fenced)

````markdown
```
plain code block
no syntax highlighting
```
````

### Code block with syntax highlighting

````markdown
```python
def greet(name):
    return f"Hello, {name}!"
```
````

````markdown
```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
```
````

````markdown
```bash
git add .
git commit -m "Add feature"
git push
```
````

````markdown
```yaml
name: My Workflow
on: [push]
```
````

Common language identifiers:
`python`, `javascript`, `html`, `css`, `bash`, `yaml`,
`json`, `sql`, `markdown`, `java`, `c`, `cpp`

---

## 2.7 Tables

````markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Row 1    | Data     | Data     |
| Row 2    | Data     | Data     |
| Row 3    | Data     | Data     |
````

### Column alignment

````markdown
| Left | Centre | Right |
|:-----|:------:|------:|
| text | text   | text  |
````

- `:---` = left aligned (default)
- `:---:` = centre aligned
- `---:` = right aligned

---

## 2.8 Blockquotes

````markdown
> This is a blockquote.
> It can span multiple lines.

> Nested blockquote:
>> This is nested inside the first quote.
````

---

## 2.9 Horizontal Rule

Three or more hyphens, asterisks or underscores:

````markdown
---
***
___
````

All three render as a horizontal dividing line.

---

## 2.10 Escaping Special Characters

If you want to display a character that Markdown uses
for formatting, escape it with a backslash:

````markdown
\*not italic\*
\# not a heading
\` not code \`
\[not a link\]
````

---

## 2.11 HTML in Markdown

GitHub Markdown supports basic HTML tags when you need
more control:

````html
<br>                    Line break
<strong>bold</strong>   Bold text
<em>italic</em>         Italic text
<details>               Collapsible section
<summary>Title</summary>
Content here
</details>
````

### Collapsible section example

````html
<details>
<summary>Click to expand</summary>

Content that is hidden until clicked.
You can put any Markdown here.

</details>
````

---

## 2.12 GitHub-Specific Markdown

GitHub adds extra features beyond standard Markdown.

### Mentioning people

````markdown
@username
````

Tags a GitHub user — they receive a notification.

### Referencing issues and PRs

````markdown
#42           Reference issue or PR number 42
Closes #42    Close issue 42 when this PR merges
Fixes #42     Same as Closes
````

### Status badges

````markdown
![Build Status](https://github.com/USER/REPO/actions/workflows/main.yml/badge.svg)
````

### Emoji

````markdown
:rocket: :tada: :white_check_mark: :x: :warning:
````

Renders as: 🚀 🎉 ✅ ❌ ⚠️

---

## 2.13 Complete README Template

A README that follows best practices:

````markdown
# Project Name

Brief one-sentence description of what this project does.

![Build Status](badge-url-here)

## Overview

2-3 sentences explaining the problem this solves and
who it is for.

## Features

- Feature one
- Feature two
- Feature three

## Installation

\```bash
git clone https://github.com/username/project.git
cd project
npm install
\```

## Usage

\```bash
npm start
\```

Example:
\```javascript
const result = doSomething('input');
console.log(result);
\```

## Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` | `3000` | Server port |
| `DEBUG` | `false` | Enable debug mode |

## Contributing

1. Fork the repository
2. Create a feature branch: `git switch -c feature/name`
3. Commit your changes: `git commit -m "Add feature"`
4. Push to your fork: `git push origin feature/name`
5. Open a pull request

## Licence

MIT Licence — see [LICENSE](LICENSE) for details.

## Author

Your Name — [your-website.com](https://your-website.com)
GitHub: [@yourusername](https://github.com/yourusername)
````

---

## 2.14 Markdown Editors and Previews

### VS Code

Press `Ctrl+Shift+V` to open a Markdown preview panel.
Or click the preview icon in the top right of the editor.

### Online editors

| Tool | URL |
|------|-----|
| StackEdit | stackedit.io |
| Dillinger | dillinger.io |
| HackMD | hackmd.io |

### GitHub preview

Every `.md` file on GitHub has a **Preview** button at the
top of the file view. Click it to see the rendered output.

---

## Quick Reference Card

### Terminal — most used commands

cd foldername move into folder
cd .. go up one level
ls / dir list files
mkdir foldername create folder
echo text > file create file
echo text >> file append to file
cat / type filename read file
clear / cls clear screen
Tab autocomplete
Up arrow repeat last command
Ctrl+C stop command


### Markdown — most used syntax
Heading 1
Heading 2

bold
italic
inline code
link text
Show Image

bullet point
numbered list
 checkbox

blockquote
--- horizontal rule


### Code block languages

python javascript html css
bash yaml json sql


---

*End of Annex A*

 