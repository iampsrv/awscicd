
# 📘 What is Version Control?

Version Control System (VCS) is a system that records changes to files over time so you can recall specific versions later.

**Types:**
- **Centralized VCS (CVCS)** – e.g., Subversion (SVN)
- **Distributed VCS (DVCS)** – e.g., Git

Git is the most widely used distributed version control system today.

---

## 🧠 Why Use Git?
- Tracks changes in code over time.
- Enables collaboration among developers.
- Facilitates branching, merging, and rollbacks.
- Supports distributed workflows (local repo + remote repo).

---

## 🗃️ Git Architecture Overview
- **Working Directory**: Local copy of the project.
- **Staging Area (Index)**: Where changes are prepared before committing.
- **Repository**:
  - **Local**: Contains full history.
  - **Remote**: Shared repository (e.g., GitHub, GitLab, CodeCommit).

---

## 🔧 Git Core Concepts

| Concept   | Description                              |
|-----------|------------------------------------------|
| Commit    | A snapshot of staged changes             |
| Branch    | Independent line of development          |
| Merge     | Combine changes from one branch to another |
| Clone     | Copy of a remote repository              |
| Pull      | Fetch and merge changes from remote      |
| Push      | Upload local commits to remote           |
| Stash     | Temporarily store uncommitted changes    |
| Tag       | Reference to a specific commit, often for releases |

---

## 🛠️ Essential Git Commands

### 🔹 Setup
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### 🔹 Initialize & Clone
```bash
git init
git clone <repo-url>
```

### 🔹 Staging & Committing
```bash
git status
git add file.txt
git add .
git commit -m "Message"
```

### 🔹 Branching
```bash
git branch
git branch new-feature
git checkout new-feature
git switch -c hotfix
```

### 🔹 Merging & Rebasing
```bash
git merge new-feature
git rebase main
```

### 🔹 Remote Operations
```bash
git remote add origin <url>
git push -u origin main
git pull origin main
```

### 🔹 Undoing Changes
```bash
git reset --soft HEAD~1
git reset --hard HEAD~1
git checkout -- file.txt
```

### 🔹 Stashing
```bash
git stash
git stash apply
```

---

## 🔁 Git Workflow Models

1. **Centralized Workflow**: One main branch (e.g., main)
2. **Feature Branch Workflow**: Developers create branches per feature and merge via PRs.
3. **Git Flow**: Standardized branching model (main, develop, feature/, release/, hotfix/).
4. **Trunk-Based Development**: Small, frequent commits to main using feature flags.

---

## 🔐 Git with SSH & HTTPS

- **HTTPS**: Requires username/password or PAT.
- **SSH**: More secure, uses public/private key pair.

```bash
ssh-keygen -t rsa -b 4096
```

Add SSH key to GitHub/CodeCommit/GitLab.

---

## 📦 Hosting Platforms

| Platform    | Features                               |
|-------------|----------------------------------------|
| GitHub      | Open source hosting, PRs, Actions CI/CD |
| GitLab      | Integrated DevOps lifecycle            |
| Bitbucket   | Jira integration, pipelines            |
| CodeCommit  | Secure Git hosting in AWS              |

---

## 📋 Best Practices

- Use meaningful commit messages
- Commit often, group logically
- Use branches for features/fixes
- Keep `main` production-ready
- Use `.gitignore`
- Tag releases with versions

### 📄 Sample .gitignore
```bash
# Python
__pycache__/
*.pyc

# Node.js
node_modules/
.env

# Java
target/
*.class
```

---

## 📈 Git in DevOps

- Used in CI/CD pipelines to trigger builds
- Stores IaC (Terraform, CloudFormation)
- Manages app source, configs, secrets
- Supports collaboration and audit trails

---

## 🧠 Common Git Mistakes

| Mistake                     | Fix                            |
|----------------------------|---------------------------------|
| Committing sensitive data  | `git filter-branch` or BFG tool |
| Large binary files         | Use Git LFS                     |
| Merge conflicts            | Resolve and commit              |
| Accidental push to main    | `git revert` or `git reset`     |

---

## 🔗 Helpful Resources
- [Git Official Docs](https://git-scm.com/doc)
- [Pro Git Book (free)](https://git-scm.com/book/en/v2)
- [GitHub Learning Lab](https://lab.github.com)
- [Git Cheatsheet](https://education.github.com/git-cheat-sheet-education.pdf)
