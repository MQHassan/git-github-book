# Chapter 9 — GitHub Actions and CI/CD

## Learning Objectives

By the end of this chapter you will be able to:

- Explain what CI/CD means and why it matters
- Understand the structure of a GitHub Actions workflow file
- Create your first workflow that runs automatically on push
- Understand triggers, jobs and steps
- Use the GitHub Actions marketplace
- Set up automated testing in a workflow
- Deploy a website automatically on every push
- Use secrets to store sensitive information securely
- Read and debug workflow run logs

---

## 9.1 What is CI/CD?

CI/CD stands for **Continuous Integration** and
**Continuous Deployment** (or Delivery).

### Continuous Integration (CI)

Every time a developer pushes code, automated checks run:
- Tests execute to verify nothing is broken
- Code style is checked for consistency
- Security vulnerabilities are scanned
- Build succeeds without errors

If any check fails, the team is notified immediately.
The problem is caught within minutes rather than days.

### Continuous Deployment (CD)

When code passes all checks and is merged to main,
it is automatically deployed to the live server.
No manual steps. No human error. Code goes from commit
to production automatically.

Without CI/CD: With CI/CD:

Push code Push code
| |
Hope it works Tests run automatically
| |
Manually test Checks pass or fail
| |
Manually deploy Auto-deploy if all pass
| |
Discover bugs in production Bugs caught before users see them


---

## 9.2 What is GitHub Actions?

GitHub Actions is GitHub's built-in CI/CD platform.
It is free for public repositories and has generous
free allowances for private repos.

When you push code, GitHub Actions:
1. Spins up a fresh virtual machine (Ubuntu, Windows or Mac)
2. Clones your repository onto it
3. Runs whatever commands you define
4. Reports success or failure
5. Optionally deploys your application

The entire process is defined in a YAML file that lives
inside your repository.

---

## 9.3 The Workflow File

Everything in GitHub Actions is controlled by a workflow file.

### Location

your-repo/
.github/
workflows/
main.yml ← your workflow file lives here


The file must be inside `.github/workflows/`. The filename
can be anything ending in `.yml` or `.yaml`.

### Basic structure

```yaml
name: My First Workflow

on:
  push:
    branches: [ main ]

jobs:
  my-job:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Say hello
        run: echo "Hello from GitHub Actions!"
```

---

## 9.4 Understanding the Structure

### name

```yaml
name: My First Workflow
```

The display name shown on the Actions tab on GitHub.
Can be anything descriptive.

### on — triggers

```yaml
on:
  push:
    branches: [ main ]
```

Defines what events start the workflow. Common triggers:

| Trigger | When it fires |
|---------|--------------|
| `push` | Every time commits are pushed |
| `pull_request` | When a PR is opened or updated |
| `schedule` | On a cron schedule (e.g. daily at midnight) |
| `workflow_dispatch` | Manually triggered from GitHub UI |
| `release` | When a new release is published |

Multiple triggers:
```yaml
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 9 * * 1'  # Every Monday at 9am
```

### jobs

```yaml
jobs:
  my-job:
    runs-on: ubuntu-latest
```

A workflow can have multiple jobs. Each job:
- Runs on its own fresh virtual machine
- Runs in parallel with other jobs by default
- Can be configured to depend on other jobs

Available runners:

| Runner | Operating system |
|--------|-----------------|
| `ubuntu-latest` | Latest Ubuntu Linux |
| `windows-latest` | Latest Windows Server |
| `macos-latest` | Latest macOS |

### steps

```yaml
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run a command
        run: echo "Hello!"
```

Steps run sequentially within a job. Two types:

**uses** — runs a pre-built action from the marketplace:
```yaml
uses: actions/checkout@v4
```

**run** — runs a shell command directly:
```yaml
run: echo "Hello from GitHub Actions!"
```

---

## 9.5 Your First Workflow

Let us create a real workflow step by step.

### Create the workflow file

In your terminal:

mkdir -p .github/workflows


Create the file:

code .github/workflows/main.yml


Paste this content:
```yaml
name: My First Workflow

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  check-repo:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: List all files
        run: ls -la

      - name: Show Git log
        run: git log --oneline -5

      - name: Say hello
        run: echo "Hello from GitHub Actions!"
```

### Commit and push

git add .github/
git commit -m "Add first GitHub Actions workflow"
git push


### View the workflow run

Go to your GitHub repo → click **Actions** tab.

You will see your workflow running. Click on it to see
each step's output.

---

## 9.6 Actions Marketplace

The `uses` keyword references pre-built actions from the
GitHub Actions Marketplace — thousands of ready-made
building blocks contributed by GitHub and the community.

### Common marketplace actions

| Action | What it does |
|--------|-------------|
| `actions/checkout@v4` | Downloads your repo onto the runner |
| `actions/setup-node@v4` | Installs Node.js |
| `actions/setup-python@v5` | Installs Python |
| `actions/upload-artifact@v4` | Saves files from the workflow |
| `actions/cache@v4` | Caches dependencies to speed up runs |

### Using an action

```yaml
steps:
  - name: Set up Node.js
    uses: actions/setup-node@v4
    with:
      node-version: '20'
```

The `with` keyword passes configuration to the action.

### Finding actions

Browse at:

https://github.com/marketplace?type=actions


---

## 9.7 Running Tests Automatically

The most common CI use case — run your test suite on
every push and pull request.

### Node.js example

```yaml
name: Node.js CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

### Python example

```yaml
name: Python CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: python -m pytest
```

### What happens when tests fail

If any step exits with an error, the workflow fails.
GitHub shows a red cross on the commit and the PR.
Merging can be blocked until tests pass.

---

## 9.8 Deploying GitHub Pages Automatically

You can deploy your website to GitHub Pages automatically
every time you push to main.

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload site
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v4
```

After pushing this workflow, every push to main
automatically rebuilds and deploys your site.
No manual steps needed ever again.

---

## 9.9 Using Secrets

Never put passwords, API keys or tokens directly in your
workflow files. They would be visible in your repo.

Instead, use GitHub Secrets — encrypted values stored
securely on GitHub.

### Creating a secret

GitHub repo → Settings → Secrets and variables → Actions
→ New repository secret


Add a secret called `MY_API_KEY` with your actual value.

### Using a secret in a workflow

```yaml
steps:
  - name: Deploy with API key
    run: deploy-script.sh
    env:
      API_KEY: ${{ secrets.MY_API_KEY }}
```

The `${{ secrets.MY_API_KEY }}` syntax references your
secret. The actual value is never visible in logs —
GitHub replaces it with `***`.

### Common secrets to store

| Secret name | What it stores |
|-------------|---------------|
| `DEPLOY_KEY` | SSH key for deployment |
| `DATABASE_URL` | Database connection string |
| `API_TOKEN` | Third-party service token |
| `SLACK_WEBHOOK` | Slack notification URL |

---

## 9.10 Multi-Job Workflows

Jobs run in parallel by default. Use `needs` to create
dependencies between jobs.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm test

  build:
    needs: test          ← only runs if test passes
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run build

  deploy:
    needs: build         ← only runs if build passes
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run deploy
```

This creates a pipeline:

test --> build --> deploy


If tests fail, build never runs.
If build fails, deploy never runs.

---

## 9.11 Environment Variables

Pass configuration to your workflow steps.

### Workflow-level variables

```yaml
env:
  NODE_ENV: production
  APP_VERSION: 1.0.0

jobs:
  build:
    steps:
      - run: echo "Building version $APP_VERSION"
```

### Step-level variables

```yaml
steps:
  - name: Run with config
    run: echo "Environment is $NODE_ENV"
    env:
      NODE_ENV: staging
```

### Built-in variables

GitHub provides many variables automatically:

| Variable | Value |
|----------|-------|
| `${{ github.repository }}` | owner/repo-name |
| `${{ github.sha }}` | Full commit SHA |
| `${{ github.ref }}` | Branch or tag ref |
| `${{ github.actor }}` | Username who triggered the run |
| `${{ github.event_name }}` | What triggered the workflow |

---

## 9.12 Reading Workflow Logs

When a workflow runs, every step produces output visible
in the Actions tab.

### Navigation

GitHub → Actions → [workflow run] → [job name] → [step name]


### What to look for when debugging

| Symptom | Where to look |
|---------|--------------|
| Workflow did not start | Check the trigger in `on:` |
| Step failed | Expand the failed step — read the error message |
| Wrong Node/Python version | Check the `setup-node` or `setup-python` step |
| Missing dependency | Check the install step output |
| Secret not working | Check secret name matches exactly |

### Re-running a workflow

If a workflow failed due to a flaky test or network error:

Actions → [failed run] → Re-run all jobs


---

## 9.13 Status Badges

Add a badge to your README showing the current build status:

```markdown
![CI](https://github.com/USERNAME/REPO/actions/workflows/main.yml/badge.svg)
```

This shows a green "passing" or red "failing" badge in your
README — immediately visible to anyone visiting your repo.

---

## Chapter Summary

| Concept | Key point |
|---------|-----------|
| CI/CD | Automated testing and deployment on every push |
| Workflow file | YAML file in `.github/workflows/` |
| Trigger | What event starts the workflow |
| Job | A set of steps running on one virtual machine |
| Step | A single command or marketplace action |
| `uses` | Run a pre-built marketplace action |
| `run` | Execute a shell command |
| Secrets | Encrypted values for sensitive data |
| `needs` | Create dependencies between jobs |

---

## Assessment — Test Yourself

**Question 1**
What is the difference between Continuous Integration
and Continuous Deployment?

**Question 2**
Your workflow file is at `.github/workflows/ci.yml`.
The workflow is not running when you push.
What are three possible causes?

**Question 3**
Why should you never put an API key directly in a workflow file?
What should you do instead?

**Question 4**
You have three jobs: `test`, `build` and `deploy`.
You want `build` to run only if `test` passes, and `deploy`
to run only if `build` passes. Write the `needs` configuration
for each job.

**Question 5**
What does `actions/checkout@v4` do and why is it almost
always the first step in every workflow?

**Question 6**
Your tests pass locally but fail in GitHub Actions.
Name four things you would check to diagnose the problem.

---

## Answers

**Answer 1**
Continuous Integration (CI) means automatically running
tests and checks every time code is pushed. The goal is
to catch bugs and integration problems immediately —
within minutes of the push — rather than discovering them
days later or in production.

Continuous Deployment (CD) means automatically deploying
code to the live server whenever it passes all CI checks
and is merged to the main branch. No manual deployment steps.
Code goes from a merged PR to production automatically.

Together CI/CD creates a pipeline where code is continuously
integrated, tested and delivered to users.

**Answer 2**
Three possible causes:
1. Wrong trigger — the `on:` section does not include `push`
   or specifies the wrong branch name
2. Wrong branch — the workflow runs on `master` but you are
   pushing to `main` (or vice versa)
3. Syntax error — the YAML file has an indentation or
   formatting error that prevents GitHub from parsing it.
   Check the Actions tab for a workflow file parse error.

**Answer 3**
If you put an API key directly in a workflow file, it is
visible to anyone who can view your repository — including
the public if the repo is public. Even in private repos,
everyone with access can see it. If the repo becomes public
or is compromised, the key is exposed permanently in
the Git history.

Instead, store sensitive values as GitHub Secrets:
Repository → Settings → Secrets and variables → Actions
Reference in the workflow as `${{ secrets.SECRET_NAME }}`
The value is encrypted and never visible in logs.

**Answer 4**
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: npm test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: npm run build

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: npm run deploy
```

**Answer 5**
`actions/checkout@v4` downloads your repository onto the
fresh virtual machine that GitHub Actions has spun up.
The machine starts completely empty — it has no knowledge
of your code. Without this step, all subsequent steps
would fail because there are no files to work with.
It is almost always the first step because everything
else — running tests, building the app, checking code
style — requires access to your source code.

**Answer 6**
Four things to check:
1. Node.js / Python version — the version in Actions may
   differ from your local version. Specify the exact version
   in `setup-node` or `setup-python`.
2. Environment variables — your local machine may have
   environment variables set that the Actions runner does not.
   Check if any tests depend on env vars.
3. Operating system differences — you may be on Windows locally
   but the runner is Ubuntu. File paths, line endings and
   commands differ.
4. Missing dependencies — your local machine may have globally
   installed packages that are not installed in the workflow.
   Check the install step and ensure all dependencies are
   in your requirements file.

---

## What is Next

In Chapter 10 we put everything together in a real project.
You will build and deploy a professional website using Git,
GitHub, GitHub Pages and GitHub Actions — applying every
skill from this book in one complete project.

*Proceed to Chapter 10 — Real Project — Build and Deploy a Website*

 

