# Git Learning Guide

## 📚 Index
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

The Git tree represents the structure of your repository, showing commits, branches, and the relationship between them. Below is a simplified diagram of a Git tree:

```
*   Commit C (main)
|\
| * Commit B (feature)
|/
*   Commit A (main)
```

- **Commit A**: The initial commit on the main branch.
- **Commit B**: A commit on a feature branch that diverged from the main branch.
- **Commit C**: A commit that merges the feature branch back into the main branch.

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
git merge <branch-name>     # Merge a branch into the current branch
```

### 9. **Pushing Changes**
```bash
git push origin <branch-name>
```

### 10. **Pulling Changes**
```bash
git pull origin <branch-name>
```

---

## Rolling Back to Previous Versions

Git allows you to roll back to previous versions of your code easily. Here are some common use cases:

### 1. **Undoing Local Changes**
To discard changes in your working directory:
```bash
git checkout -- <file>
```

### 2. **Reverting a Commit**
To create a new commit that undoes the changes made by a previous commit:
```bash
git revert <commit-hash>
```

### 3. **Resetting to a Previous Commit**
To reset your branch to a specific commit (this will discard all changes after that commit):
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

When deploying a website, it's essential to manage your production builds effectively. Here’s a typical workflow:

1. **Create a Production Branch**
   ```bash
   git checkout -b production
   ```

2. **Merge Changes from Development**
   ```bash
   git checkout production
   git merge development
   ```

3. **Build Your Project**
   Use your build tool (e.g., Webpack, Gulp) to create a production build.

4. **Deploy to Server**
   Use FTP, SSH, or a CI/CD pipeline to deploy your build to the production server.

5. **Tagging Releases**
   Tag your releases for easy reference:
   ```bash
   git tag -a v1.0 -m "Release version 1.0"
   git push origin v1.0
   ```

---

## Understanding "master" and "main" Branches

Historically, the default branch in Git was called **master**. However, many projects have transitioned to using **main** as the default branch name to promote inclusivity. 

- **master**: The original default branch name.
- **main**: The new default branch name adopted by many repositories, including GitHub.

To rename your local branch from master to main:
```bash
git branch -m master main
```

To update the remote repository:
```bash
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
- Click on "Add file" and select "Upload files."
- Drag and drop files or choose files from your computer.
- Commit changes with a message.

### 3. **Creating a Branch**
- Go to the main page of your repository.
- Click on the branch dropdown (usually says "main").
- Type the new branch name and click "Create branch."

### 4. **Creating a Pull Request**
- After pushing changes to a branch, go to the "Pull requests" tab.
- Click "New pull request."
- Select the base and compare branches, then click "Create pull request."

### 5. **Reviewing Pull Requests**
- Review changes, add comments, and approve or request changes.

---

## Conclusion

Git is an essential tool for modern software development, enabling collaboration and version control. By mastering Git commands and workflows, you can effectively manage your projects and collaborate with others. Happy coding! 🚀