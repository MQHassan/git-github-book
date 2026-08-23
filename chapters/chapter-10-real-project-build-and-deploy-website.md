# Chapter 10 — Real Project — Build and Deploy a Website

## Learning Objectives

By the end of this chapter you will be able to:

- Apply every skill from this book in one complete project
- Build a professional website from scratch using HTML and CSS
- Host it live on the internet for free using GitHub Pages
- Set up automatic deployment using GitHub Actions
- Update the live site by simply pushing to GitHub
- Structure a multi-page website with navigation
- Use branches and pull requests for website changes
- Add a custom domain to your GitHub Pages site

---

## 10.1 Project Overview

In this chapter you build a real professional website
and deploy it live using everything you have learned.

### What you will build

A personal professional website with:
- A landing page with your name and introduction
- An about page with your background
- A projects page linking to your GitHub repos
- A contact page
- Navigation between all pages
- Automatic deployment on every push

### What you will use

| Technology | Purpose |
|------------|---------|
| HTML5 | Page structure and content |
| CSS3 | Styling and layout |
| Git | Version control |
| GitHub | Remote repository |
| GitHub Pages | Free website hosting |
| GitHub Actions | Automatic deployment |

### The end result

Your website will be live at:

https://YOUR-USERNAME.github.io


Accessible to anyone in the world, for free, forever.

---

## 10.2 Understanding GitHub Pages

GitHub Pages is a free static website hosting service
built into GitHub. It serves HTML, CSS and JavaScript
files directly from a repository.

### How it works

You push HTML files to GitHub
|
GitHub Pages detects the push
|
Files are served at your-username.github.io
|
Anyone in the world can visit your site


### Two types of GitHub Pages sites

| Type | URL | Repository name |
|------|-----|----------------|
| User site | username.github.io | Must be named `username.github.io` |
| Project site | username.github.io/project | Any repo name |

For a personal portfolio you want the user site —
your name as the URL.

---

## 10.3 Setting Up the Repository

### Step 1 — Create the repository on GitHub

Go to `https://github.com/new`

Repository name: `YOUR-USERNAME.github.io`

Replace `YOUR-USERNAME` with your actual GitHub username.
This exact naming is required for a user GitHub Pages site.

Settings:
- Public
- No README, no .gitignore, no licence

Click **Create repository**.

### Step 2 — Clone locally

cd ~/Desktop
git clone https://github.com/YOUR-USERNAME/YOUR-USERNAME.github.io.git
cd YOUR-USERNAME.github.io


Or if you prefer to initialise locally:

mkdir YOUR-USERNAME.github.io
cd YOUR-USERNAME.github.io
git init
git remote add origin https://github.com/YOUR-USERNAME/YOUR-USERNAME.github.io.git


---

## 10.4 Project Structure

Before writing any code, plan the structure:

YOUR-USERNAME.github.io/
index.html ← landing page (required)
about.html ← about page
projects.html ← projects page
contact.html ← contact page
css/
style.css ← all styling
images/
profile.jpg ← your photo
.github/
workflows/
deploy.yml ← auto-deploy workflow


`index.html` is the required entry point.
When someone visits your URL, GitHub Pages serves `index.html`.

---

## 10.5 Building the Landing Page

Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Your Name — Professional Title</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>

  <!-- Navigation -->
  <nav>
    <div class="logo">Your Name</div>
    <ul>
      <li><a href="index.html">Home</a></li>
      <li><a href="about.html">About</a></li>
      <li><a href="projects.html">Projects</a></li>
      <li><a href="contact.html">Contact</a></li>
    </ul>
  </nav>

  <!-- Hero section -->
  <section class="hero">
    <h1>Your Full Name</h1>
    <h2>Your Professional Title</h2>
    <p>
      A short introduction about yourself — what you do,
      what you are passionate about, and what makes you
      unique. Keep it to 2-3 sentences.
    </p>
    <a href="about.html" class="btn">Learn More</a>
    <a href="contact.html" class="btn-outline">Get in Touch</a>
  </section>

  <!-- Quick links -->
  <section class="quick-links">
    <h3>What I Do</h3>
    <div class="cards">
      <div class="card">
        <h4>Skill or Service 1</h4>
        <p>Brief description of this skill or service.</p>
      </div>
      <div class="card">
        <h4>Skill or Service 2</h4>
        <p>Brief description of this skill or service.</p>
      </div>
      <div class="card">
        <h4>Skill or Service 3</h4>
        <p>Brief description of this skill or service.</p>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer>
    <p>Your Name — Your Title</p>
    <p>
      <a href="https://github.com/YOUR-USERNAME">GitHub</a>
    </p>
  </footer>

</body>
</html>
```

---

## 10.6 Adding Styles

Create the `css` folder and `style.css`:

```css
/* Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Base */
body {
  font-family: Georgia, serif;
  background: #FAFAFA;
  color: #333;
  line-height: 1.6;
}

/* Navigation */
nav {
  background: #2C3E50;
  padding: 16px 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

nav .logo {
  color: white;
  font-size: 20px;
  font-weight: bold;
}

nav ul {
  list-style: none;
  display: flex;
  gap: 28px;
}

nav ul li a {
  color: white;
  text-decoration: none;
  font-size: 15px;
  transition: opacity 0.2s;
}

nav ul li a:hover {
  opacity: 0.75;
}

/* Hero */
.hero {
  background: linear-gradient(135deg, #2C3E50, #3498DB);
  color: white;
  padding: 100px 40px;
  text-align: center;
}

.hero h1 {
  font-size: 48px;
  margin-bottom: 12px;
}

.hero h2 {
  font-size: 24px;
  font-weight: normal;
  opacity: 0.85;
  margin-bottom: 20px;
}

.hero p {
  font-size: 18px;
  max-width: 600px;
  margin: 0 auto 36px;
  opacity: 0.9;
}

/* Buttons */
.btn {
  background: white;
  color: #2C3E50;
  padding: 14px 32px;
  border-radius: 30px;
  text-decoration: none;
  font-size: 16px;
  margin: 0 8px;
  display: inline-block;
  font-weight: bold;
}

.btn-outline {
  background: transparent;
  color: white;
  border: 2px solid white;
  padding: 12px 32px;
  border-radius: 30px;
  text-decoration: none;
  font-size: 16px;
  margin: 0 8px;
  display: inline-block;
}

/* Quick links */
.quick-links {
  padding: 70px 40px;
  text-align: center;
}

.quick-links h3 {
  font-size: 32px;
  margin-bottom: 40px;
  color: #2C3E50;
}

.cards {
  display: flex;
  flex-wrap: wrap;
  gap: 24px;
  justify-content: center;
}

.card {
  background: white;
  border-radius: 12px;
  padding: 32px 28px;
  width: 260px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.08);
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-4px);
}

.card h4 {
  font-size: 18px;
  color: #2C3E50;
  margin-bottom: 10px;
}

.card p {
  font-size: 14px;
  color: #666;
}

/* Footer */
footer {
  background: #2C3E50;
  color: #ccc;
  text-align: center;
  padding: 28px;
  font-size: 14px;
}

footer a {
  color: #3498DB;
  text-decoration: none;
}

/* Responsive */
@media (max-width: 768px) {
  nav {
    flex-direction: column;
    gap: 16px;
    padding: 16px;
  }

  nav ul {
    gap: 16px;
    flex-wrap: wrap;
    justify-content: center;
  }

  .hero {
    padding: 60px 20px;
  }

  .hero h1 {
    font-size: 32px;
  }

  .quick-links {
    padding: 40px 20px;
  }
}
```

---

## 10.7 Your First Commit

git add .
git commit -m "Add landing page and styles"
git push -u origin main


Visit `https://YOUR-USERNAME.github.io` in your browser.

GitHub Pages may take 1 to 2 minutes to deploy the first time.
Refresh after a couple of minutes and your site will be live.

---

## 10.8 Adding More Pages

Each additional page follows the same pattern.
Copy the navigation and footer from `index.html`
and add your content in between.

### about.html structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>About — Your Name</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>

  <nav>
    <!-- Same navigation as index.html -->
  </nav>

  <section class="page-hero">
    <h1>About Me</h1>
  </section>

  <section class="content">
    <h2>My Story</h2>
    <p>Write your professional background here.</p>

    <h2>Skills</h2>
    <ul>
      <li>Skill 1</li>
      <li>Skill 2</li>
      <li>Skill 3</li>
    </ul>

    <h2>Experience</h2>
    <p>Your career history here.</p>
  </section>

  <footer>
    <!-- Same footer as index.html -->
  </footer>

</body>
</html>
```

Add to `style.css`:
```css
.page-hero {
  background: #2C3E50;
  color: white;
  padding: 60px 40px;
  text-align: center;
}

.page-hero h1 {
  font-size: 40px;
}

.content {
  max-width: 800px;
  margin: 60px auto;
  padding: 0 40px;
}

.content h2 {
  font-size: 24px;
  color: #2C3E50;
  margin: 32px 0 12px;
}

.content p {
  font-size: 16px;
  color: #555;
  line-height: 1.8;
  margin-bottom: 16px;
}

.content ul {
  padding-left: 20px;
  color: #555;
  line-height: 2;
}
```

---

## 10.9 Using Branches for Changes

Every change to your live website should go through a branch.
This protects your site from broken code reaching production.

### Workflow for adding a new page

Step 1: Create a branch
git switch -c add-projects-page

Step 2: Create projects.html
[write the page]

Step 3: Commit
git add projects.html
git commit -m "Add projects page"

Step 4: Push the branch
git push origin add-projects-page

Step 5: Open a pull request on GitHub

Step 6: Review the changes

Step 7: Merge the PR

Step 8: Delete the branch
git switch main
git pull
git branch -d add-projects-page


### Why this matters for a website

If you push broken HTML directly to main, your live site
breaks immediately and visitors see an error.

With branches and pull requests:
- You can preview the change before it goes live
- You can easily revert if something breaks
- Your history shows exactly when and why each change was made

---

## 10.10 Setting Up Auto-Deploy

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Enable GitHub Pages in your repository settings:

Repository → Settings → Pages → Source → GitHub Actions


Now every push to main automatically rebuilds and deploys
your site within about 60 seconds.

---

## 10.11 Adding Your Profile Photo

Create an `images` folder and add your photo:

mkdir images


Copy your photo into the images folder then reference it
in your HTML:

```html
<img src="images/profile.jpg" alt="Your Name"
     style="width: 150px; height: 150px;
            border-radius: 50%; object-fit: cover;">
```

Or in CSS:
```css
.profile-photo {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid white;
  box-shadow: 0 4px 16px rgba(0,0,0,0.2);
}
```

---

## 10.12 Adding a Custom Domain

If you own a domain name (e.g. `yourname.com`), you can
point it to your GitHub Pages site for free.

### Step 1 — Add your domain on GitHub

Repository → Settings → Pages → Custom domain


Enter your domain: `yourname.com`

Click Save. GitHub creates a CNAME file in your repo.

### Step 2 — Configure your DNS

At your domain registrar (GoDaddy, Namecheap, etc.),
add these DNS records:

**For an apex domain (yourname.com):**

Type: A
Name: @
Values:
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153


**For www subdomain:**

Type: CNAME
Name: www
Value: YOUR-USERNAME.github.io


DNS changes take up to 48 hours to propagate.

### Step 3 — Enable HTTPS

Once the domain is verified:

Repository → Settings → Pages → Enforce HTTPS ✓


Your site is now served securely at `https://yourname.com`.

---

## 10.13 The Complete Project Workflow

Once your site is set up, maintaining it is simple:

### Adding a new article or page

git switch -c add-new-article
[create the new file]
git add .
git commit -m "Add article: [title]"
git push origin add-new-article
[open PR on GitHub]
[merge PR]
git switch main
git pull


Site updates automatically within 60 seconds of merging.

### Fixing a typo

git switch -c fix/typo-homepage
[fix the typo]
git add index.html
git commit -m "Fix typo in homepage hero text"
git push origin fix/typo-homepage
[open and merge PR]


### Emergency fix

git switch -c hotfix/broken-navigation
[fix the issue]
git add .
git commit -m "Fix broken navigation links"
git push origin hotfix/broken-navigation
[open and immediately merge PR]


---

## Chapter Summary

| Topic | Key point |
|-------|-----------|
| GitHub Pages | Free hosting for static HTML/CSS/JS sites |
| User site | Repo named `username.github.io` — served at that URL |
| index.html | Required entry point — GitHub Pages serves this first |
| Auto-deploy | Push to main → Actions runs → site updates in 60s |
| Branch workflow | Always use branches for changes to live sites |
| Custom domain | Point any domain to your GitHub Pages site for free |
| HTTPS | GitHub provides free SSL certificates automatically |

---

## Assessment — Test Yourself

**Question 1**
What is the required repository name for a personal
GitHub Pages user site? Why must it follow this format?

**Question 2**
You push changes to your GitHub Pages site but the live
site has not updated after 5 minutes. What do you check?

**Question 3**
Why should you use branches even for a personal website
that only you maintain?

**Question 4**
Your navigation bar HTML is identical on all four pages
of your website. Every time you add a new page you must
update four files. What is the professional solution
to this problem?

**Question 5**
You want your website to be accessible at both
`yourname.com` and `www.yourname.com`. What DNS records
do you need to configure?

---

## Answers

**Answer 1**
The repository must be named exactly `USERNAME.github.io`
where USERNAME matches your GitHub username exactly,
including capitalisation. GitHub uses this special naming
convention to identify which repository should be served
as the user's personal GitHub Pages site. Any other name
would create a project site at `username.github.io/repo-name`
instead of the root URL `username.github.io`.

**Answer 2**
Check in this order:
1. Actions tab — is the deployment workflow running?
   Did it pass or fail?
2. GitHub Pages settings — is Pages enabled and set to
   the correct source (main branch or GitHub Actions)?
3. Browser cache — hard refresh with Ctrl+Shift+R
   to bypass cached content
4. File names — is `index.html` correctly named?
   GitHub Pages is case-sensitive on some systems

**Answer 3**
Even for a solo personal site, branches provide:
- Safety — broken code on a branch does not break the live site
- Review opportunity — you can compare the branch against main
  before merging, catching mistakes before visitors see them
- History — the git log shows exactly what changed and when,
  making it easy to revert to a previous state
- Good habits — practising branch workflow on personal projects
  prepares you for team environments

**Answer 4**
The professional solution is to use a static site generator
or templating system — for example Jekyll (supported natively
by GitHub Pages), Hugo or Eleventy. These tools let you define
the navigation once in a template and render it across all pages
automatically. Alternatively, for a simple site, JavaScript can
inject shared components. For a pure HTML site, accept the
duplication but be disciplined about updating all pages together.

**Answer 5**
For the apex domain `yourname.com`:

Type: A
Name: @
Values: 185.199.108.153, 185.199.109.153,
185.199.110.153, 185.199.111.153


For the www subdomain:

Type: CNAME
Name: www
Value: YOUR-USERNAME.github.io


Then in GitHub Pages settings, set your custom domain
to `yourname.com`. GitHub will automatically redirect
`www.yourname.com` to `yourname.com`.
Enable HTTPS enforcement after the domain is verified.

---

## What is Next

In the final chapter we step back and look at the bigger
picture — professional Git practices, team conventions,
career advice and how to continue learning.

*Proceed to Chapter 11 — Best Practices*

 

