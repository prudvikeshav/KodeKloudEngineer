## Problem Statement

The xFusionCorp development team added updates to the project that is maintained under _/opt/demo.git_ repo and cloned under _/usr/src/kodekloudrepos/demo_. Recently some changes were made on Git server that is hosted on _Storage server_ in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on _/usr/src/kodekloudrepos/demo_ repository as per details mentioned below:

- a. In _/usr/src/kodekloudrepos/demo_ repo add a new remote dev_demo and point it to _/opt/xfusioncorp_demo_.git repository.

- b. There is a file _/tmp/index.html_ on same server; copy this file to the repo and add/commit to _master_ branch.

- c. Finally push _master_ branch to this new remote origin.

## Solution

1. **Login to the Storage Server and Switch to Root User:**
   - Connect to the storage server and elevate to the root user.

   ```bash
   ssh natasha@ststor01
   sudo su
   ```

2. **Navigate to the Repository Directory:**
   - Change to the directory where the Git repository is located.

   ```bash
   cd /usr/src/kodekloudrepos/demo/
   ```

3. **Check the Existing Git Remotes:**
   - List the current remotes to verify existing configurations.

   ```bash
   git remote -v
   ```

   **Expected Output:**

   ```plaintext
   origin  /opt/demo.git (fetch)
   origin  /opt/demo.git (push)
   ```

4. **Add a New Remote Named _dev_demo_:**
   - Add a new remote named _dev_demo_ that points to the _/opt/xfusioncorp_demo.git_ repository.

   ```bash
   git remote add dev_demo /opt/xfusioncorp_demo.git
   ```

5. **Verify the New Remote Configuration:**
   - List the remotes again to ensure that the new remote has been added correctly.

   ```bash
   git remote -v
   ```

   **Expected Output:**

   ```plaintext
   origin  /opt/demo.git (fetch)
   origin  /opt/demo.git (push)
   dev_demo  /opt/xfusioncorp_demo.git (fetch)
   dev_demo  /opt/xfusioncorp_demo.git (push)
   ```

6. **Copy the File _index.html_ into the Repository:**
   - Copy the _index.html_ file from _/tmp_ to the repository directory.

   ```bash
   cp /tmp/index.html /usr/src/kodekloudrepos/demo/
   ```

   - Verify that the file has been successfully copied.

   ```bash
   ls -al index.html
   ```

   **Expected Output:**

   ```plaintext
   -rw-r--r-- 1 root root 1234 Aug 13 08:01 index.html
   ```

7. **Add and Commit the Changes:**
   - Stage the new file for commit and commit the changes to the _master_ branch.

   ```bash
   git add index.html
   git commit -m "Added index.html to master branch"
   ```

   **Expected Output:**

   ```plaintext
   [master 64ad743] Added index.html to master branch
    1 file changed, 10 insertions(+)
    create mode 100644 index.html
   ```

8. **Push the _master_ Branch to the New Remote:**
   - Push the changes in the _master_ branch to the new remote _dev_demo_.

   ```bash
   git push dev_demo master
   ```

   **Expected Output:**

   ```plaintext
   Enumerating objects: 6, done.
   Counting objects: 100% (6/6), done.
   Delta compression using up to 36 threads
   Compressing objects: 100% (4/4), done.
   Writing objects: 100% (6/6), 584 bytes | 584.00 KiB/s, done.
   Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
   To /opt/xfusioncorp_demo.git
    * [new branch]      master -> master
   ```
