## Problem Statement

The Nautilus application development team was working on a git repository _/usr/src/kodekloudrepos/media_ present on Storage server in Stratos DC. One of the developers mistakenly created a couple of files under this repository, but now they want to clean this repository without adding/pushing any new files. Find below more details:

Clean the _/usr/src/kodekloudrepos/media_git repository without adding/pushing any new files, make sure _git status_ is clean.

## Solution

1. **Login to the Storage Server and Gain Root Access:**
   - Connect to the storage server where the repository is located using SSH.
   - Switch to the root user to have full administrative access.

   ```bash
   ssh natasha@ststor01
   sudo su
   ```

2. **Navigate to the Repository Directory:**
   - Change the directory to where the git repository is located.

   ```bash
   cd /usr/src/kodekloudrepos/media
   ```

3. **Check the Status of the Repository:**
   - Verify the current state of the repository to identify untracked files.

   ```bash
   git status
   ```

   **Expected Output:**

   ```
           y6328.rpm
        y646.png
        y6691.int
        y694.org
        y7355.xml
        y7373.dmg
        y7421.yml
        y7770.exe
        y8047.rpm
        y8176.org
        y8400.yaml
        y8548.yaml
        y86.ini
        y9062.png
        y9131.exe
        y9154.temp
        y9175.xml
        y9544.pdf
        y9576.fifo
        y9634.tmp
        y9761.int

    nothing added to commit but untracked files present (use "git add" to track)
   ```

   This output indicates that there are several untracked files in the working directory, and they are not yet part of the repository.

4. **Clean Untracked Files:**
   - Use the `git clean` command to remove all untracked files from the working directory. The `-f` flag is used to force the removal of these files.

   ```bash
   git clean -f
   ```

   **Expected Output:**

   ```
   Removing y7355.xml
   Removing y7373.dmg
   Removing y7421.yml
   Removing y7770.exe
   Removing y8047.rpm
   Removing y8176.org
   Removing y8400.yaml
   Removing y8548.yaml
   Removing y86.ini
   Removing y9062.png
   Removing y9131.exe
   Removing y9154.temp
   Removing y9175.xml
   Removing y9544.pdf
   Removing y9576.fifo
   Removing y9634.tmp
   Removing y9761.int
   ```

   This command removes all untracked files listed in the `git status` output.

5. **Verify the Repository Status Again:**
   - Check the status of the repository once more to ensure that all untracked files have been removed and the working tree is clean.

   ```bash
   git status
   ```

   **Expected Output:**

   ```
   On branch master
   Your branch is up to date with 'origin/master'.

   nothing to commit, working tree clean
   ```

   This output confirms that the repository is now clean, with no untracked files and no changes to commit.
