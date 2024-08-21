## Problem Statement

The Nautilus application development team was working on a git repository _/usr/src/kodekloudrepos/games_ present on _Storage server_ in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

In _/usr/src/kodekloudrepos/games_ git repository, revert the latest commit ( HEAD ) to the previous commit (JFYI the previous commit hash should be with _initial commit_ message ).

Use _revert games_ message (please use all small letters for commit message) for the new revert commit.

## Solution

### 1. **Login to the Storage Server and Gain Root Access:**

- Connect to the storage server via SSH.
- Switch to the root user to perform administrative tasks.

   ```bash
   ssh natasha@ststor01
   sudo su
   ```

### 2. **Navigate to the Repository Directory:**

- Change to the directory containing the repository where the changes are to be made.

   ```bash
   cd /usr/src/kodekloudrepos/games
   ```

### 3. **Check the Status of the Repository:**

- Verify the status of the repository to ensure you are on the correct branch and there are no uncommitted changes.

   ```bash
   git status
   ```

   **Expected Output:**

   ```plaintext
   On branch master
   Your branch is up to date with 'origin/master'.

   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           games.txt

   nothing added to commit but untracked files present (use "git add" to track)
   ```

### 4. **View the Commit History:**

- Check the commit history to identify the latest commit and the previous commit with the _initial commit_ message.

   ```bash
   git log --oneline
   ```

   **Expected Output:**

   ```plaintext
   b6d153a (HEAD -> master, origin/master) add data.txt file
   7c10967 initial commit
   ```

- `b6d153a` is the latest commit you want to revert.
- `7c10967` is the previous commit with the _initial commit_ message.

### 5. **Revert the Latest Commit:**

- Use the `git revert` command to create a new commit that undoes the changes introduced by the latest commit. This will open your default text editor to edit the commit message.

   ```bash
   git revert HEAD
   ```

   **Editor Instructions:**

- In the text editor that opens, change the default commit message to `revert games`. Make sure the message is in all lowercase letters as specified.
- Save the changes and close the editor. This creates a new commit that reverts the changes made by the latest commit.

   **Expected Output:**

   ```plaintext
   [master 14011bb] revert games
    1 file changed, 1 insertion(+)
    create mode 100644 index.html
   ```

   **Note:** If you do not see the commit message `revert games` and instead see the default message, manually edit the commit message in the editor.

### 6. **Push the Revert Commit to the Remote Repository:**

- Push the new revert commit to the remote repository to update it.

   ```bash
   git push origin master
   ```

   **Expected Output:**

   ```plaintext
   Enumerating objects: 4, done.
   Counting objects: 100% (4/4), done.
   Delta compression using up to 8 threads
   Compressing objects: 100% (2/2), done.
   Writing objects: 100% (2/2), 294 bytes | 294.00 KiB/s, done.
   Total 2 (delta 0), reused 0 (delta 0), pack-reused 0
   To /usr/src/kodekloudrepos/games
    * [new commit]      revert games -> master
   ```
