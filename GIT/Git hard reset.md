## Problem Statement

The Nautilus application development team was working on a git repository _/usr/src/kodekloudrepos/official_ present on _Storage server_ in Stratos DC. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the _HEAD_ and the branch itself to a commit with message add _data.txt_ file. Find below more details:

In _/usr/src/kodekloudrepos/official git_ repository, reset the git commit history so that there are only two commits in the commit history i.e _initial commit_ and add _data.txt_ file.

Also make sure to push your changes.

Here's an enhanced description for the solution part of the problem, providing more detail and clarity on each step:

## Solution

1. **Login to the Storage Server and Gain Root Access:**
   - Begin by establishing an SSH connection to the storage server where the repository is located.
   - Once logged in, switch to the root user to ensure you have the necessary permissions to modify the repository.

   ```bash
   ssh natasha@ststor01
   sudo su
   ```

2. **Navigate to the Repository Directory:**
   - Change your working directory to the location of the git repository on the storage server.

   ```bash
   cd /usr/src/kodekloudrepos/official/
   ```

3. **Verify the Current Commit History:**
   - Check the commit history to identify the commit hashes and messages. This will help in verifying the correct commit to which we want to reset.

   ```bash
   git log --oneline
   ```

   **Expected Output:**

   ```
   51e564b (HEAD -> master, origin/master) Test Commit10
   de4c979 Test Commit9
   08a77b8 Test Commit8
   cf35dc9 Test Commit7
   855af7c Test Commit6
   3a1259e Test Commit5
   6d44162 Test Commit4
   42e70de Test Commit3
   9f73b38 Test Commit2
   25ced5c Test Commit1
   c1eb942 add data.txt file
   432c9fb initial commit
   ```

   In this output, you can see the commits in reverse chronological order, with the most recent commit at the top.

4. **Perform a Hard Reset to the Desired Commit:**
   - To clean up the commit history and retain only the desired commits, perform a hard reset to the commit with the message _"add data.txt file"_. This action will reset the `HEAD` of the branch and discard all commits that come after the specified commit.

   ```bash
   git reset --hard c1eb942
   ```

   **Expected Output:**

   ```
   HEAD is now at c1eb942 add data.txt file
   ```

   This output indicates that the `HEAD` of the `master` branch has been moved to the commit with hash `c1eb942`, effectively removing all commits made after this commit.

5. **Verify the Updated Commit History:**
   - After performing the hard reset, check the commit history again to confirm that it now only includes the two desired commits: the initial commit and the commit with the message _"add data.txt file"_.

   ```bash
   git log --oneline
   ```

   **Expected Output:**

   ```
   c1eb942 (HEAD -> master) add data.txt file
   432c9fb initial commit
   ```

   This output confirms that the commit history now consists of only the two specified commits.

6. **Push the Changes to the Remote Repository:**
   - To update the remote repository and reflect the changes made by the hard reset, you need to force push the updated `master` branch. This will overwrite the remote branch with your local branch's state.

   ```bash
   git push origin master --force
   ```

   **Expected Output:**

   ```
   Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
   To /opt/official.git
   - 51e564b...c1eb942 master -> master (forced update)
   ```

   This output confirms that the remote `master` branch has been forcefully updated to match the state of your local `master` branch, with the commit history reflecting only the desired commits.
