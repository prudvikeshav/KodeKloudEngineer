## Problem Statement

The Nautilus application development team has been working on a project repository _/opt/demo.git_. This repo is cloned at _/usr/src/kodekloudrepos_on storage server in Stratos DC. They recently shared the following requirements with DevOps team:

- Create a new branch _xfusion_ in _/usr/src/kodekloudrepos/demo_ repo from master and copy the _/tmp/index.html_ file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into_master_ branch. Finally, push the changes to the origin for both of the branches.

## Solution

1. **Login to the Storage Server and Gain Root Access:**
   - Connect to the storage server via SSH.
   - Switch to the root user.

   ```bash
   ssh natasha@ststor01
   sudo su
   ```

2. **Navigate to the Repository Directory:**
   - Change to the directory containing the repository.

   ```bash
   cd /usr/src/kodekloudrepos/demo
   ```

3. **Verify the Repository Contents:**
   - List the files to confirm the repository's state.

   ```bash
   ls -al
   ```

   **Expected Output:**

   ```plaintext
   total 20
   drwxr-xr-x 3 root root 4096 Aug 13 08:02 .
   drwxr-xr-x 3 root root 4096 Aug 13 08:02 ..
   drwxr-xr-x 8 root root 4096 Aug 13 08:02 .git
   -rw-r--r-- 1 root root   34 Aug 13 08:02 info.txt
   -rw-r--r-- 1 root root   26 Aug 13 08:02 welcome.txt
   ```

4. **Check the Current Branch:**
   - Verify the active branch to ensure it’s _master_.

   ```bash
   git branch
   ```

   **Expected Output:**

   ```plaintext
   * master
   ```

5. **Create and Switch to the New Branch:**
   - Create a new branch named _xfusion_ from _master_.
   - Switch to the new branch.

   ```bash
   git checkout -b xfusion
   ```

   **Expected Output:**

   ```plaintext
   Switched to a new branch 'xfusion'
   ```

6. **Move the File from /tmp to the Repository:**
   - Move the _index.html_ file into the repository directory.

   ```bash
   mv /tmp/index.html .
   ```

   **Verify the File Move:**

   ```bash
   ls -al
   ```

   **Expected Output:**

   ```plaintext
   total 24
   drwxr-xr-x 3 root root 4096 Aug 13 08:12 .
   drwxr-xr-x 3 root root 4096 Aug 13 08:02 ..
   drwxr-xr-x 8 root root 4096 Aug 13 08:10 .git
   -rw-r--r-- 1 root root   27 Aug 13 08:01 index.html
   -rw-r--r-- 1 root root   34 Aug 13 08:02 info.txt
   -rw-r--r-- 1 root root   26 Aug 13 08:02 welcome.txt
   ```

7. **Add and Commit the Changes:**
   - Stage the new file and commit the changes with an appropriate message.

   ```bash
   git add index.html
   git commit -m "Add index.html to xfusion branch"
   ```

   **Expected Output:**

   ```plaintext
   [xfusion ecaecc9] Add index.html to xfusion branch
    1 file changed, 1 insertion(+)
    create mode 100644 index.html
   ```

8. **Push the New Branch to the Remote Repository:**
   - Push the _xfusion_ branch to the remote repository.

   ```bash
   git push origin xfusion
   ```

   **Expected Output:**

   ```plaintext
   Enumerating objects: 4, done.
   Counting objects: 100% (4/4), done.
   Delta compression using up to 36 threads
   Compressing objects: 100% (2/2), done.
   Writing objects: 100% (3/3), 331 bytes | 331.00 KiB/s, done.
   Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
   To /opt/demo.git
    * [new branch]      xfusion -> xfusion
   ```

9. **Switch Back to the Master Branch:**
   - Check out the _master_ branch.

   ```bash
   git checkout master
   ```

   **Expected Output:**

   ```plaintext
   Switched to branch 'master'
   Your branch is up to date with 'origin/master'
   ```

10. **Merge the _xfusion_ Branch into _master_:**
    - Merge the changes from _xfusion_ into _master_.

    ```bash
    git merge xfusion
    ```

    **Expected Output:**

    ```plaintext
    Updating f9b6eb2..ecaecc9
    Fast-forward
     index.html | 1 +
     1 file changed, 1 insertion(+)
     create mode 100644 index.html
    ```

11. **Push the Updated Master Branch to the Remote Repository:**
    - Push the changes to the _master_ branch on the remote.

    ```bash
    git push origin master
    ```

    **Expected Output:**

    ```plaintext
    Enumerating objects: 4, done.
    Counting objects: 100% (4/4), done.
    Delta compression using up to 36 threads
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 331 bytes | 331.00 KiB/s, done.
    Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
    To /opt/demo.git
     * [new branch]      xfusion -> xfusion
     * [updated]         master -> master
    ```
