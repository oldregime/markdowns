# Git Learning Guide

## 📚 Index
1. [Introduction to Git](#introduction-to-git)
2. [Understanding the Git Tree](#understanding-the-git-tree)
3. [Essential Git Commands](#essential-git-commands)
4. [Rolling Back to Previous Versions](#rolling-back-to-previous-versions)
5. [Handling Production Builds for Websites](#handling-production-builds-for-websites)
6. [Branches in Git: Master vs. Main](#branches-in-git-master-vs-main)
7. [Using GitHub's Web Interface](#using-githubs-web-interface)
8. [Conclusion](#conclusion)

---

## Introduction to Git

**Git** is a distributed version control system that allows multiple developers to work on a project simultaneously without interfering with each other's changes. It tracks changes in source code during software development, enabling collaboration and version management.

---

## Understanding the Git Tree

The Git tree represents the structure of your project and its history. Each commit creates a new node in the tree, forming a directed acyclic graph (DAG). Here’s a simple diagram of a Git tree:

```
*   Commit 3 (HEAD -> main)
|  
*   Commit 2
|  
*   Commit 1
```

In this diagram:
- Each asterisk (*) represents a commit.
- The arrow (HEAD -> main) indicates that the current branch is `main`.

---

## Essential Git Commands

Here are some essential Git commands to get you started:

### 1. **Configuration**
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### 2. **Creating a Repository**
```bash
git init
```

### 3. **Cloning a Repository**
```bash
git clone <repository-url>
```

### 4. **Checking Status**
```bash
git status
```

### 5. **Adding Changes**
```bash
git add <file>          # Add a specific file
git add .               # Add all changes
```

### 6. **Committing Changes**
```bash
git commit -m "Commit message"
```

### 7. **Viewing Commit History**
```bash
git log
```

### 8. **Pushing Changes**
```bash
git push origin <branch-name>
```

### 9. **Pulling Changes**
```bash
git pull origin <branch-name>
```

### 10. **Creating a Branch**
```bash
git branch <branch-name>
```

### 11. **Switching Branches**
```bash
git checkout <branch-name>
```

### 12. **Merging Branches**
```bash
git merge <branch-name>
```

---

## Rolling Back to Previous Versions

Sometimes, you may need to revert to a previous version of your project. Here are a few methods to do so:

### 1. **Using `git checkout`**
To revert to a specific commit:
```bash
git checkout <commit-hash>
```

### 2. **Using `git revert`**
To create a new commit that undoes changes made by a previous commit:
```bash
git revert <commit-hash>
```

### 3. **Using `git reset`**
To reset your branch to a previous commit:
```bash
git reset --hard <commit-hash>  # Warning: This will discard all changes after the specified commit
```

---

## Handling Production Builds for Websites

When deploying a website, it's essential to manage your production builds effectively. Here are some best practices:

1. **Use a Separate Branch for Production**
   - Create a `production` branch to keep your production-ready code separate from development.

2. **Tagging Releases**
   - Use tags to mark specific commits as releases:
   ```bash
   git tag -a v1.0 -m "Release version 1.0"
   ```

3. **Deploying from the Production Branch**
   - Ensure that your deployment process pulls from the `production` branch to maintain stability.

---

## Branches in Git: Master vs. Main

Historically, the default branch in Git was called `master`. However, many projects have transitioned to using `main` as the default branch name to promote inclusivity. 

- **Master Branch**: The original default branch name in Git.
- **Main Branch**: The new default branch name adopted by many repositories.

### Changing the Default Branch Name
To change the default branch name from `master` to `main`:
```bash
git branch -m master main
git push -u origin main
```

---

## Using GitHub's Web Interface

GitHub provides a user-friendly web interface for managing repositories. Here’s how to use it:

### 1. **Creating a Repository**
- Go to GitHub and click on the "+" icon in the top right corner.
- Select "New repository."
- Fill in the repository name, description, and choose visibility (public/private).

### 2. **Uploading Files**
- Navigate to your repository.
- Click on "Add file" and select "Upload files."
- Drag and drop files or choose files from your computer.

### 3. **Creating a Branch**
- Go to the main page of your repository.
- Click on the branch dropdown (usually says `main`).
- Type the new branch name and click "Create branch."

### 4. **Making a Pull Request**
- After pushing changes to a branch, go to the "Pull requests" tab.
- Click "New pull request."
- Select the base and compare branches, then click "Create pull request."

### 5. **Reviewing Pull Requests**
- Review changes, leave comments, and approve or request changes.

---

## Conclusion

Git is a powerful tool for version control and collaboration in software development. By mastering the essential commands and understanding the concepts of branches and commits, you can effectively manage your projects and collaborate with others. Use this guide as a reference as you continue to learn and work with Git!