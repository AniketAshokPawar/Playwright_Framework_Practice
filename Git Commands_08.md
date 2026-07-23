# Git Commands Used in Automation Testing

## Git Workflow in Automation Projects

In automation projects, we generally follow a feature branch workflow.

```
GitLab Repository

        |
        |
        ↓

Main Branch

        |
        |
        ↓

Feature Branch

        |
        |
        ↓

Automation Development

        |
        |
        ↓

Merge Request

        |
        |
        ↓

Code Review + Pipeline

        |
        |
        ↓

Merge to Main
```

---

# 1. git clone

## Purpose

Clone/download repository from GitLab/GitHub to local machine.

## Command

```bash
git clone <repository-url>
```

## Example

```bash
git clone https://gitlab.company.com/project/playwright-framework.git
```

## Automation Usage

When joining a new project:

```bash
git clone repository-url

cd project-folder

npm install
```

---

# 2. git status ⭐⭐⭐⭐⭐

## Purpose

Check current working directory status.

## Command

```bash
git status
```

Shows:

- Modified files
- Untracked files
- Current branch
- Merge conflicts

## Example

```
On branch feature/login-test

Changes not staged:
 modified: LoginPage.ts
```

---

# 3. Daily Pull Latest Changes From Main Branch

## Scenario

Team members continuously push changes into main branch.

Before starting work, we update our local branch.

---

## Step 1: Switch to Main Branch

```bash
git checkout NXXE_DEV_New
```

---

## Step 2: Pull Latest Main Changes

```bash
git pull origin NXXE_DEV_New
```

Meaning:

Get latest code from GitLab main branch.

---

## Step 3: Switch Back to Feature Branch

```bash
git checkout Dev_New_AniketPawar
```

---

## Step 4: Merge Main Into Feature Branch

```bash
git merge NXXE_DEV_New
```

Purpose:

Bring latest main changes into our feature branch.

---

## Step 5: Check Status

```bash
git status
```

Used to check:

- Merge completed successfully
- Conflicts
- Pending changes

---

# 4. Creating Feature Branch

## Why Feature Branch?

We should not directly write automation code on main branch.

Example:

```
main

        |
        |
        ↓

feature/login-test
```

---

## Create New Branch

```bash
git checkout -b feature/login-test
```

This command:

1. Creates a new branch
2. Switches to that branch

---

# 5. Switching Branch

## Using checkout

```bash
git checkout branch-name
```

Example:

```bash
git checkout main
```

---

## Using switch

Modern Git alternative:

```bash
git switch branch-name
```

Example:

```bash
git switch feature/login-test
```

---

# 6. git add

## Purpose

Move changes into staging area before commit.

## Command

Add specific file:

```bash
git add LoginPage.ts
```

Add all files:

```bash
git add .
```

Flow:

```
Working Directory

        ↓

git add

        ↓

Staging Area
```

---

# 7. git commit

## Purpose

Save changes locally with a meaningful message.

## Command

```bash
git commit -m "Added login automation test"
```

Good commit messages:

```
Added student registration test

Fixed login locator issue

Updated test data handling
```

---

## Skip Git Hooks

Sometimes:

```bash
git commit -m "message" --no-verify
```

Purpose:

Skip pre-commit hooks.

---

# 8. git push

## Purpose

Upload local commits to remote repository.

## Command

```bash
git push origin feature/login-test
```

Flow:

```
Local Branch

      ↓

git push

      ↓

GitLab Branch
```

---

# 9. Merge Request Workflow

Typical automation workflow:

```
Create Feature Branch

        ↓

Develop Automation Script

        ↓

git add .

        ↓

git commit

        ↓

git push

        ↓

Create MR

        ↓

Code Review

        ↓

Pipeline Execution

        ↓

Merge
```

---

# 10. Changes After MR Review

## Scenario

Reviewer requests changes.

Steps:

Modify code

```bash
git add .
```

Commit:

```bash
git commit -m "Updated code based on review comments"
```

Push:

```bash
git push
```

The same MR gets updated automatically.

---

# 11. git stash ⭐⭐⭐⭐⭐

## Purpose

Temporarily save unfinished changes without committing.

## Scenario

Working on Branch 1:

```
Branch 1

Changes not completed
```

Need to switch branch.

Use:

```bash
git stash
```

Now:

```
Changes stored temporarily
```

Switch branch:

```bash
git checkout branch2
```

---

## Restore Stashed Changes

```bash
git stash pop
```

---

## Stash Untracked Files

If normal stash does not include new files:

```bash
git stash -u
```

---

# 12. Working With Multiple Branches

Example:

```
Branch 1

unfinished changes


Need to work on Branch 2
```

Steps:

```bash
git stash

git checkout branch2
```

Come back:

```bash
git checkout branch1

git stash pop
```

---

# 13. Accidental Code Changes on Main Branch

## Scenario

You wrote automation code on main branch accidentally.

Solution:

Create branch from current changes:

```bash
git checkout -b feature/new-test
```

Your changes move to the new branch.

---

# 14. Branch Behind Main Branch

## Scenario

GitLab shows:

```
Your branch is behind main branch
```

Solution:

Pull latest main:

```bash
git pull origin NXXE_DEV_New
```

Push updated branch:

```bash
git push --set-upstream origin feature-branch
```

---

# 15. git fetch

## Purpose

Download latest remote changes without merging.

Command:

```bash
git fetch
```

Difference:

```
git fetch

Download changes only


git pull

Download + merge changes
```

---

# 16. git diff

## Purpose

Check differences before commit.

Command:

```bash
git diff
```

Example:

Check what changes were made in automation files.

---

# 17. git log

## Purpose

View commit history.

Command:

```bash
git log
```

Example:

```
commit abc123

Added login test
```

---

# 18. git restore

## Purpose

Discard local changes.

Example:

Changed file:

```
LoginPage.ts
```

Restore:

```bash
git restore LoginPage.ts
```

---

# 19. git cherry-pick

## Purpose

Copy a specific commit from another branch.

Example:

```
Branch A

Commit X


Need same commit in Branch B
```

Command:

```bash
git cherry-pick commit-id
```

---

# Most Used Git Commands For Automation Engineers

Daily commands:

```bash
git status

git pull

git checkout

git checkout -b

git merge

git add .

git commit -m

git push

git stash

git stash pop
```

Additional useful commands:

```bash
git fetch

git diff

git log

git restore

git cherry-pick
```

---

# Interview Questions

## Q1. Explain your daily Git workflow.

Answer:

"In my project, we follow feature branch workflow. I first pull the latest changes from the main branch, create a feature branch, develop automation scripts, check changes using git status, stage changes using git add, commit changes with meaningful messages, and push the branch to GitLab. After that, I create a merge request for review and pipeline validation."

---

## Q2. How do you update your feature branch with latest main changes?

Answer:

"I checkout the main branch, pull the latest changes, switch back to my feature branch, and merge the main branch into my feature branch."

Commands:

```bash
git checkout main

git pull

git checkout feature-branch

git merge main
```

---

## Q3. What will you do if you have changes but need to switch branches?

Answer:

"I use git stash to temporarily save my changes, switch branches, and later restore changes using git stash pop."

---

## Q4. What happens after pushing changes to GitLab?

Answer:

"After pushing the feature branch, I create a merge request. The code is reviewed, pipeline validation is performed, and after approval the changes are merged into the main branch."
