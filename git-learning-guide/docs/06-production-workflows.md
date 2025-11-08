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

The Git tree represents the structure of your project and its history. Each commit creates a new node in the tree, which points to its parent commit(s). Here’s a simplified diagram of a Git tree:

```
*   Commit C (HEAD -> main)
|   Author: User
|   Date: Today
|   Message: Added new feature
|
*   Commit B
|   Author: User
|   Date: Yesterday
|   Message: Fixed bug
|
*   Commit A
    Author: User
    Date: Two days ago
    Message: Initial commit
```

In this diagram:
- Each commit is represented by an asterisk (*).
- The `HEAD` pointer indicates the current branch (in this case, `main`).
- Each commit has a message, author, and date.

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

Git allows you to roll back to previous versions of your project easily. Here are some common use cases:

### 1. **Undoing Changes in the Working Directory**
If you want to discard changes in your working directory:
```bash
git checkout -- <file>
```

### 2. **Unstaging Changes**
To unstage a file that you added:
```bash
git reset <file>
```

### 3. **Reverting a Commit**
To create a new commit that undoes the changes made by a previous commit:
```bash
git revert <commit-hash>
```

### 4. **Resetting to a Previous Commit**
To reset your branch to a previous commit (this will discard all changes after that commit):
```bash
git reset --hard <commit-hash>
```

---

## Handling Production Builds for Websites

When deploying a website, you may want to manage different environments (development, staging, production). Here’s a common workflow:

1. **Create a Production Branch**
   ```bash
   git checkout -b production
   ```

2. **Merge Changes from Development**
   ```bash
   git checkout production
   git merge development
   ```

3. **Deploy the Production Build**
   Use your deployment tool (like FTP, SSH, or CI/CD pipelines) to push the `production` branch to your live server.

4. **Tagging Releases**
   Tag your production releases for easy reference:
   ```bash
   git tag -a v1.0 -m "Release version 1.0"
   git push origin v1.0
   ```

---

## Branches in Git: Master vs. Main

Historically, the default branch in Git was called `master`. However, many projects have transitioned to using `main` as the default branch name to promote inclusivity. 

- **Master Branch**: The original default branch name in Git. It typically represents the stable version of the project.
- **Main Branch**: A newer, more inclusive term for the default branch. It serves the same purpose as `master`.

### Changing the Default Branch Name
To change the default branch name from `master` to `main`:
```bash
git branch -m master main
git push -u origin main
```

---

## Using GitHub's Web Interface

GitHub provides a user-friendly web interface for managing repositories. Here’s how to use it:

### 1. **Creating a New Repository**
- Go to GitHub and log in.
- Click on the "+" icon in the top right corner and select "New repository."
- Fill in the repository name, description, and choose visibility (public/private).
- Click "Create repository."

### 2. **Uploading Files**
- Navigate to your repository.
- Click on "Add file" and select "Upload files."
- Drag and drop files or choose files from your computer.
- Commit changes with a message.

### 3. **Creating a Branch**
- Go to the main page of your repository.
- Click on the branch dropdown (usually says `main`).
- Type the new branch name and click "Create branch."

### 4. **Creating a Pull Request**
- After pushing changes to a branch, go to the "Pull requests" tab.
- Click "New pull request."
- Select the base and compare branches, then click "Create pull request."

### 5. **Reviewing Pull Requests**
- Review changes, leave comments, and approve or request changes.

---

## Conclusion

This guide provides a comprehensive overview of Git, from basic commands to managing branches and using GitHub's web interface. By mastering these concepts, you'll be well-equipped to handle version control in your projects. Happy coding! 🚀