# Introduction to Using Git and Branching

This guide explains the basic Git commands, focusing on how to use branches effectively.

## Prerequisite

Before starting, you need to install Git on your computer.
*   Download Git from the official website: [https://git-scm.com/downloads](https://git-scm.com/downloads)
*   If you need specific instructions for your operating system (MacOS, Windows, Linux), search online for "install git on [Your OS]".

## 1. `git clone`: Getting the Repository Locally

Cloning copies a repository from a remote server (like GitHub or a Monash GitLab server) to your local machine. The copy on your machine is called your "local repository".

To clone, open your terminal or command prompt, navigate (`cd`) to the directory where you want to store the project, and run:

```bash
git clone <repository_url>
```

**Choosing SSH vs. HTTPS:**

You typically have two URL options for cloning: HTTPS or SSH. You can usually find these URLs on the repository's main page, often under a "Code" or "Clone" button.

*   **HTTPS:** Looks like `https://github.com/user_name/repo-name.git`. Cloning via HTTPS might ask for your username and password or a personal access token for authentication.
*   **SSH:** Looks like `git@github.com:user_name/repo_name.git`. Using SSH requires you to set up an SSH key pair on your computer and add the *public* key to your profile on the Git hosting service (e.g., GitLab Profile > SSH Keys > Add new key). This allows passwordless authentication.

*   **Note on Monash GitLab:** Monash GitLab often requires or strongly recommends using SSH for authentication. Ensure your SSH key is set up and added to your Monash GitLab profile.

**Important:** After cloning, change your current directory (`cd`) into the newly created repository folder. Most subsequent Git commands must be run from *within* this repository directory.

```bash
cd <repository_name>
```

## 2. `git pull`: Syncing with Remote Changes

Pulling fetches updates from the remote repository and merges them into your current local branch. This command keeps your local repository synchronized with the remote version.

Use `git pull` when you want to get the latest changes made by others. Cloning downloads a *new* repository, while pulling updates an *existing* one.

```bash
git pull
```

Git knows where to pull from because the remote repository's URL (usually named `origin`) is stored in the local repository's configuration when you clone. (You can see this in the hidden `.git/config` file, but be careful editing it directly).

## 3. Branching: Working in Isolation

### What is a Branch?

Most repositories have a default branch, often named `main` or `master`, which usually holds the stable or production-ready code.

If multiple people make changes directly on the `main` branch simultaneously, it can lead to conflicts and instability. **Branching** solves this by allowing you to work on features or fixes in isolation.

Creating a branch essentially creates a separate line of development based on a snapshot of another branch (like `main`). You can make changes on your new branch without affecting `main` until you're ready to merge.

### `git branch`: Managing Branches

*   **Create a new branch:**

    ```bash
    git branch <new_branch_name>
    ```
    *(Example: `git branch feature-user-login`)*

*   **List all local branches:**

    ```bash
    git branch
    ```
    *(The current branch you are on will be marked with an asterisk `*`)*

### `git checkout`: Switching Branches

Creating a branch doesn't automatically switch to it. Use `checkout` to change your active working branch:

```bash
git checkout <branch_name>
```
*(Example: `git checkout feature-user-login`)*

*   **Shortcut:** You can create and switch to a new branch in one command:

    ```bash
    git checkout -b <new_branch_name>
    ```
    *(Example: `git checkout -b fix-navbar-bug`)*

## 4. Making Changes: Add, Commit

Once you are on your new branch, you can start making changes to the code. The typical workflow is: Modify files -> Stage changes -> Commit changes.

### `git add`: Staging Changes

Before you can permanently save ("commit") your changes, you need to tell Git which modified or new files you want to include in the next snapshot. This process is called **staging**.

*   **Stage a specific file:**

    ```bash
    git add <path/to/your/file>
    ```
    *(Example: `git add src/user.py`)*

*   **Stage all changes in the current directory and subdirectories:**

    ```bash
    git add .
    ```
    *(The `.` represents the current directory.)*

    **Caution:** Be careful with `git add .`. It stages *all* modified and new untracked files. This might include temporary files, build artifacts, or sensitive data you don't want in the repository. Consider using a `.gitignore` file to tell Git which files/patterns to ignore permanently.

### `git diff --staged`: Reviewing Staged Changes

Before committing, it's good practice to review exactly what changes you have staged:

```bash
git diff --staged
```

This command shows the differences between your last commit and what's currently in the staging area.

### `git reset`: Unstaging Changes

If you staged a file by mistake:

*   **Unstage a specific file:**

    ```bash
    git reset <path/to/your/file>
    ```

*   **Unstage all currently staged changes:**

    ```bash
    git reset
    ```

**Important:** By default, `git reset` only removes changes from the staging area. It *does not* discard the modifications in your actual files (your working directory).

### `git commit`: Saving Changes

Committing takes the changes currently in the staging area and saves them permanently to the repository's history on your current branch.

```bash
git commit -m "Your descriptive commit message"
```

*   **Commit Messages:** Always write clear, concise messages explaining the *purpose* of the change (e.g., "Fix login validation bug #123", "Implement user profile page"). Good messages are crucial for understanding the project's history.

## 5. `git push`: Sharing Your Branch

After committing your changes locally on your feature branch, you need to push the branch (and its commits) to the remote repository (e.g., GitLab/GitHub) so others can see it or so you can create a merge request.

```bash
git push <remote_name> <branch_name>
```

Typically, the remote name is `origin`:

```bash
git push origin <your_branch_name>
```
*(Example: `git push origin feature-user-login`)*

Note: you can also use `git push origin head` to push the current branch

If the branch doesn't exist on the remote yet, Git might show you a command like `git push --set-upstream origin <your_branch_name>` the first time. You can use that command.

## 6. Merging: Integrating Your Changes

Once your work on the branch is complete, tested, and pushed, you'll want to merge it back into the `main` (or another target) branch.

This is usually done via a **Merge Request** (GitLab terminology) or **Pull Request** (GitHub terminology).

*   Go to the GitLab/GitHub web interface for your repository.
*   You'll often see a prompt to create a Merge/Pull Request for your recently pushed branch.
*   Create the request, describing your changes.
*   Depending on the project's workflow, your request might need review and approval from teammates before it can be merged. This is a common best practice.
*   Once approved (if required), the changes can be merged via the web interface.

## 7. Merge Conflicts

Sometimes, when you try to merge, Git encounters a **merge conflict**. This happens if changes were made to the *same lines* in the *same file(s)* on both your branch and the target branch (e.g., `main`) since you created your branch. Git doesn't know which version to keep.

*   **Prevention:** Regularly pull changes from `main` into your feature branch (`git pull origin main` while on your feature branch) to integrate upstream changes often and resolve smaller conflicts sooner.
*   **Resolution:** Merge conflicts must be resolved manually.
    *   GitLab/GitHub often provide web-based tools to help resolve them during the Merge/Pull Request.
    *   Alternatively, pull the target branch, attempt the merge locally (`git merge main` while on your feature branch), and Git will mark the conflicting sections in the affected files. You then edit the files to fix the conflicts, `git add` the resolved files, and `git commit` to finalize the merge.

## Quick Workflow Summary

1.  `git clone <url>` (Only the first time)
2.  `git pull` (Start on `main`, make sure it's up-to-date)
3.  `git checkout -b <new_branch_name>` (Create and switch to a new branch)
4.  *Work:* Write/change code, test your changes.
5.  `git add <files>` or `git add .` (Stage your changes)
6.  `git commit -m "Message"` (Commit your staged changes)
7.  *Repeat steps 4-6 as needed.*
8.  `git push origin <new_branch_name>` (Push your branch to the remote)
9.  *Go to GitLab/GitHub:* Create a Merge Request / Pull Request.
10. *Wait for review/approval* (If applicable).
11. *Merge the request* (Usually via the web UI).
12. `git checkout main` (Switch back to the main branch locally)
13. `git pull` (Update your local `main` branch with the newly merged changes)
14. *Repeat from step 3 for the next task.*

## Example Scenario

```bash
# 1. Clone (if you haven't already)
# git clone git@gitlab.com:your-group/your-project.git
# cd your-project

# 2. Ensure main is up-to-date
git checkout main
git pull origin main

# 3. Create and switch to a new branch
git checkout -b feature-add-user-api

# 4. Work: Create/edit files (e.g., api.py, models.py)
#    ... write code ...
#    ... test code ...

# 5. Stage the changes
git add src/api.py src/models.py

# (Optional: Check what's staged)
# git diff --staged

# 6. Commit the changes
git commit -m "Implement initial User API endpoint"

# 4-6. More work, staging, and committing
#    ... more code changes ...
#    ... test again ...
git add src/api.py
git commit -m "Add input validation to User API"

# 8. Push the branch to the remote
git push origin feature-add-user-api

# 9-11. Go to GitLab/GitHub UI
#       - Create Merge Request for 'feature-add-user-api' into 'main'
#       - Wait for approval (if needed)
#       - Merge the request

# 12. Switch back to main locally
git checkout main

# 13. Pull the merged changes into your local main
git pull origin main

# Ready for the next task (go back to step 3)
```
