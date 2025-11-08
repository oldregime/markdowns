# Git Learning Guide

## Index
1. [Introduction to Git](#introduction-to-git)
2. [Understanding the Git Tree](#understanding-the-git-tree)
3. [Essential Git Commands](#essential-git-commands)
4. [Rolling Back to Previous Versions](#rolling-back-to-previous-versions)
5. [Handling Production Builds for Websites](#handling-production-builds-for-websites)
6. [Understanding "master" and "main" Branches](#understanding-master-and-main-branches)
7. [Using GitHub's Web Interface](#using-githubs-web-interface)
8. [Conclusion](#conclusion)

---

## Introduction to Git

**Git** is a distributed version control system that allows multiple developers to work on a project simultaneously without interfering with each other's changes. It tracks changes in source code during software development, enabling collaboration and version management.

---

## Understanding the Git Tree

The Git tree represents the structure of your project and its history. Each commit creates a new node in the tree, which points to its parent commit(s). Here’s a simple diagram of the Git tree:

```
*   Commit 3 (HEAD -> main)
|  
*   Commit 2
|  
*   Commit 1
|  
*   Commit 0 (Initial commit)
```

In this diagram:
- Each asterisk (*) represents a commit.
- The arrow (HEAD -> main) indicates the current branch and commit.

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

### 8. **Branching**
```bash
git branch <branch-name>   # Create a new branch
git checkout <branch-name>  # Switch to a branch
```

### 9. **Merging Branches**
```bash
git checkout main          # Switch to main branch
git merge <branch-name>    # Merge changes from another branch
```

### 10. **Pushing Changes**
```bash
git push origin <branch-name>
```

### 11. **Pulling Changes**
```bash
git pull origin <branch-name>
```

---

## Rolling Back to Previous Versions

Git allows you to roll back to previous versions of your project easily. Here are some common use cases:

### 1. **Undoing Local Changes**
If you want to discard changes in your working directory:
```bash
git checkout -- <file>
```

### 2. **Reverting a Commit**
To create a new commit that undoes the changes made by a previous commit:
```bash
git revert <commit-hash>
```

### 3. **Resetting to a Previous Commit**
To reset your branch to a specific commit (this will lose changes):
```bash
git reset --hard <commit-hash>
```

### 4. **Soft Reset**
To keep changes in your working directory but reset the commit history:
```bash
git reset --soft <commit-hash>
```

---

## Handling Production Builds for Websites

When deploying a website, you may want to create a production build. Here’s a typical workflow:

1. **Create a Production Branch**
   ```bash
   git checkout -b production
   ```

2. **Merge Changes from Main**
   ```bash
   git merge main
   ```

3. **Build the Project**
   Use your build tool (e.g., Webpack, Gulp) to create a production build.

4. **Deploy the Build**
   Push the production branch to your server or hosting service.

5. **Tagging Releases**
   Tag your production builds for easy reference:
   ```bash
   git tag -a v1.0 -m "Version 1.0 Release"
   git push origin v1.0
   ```

---

## Understanding "master" and "main" Branches

Historically, the default branch in Git was named **master**. However, many projects have transitioned to using **main** as the default branch name to promote inclusivity. 

- **master**: The traditional default branch.
- **main**: The new default branch name adopted by many repositories.

You can rename your default branch using:
```bash
git branch -m master main
git push -u origin main
```

---

## Using GitHub's Web Interface

GitHub provides a user-friendly web interface for managing repositories. Here’s how to use it:

### 1. **Creating a New Repository**
- Go to GitHub and click on the "+" icon in the top right corner.
- Select "New repository."
- Fill in the repository name, description, and choose visibility (public/private).
- Click "Create repository."

### 2. **Uploading Files**
- Navigate to your repository.
- Click on "Add file" > "Upload files."
- Drag and drop files or select them manually.
- Commit changes with a message.

### 3. **Creating a Branch**
- Go to the "Code" tab.
- Click on the branch dropdown (usually says "main").
- Type the new branch name and press Enter.

### 4. **Creating a Pull Request**
- After pushing changes to a branch, go to the "Pull requests" tab.
- Click "New pull request."
- Select the base and compare branches, then click "Create pull request."

### 5. **Reviewing and Merging Pull Requests**
- Review the changes and comments.
- Click "Merge pull request" to merge changes into the base branch.

---

## Conclusion

This guide provides a comprehensive overview of Git, from basic commands to handling production builds and using GitHub's web interface. By mastering these concepts, you'll be well-equipped to manage your projects effectively with Git. Happy coding! 🚀