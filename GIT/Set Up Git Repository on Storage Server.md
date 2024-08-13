## Problem Statement

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the _Storage server_ in the Stratos DC:

Utilize _yum_ to install the _git_ package on the _Storage Server_.

Create a bare repository named _/opt/ecommerce.git_(ensure exact name usage).

## Solution

### 1. Login into the Storage Server

**Command:**

```bash
ssh natasha@ststor01
```

### 2. Install the Git Package

**Command:**

```bash
yum install git -y
```

### 3. Verify Git Installation

**Command:**

```bash
git
```

**Expected Output:**

```
git
usage: git [-v | --version] [-h | --help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [--config-env=<name>=<envvar>] <command> [<args>]

These are common Git commands used in various situations:
```

**Description:**
Running the `git` command without any arguments displays the general usage instructions and available commands for Git. This output confirms that Git is installed correctly and provides a brief overview of how to use Git commands.

### 4. Create the Directory for the Repository

**Command:**

```bash
mkdir /opt/ecommerce.git
```

**Description:**
This command creates a new directory named `ecommerce.git` in the `/opt` directory. The `mkdir` command stands for "make directory." This directory will serve as the location for the Git repository. Ensure that the directory path matches the required naming convention exactly.

### 5. Initialize a Bare Repository

**Command:**

```bash
git init --bare
```

**Expected Output:**

```
Initialized empty Git repository in /opt/ecommerce.git/
```

**Description:**
The `git init --bare` command initializes a new, empty Git repository in the `/opt/ecommerce.git` directory. A bare repository is one that does not have a working directory; it is used solely for storing the version control information. This is typically used for remote repositories where developers will push their changes but not work directly on the files.
