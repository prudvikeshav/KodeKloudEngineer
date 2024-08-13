## Problem Statement

The Nautilus application development team has been working on a project repository _/opt/news.git_. This repo is cloned at _/usr/src/kodekloudrepos_ on storage server in Stratos DC. They recently shared the following requirements with DevOps team:

One of the developers is working on _feature_ branch and their work is still in progress, however there are some changes which have been pushed into the _master_ branch, the developer now wants to _rebase_ the _feature_ branch with the _master_ branch without loosing any data from the _feature_ branch, also they don't want to add any _merge commit_by simply merging the _master_ branch into the _feature_ branch. Accomplish this task as per requirements mentioned.

Also remember to push your changes once done.

## Solution

### 1. Login to the Storage Server and Gain Root Access

First, connect to the storage server and switch to the root user if required:

```bash
ssh natasha@ststor01
sudo su
```

### 2. Navigate to the Repository Directory

Change to the directory where the repository is cloned:

```bash
cd /usr/src/kodekloudrepos/news.git
```

### 3. Fetch the Latest Changes

Retrieve the latest updates from the remote repository to ensure you have the most recent commits:

```bash
git fetch origin
```

### 4. Verify the Current Branches

Check the branches available and confirm that you are on the correct feature branch:

```bash
git branch
```

**Expected Output:**

```
* feature
  master
```

### 5. Review the Current Log

Inspect the commit history to understand the current state of the branches:

```bash
git log --oneline
```

**Example Output:**

```
95bc45d (HEAD -> feature, origin/feature) Add new feature
9331fa7 initial commit
```

### 6. Rebase the Feature Branch onto the Master Branch

Rebase the feature branch on top of the latest master branch to apply your changes on top of the updated master branch:

```bash
git rebase origin/master
```

**Expected Result:**

```
Successfully rebased and updated refs/heads/feature.
```

### 7. Check the Status

Verify that the rebase was successful and there are no uncommitted changes:

```bash
git status
```

**Expected Output:**

```
On branch feature
nothing to commit, working tree clean
```

### 8. Review the Updated Log

Ensure that the feature branch now includes the latest changes from the master branch:

```bash
git log --oneline
```

**Example Output:**

```
2331415 (HEAD -> feature) Add new feature
ba289f1 (origin/master, master) Update info.txt
0e98a58 initial commit
```

### 9. Push the Rebased Feature Branch

Push the rebased feature branch to the remote repository. Since the history of the feature branch has been rewritten, you need to use the `--force` option:

```bash
git push origin feature --force
```

**Expected Output:**

```
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 36 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 295 bytes | 295.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/news.git
 + e2ba2f3...2331415 feature -> feature (forced update)
```
