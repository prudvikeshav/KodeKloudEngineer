## Problem Statement

The Nautilus development team shared with the DevOps team requirements for new application development, setting up a Git repository for that project. Create a Git repository on Storage server in Stratos DC as per details given below:

Install _git_ package using _yum_on _Storage server_.

After that, create/init a git repository named _/opt/news.git_ (use the exact name as asked and make sure not to create a bare repository).

Certainly! Here's an enhanced version of the solution with clear and precise instructions for creating a Git repository on the Storage server. This version includes improved formatting and details for each step.

## Solution

### 1. Log In to the Storage Server and Switch to Root

First, log in to the Storage server and switch to the root user to ensure you have the necessary permissions:

```bash
ssh natasha@ststor01
sudo su
```

### 2. Install Git on the Storage Server

Update the package index and install the `git` package using `yum`:

```bash
yum update -y
yum install git -y
```

### 3. Verify Git Installation

Check the Git version to confirm the installation:

```bash
git --version
```

**Expected Output:**

```
git version 2.43.5
```

This confirms that Git is installed and provides the version number.

### 4. Initialize the Git Repository

Create and initialize the Git repository at `/opt/news.git`. Note that the repository should not be bare as per the requirements.

**Change to the `/opt` directory and run:**

```bash
cd /opt
git init news.git
```

**Expected Output:**

```
Initialized empty Git repository in /opt/news.git/
```

This command initializes a new Git repository in the specified directory.

### 5. Verify the Repository Initialization

Navigate to the repository directory and list its contents to ensure it has been initialized correctly:

```bash
cd /opt/news.git
ls -la
```

**Expected Output:**

```
total 40
drwxr-xr-x 7 root root 4096 Aug 13 06:22 .
drwxr-xr-x 1 root root 4096 Aug 13 06:22 ..
-rw-r--r-- 1 root root   23 Aug 13 06:22 HEAD
drwxr-xr-x 2 root root 4096 Aug 13 06:22 branches
-rw-r--r-- 1 root root   66 Aug 13 06:22 config
-rw-r--r-- 1 root root   73 Aug 13 06:22 description
drwxr-xr-x 2 root root 4096 Aug 13 06:22 hooks
drwxr-xr-x 2 root root 4096 Aug 13 06:22 info
drwxr-xr-x 4 root root 4096 Aug 13 06:22 objects
drwxr-xr-x 4 root root 4096 Aug 13 06:22 refs
```

This output verifies that the Git repository has been initialized with the necessary directory structure.

This enhanced version of the solution provides a clearer and more detailed guide, making it easier to follow the instructions and verify each step.
