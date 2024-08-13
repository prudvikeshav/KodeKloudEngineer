## Problem Statement

The Nautilus application development team was working on a git repository _/usr/src/kodekloudrepos/media_ present on Storage server in Stratos DC. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:

Look for the stashed changes under _/usr/src/kodekloudrepos/media_ git repository, and restore the stash with _stash@{1}_ identifier. Further, commit and push your changes to the origin.

## Solution

1. **Login to the Storage Server and Gain Root Access:**
   - Start by connecting to the storage server where the git repository is located using SSH.
   - Once connected, switch to the root user to ensure you have the necessary permissions to modify the repository.

   ```bash
   ssh natasha@ststor01
   sudo su
   ```

2. **Navigate to the Repository Directory:**
   - Change the working directory to the location of the git repository where the stashed changes are stored.

   ```bash
   cd /usr/src/kodekloudrepos/media
   ```

3. **List the Stashed Changes:**
   - Use the `git stash list` command to view all stashed changes. This will help you identify the specific stash you want to restore by its identifier.

   ```bash
   git stash list
   ```

   **Expected Output:**

   ```
   stash@{0}: WIP on master: 0a8d96c initial commit
   stash@{1}: WIP on master: 0a8d96c initial commit
   ```

   This output lists the stashes along with their identifiers and associated commit messages. Locate the stash with the identifier `stash@{1}` that you need to restore.

4. **Apply and Review the Stash:**
   - Before applying the specific stash, you can optionally check and apply the most recent stash to ensure there are no conflicts or leftover changes.

   ```bash
   git stash pop
   ```

   **Expected Output:**

   ```
   On branch master

   Your branch is up to date with 'origin/master'.

   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
        new file:   welcome.txt
   ```

   This command applies the latest stash and shows the changes that have been staged. If you see no changes, proceed to the next step.

5. **Apply the Stash with Identifier `stash@{1}`:**
   - Restore the changes from the stash with the identifier `stash@{1}`. This command reapplies the changes that were saved in that stash to your working directory.

   ```bash
   git stash apply stash@{1}
   ```

   **Expected Output:**

   ```
   On branch master

   Your branch is up to date with 'origin/master'.

   Changes to be committed:
   (use "git restore --staged <file>..." to unstage)
       new file:   welcome.txt
   ```

   This output confirms that the changes from `stash@{1}` have been successfully reapplied to your working directory.

6. **Check the Status of the Repository:**
   - Use `git status` to verify the current state of the repository. This will ensure that the changes from the stash have been applied correctly and are now ready to be committed.

   ```bash
   git status
   ```

   **Expected Output:**

   ```
   On branch master

   Your branch is up to date with 'origin/master'.

   Changes to be committed:
   (use "git restore --staged <file>..." to unstage)
       new file:   welcome.txt
   ```

   This output indicates that the changes from the stash are staged and ready for a commit.

7. **Stage and Commit the Restored Changes:**
   - Add the restored changes to the staging area and create a commit with a descriptive message to indicate that the changes were restored from the stash.

   ```bash
   git add .
   git commit -m "Restored changes from stash@{1}"
   ```

   **Expected Output:**

   ```
   [master 50e8d5b] Restored changes from stash@{1}
   
   1 file changed, 1 insertion(+)
   create mode 100644 welcome.txt
   ```

   This output confirms that the changes have been committed successfully with the specified commit message.

8. **Push the Changes to the Remote Repository:**
   - Finally, push the new commit to the remote repository to update the `master` branch with the restored changes.

   ```bash
   git push origin master
   ```

   **Expected Output:**

   ```
   Enumerating objects: 4, done.

Counting objects: 100% (4/4), done.
Delta compression using up to 36 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 319 bytes | 319.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/media.git
   0a8d96c..50e8d5b  master -> master

   ```

   This output confirms that the changes have been successfully pushed to the remote repository and are now reflected in the `master` branch.
