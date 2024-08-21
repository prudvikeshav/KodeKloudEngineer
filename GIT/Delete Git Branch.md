## Problem Statement

The Nautilus developers are engaged in active development on one of the project repositories located at _/usr/src/kodekloudrepos/apps_. During testing, several test branches were created, and now they require cleanup. Here are the requirements provided to the DevOps team:

On the Storage server in Stratos DC, _delete_ a branch named _xfusioncorp_apps_ from the _/usr/src/kodekloudrepos/apps_ Git repository.

## Solution

### 1. Log In to the Storage Server and Switch to Root User

First, log in to the storage server and switch to the root user to ensure you have the necessary permissions:

```bash
ssh natasha@ststor01
sudo su
```

### 2. Navigate to the Repository Directory

Change to the directory where the Git repository is located:

```bash
cd /usr/src/kodekloudrepos/apps
```

### 3. Check Existing Branches

List the existing branches in the repository. If you encounter a "dubious ownership" error, you need to configure Git to recognize the directory as safe:

```bash
git branch
```

**Expected Output (if you see an error):**

```
fatal: detected dubious ownership in repository at '/usr/src/kodekloudrepos/apps'
To add an exception for this directory, call:

        git config --global --add safe.directory /usr/src/kodekloudrepos/apps
```

To resolve this, add the repository directory as a safe directory:

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/apps
```

Re-run the command to check branches:

```bash
git branch
```

**Expected Output:**

```
  master
* xfusioncorp_apps
```

This output indicates that you have two branches: `master` (the main branch) and `xfusioncorp_apps` (the branch you want to delete).

### 4. Switch to the `master` Branch

Before deleting a branch, you must be on a different branch. Switch to the `master` branch:

```bash
git checkout master
```

**Expected Output:**

```
Switched to branch 'master'
Your branch is up to date with 'origin/master'
```

### 5. Delete the `xfusioncorp_apps` Branch

Now that you are on the `master` branch, you can delete the `xfusioncorp_apps` branch:

```bash
git branch -d xfusioncorp_apps
```

**Expected Output:**

```
Deleted branch xfusioncorp_apps (was 02fb854).
```

- The `-d` flag deletes the branch if it has been fully merged into the `master` branch. If the branch is not merged and you still want to delete it, use `-D` instead of `-d`.

### 6. Verify Branch Deletion

To ensure the branch has been deleted successfully, list the branches again:

```bash
git branch
```

**Expected Output:**

```
* master
```

The `xfusioncorp_apps` branch should no longer be listed.
