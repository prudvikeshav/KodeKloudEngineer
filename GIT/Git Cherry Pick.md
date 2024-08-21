## Problem Statement

The Nautilus application development team has been working on a project repository _/opt/ecommerce.git_. This repo is cloned at _/usr/src/kodekloudrepos_ on storage server in Stratos DC. They recently shared the following requirements with the DevOps team:

There are two branches in this repository, _master_ and _feature_. One of the developers is working on the _feature_ branch and their work is still in progress, however they want to merge one of the commits from the _feature_ branch to the _master_ branch, the message for the commit that needs to be merged into _master_ is _Update_ _info.txt_. Accomplish this task for them, also remember to push your changes eventually.

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

   ```
   ls -al
   ```

   output:

   ```
   total 20

  drwxr-xr-x 3 root root 4096 Aug 13 11:56 .
  drwxr-xr-x 3 root root 4096 Aug 13 11:56 ..
  drwxr-xr-x 8 root root 4096 Aug 13 11:56 .git
  -rw-r--r-- 1 root root   35 Aug 13 11:56 info.txt
  -rw-r--r-- 1 root root   17 Aug 13 11:56 welcome.txt

```
3. **Check the Branches:**
   - List all the branches in the repository to confirm you are currently on the `feature` branch and to identify the other available branch (`master`).

   ```bash
   git branch
   ```

  OutPut:
    ```
    * feature
    master
    ```
4. **Check the Commit Logs:**

- Review the commit history of the `feature` branch to locate the commit with the message _Update info.txt_. Note its commit hash as you will need it for cherry-picking.

   ```bash
   git log --oneline
   ```

    Output:

    ```
    a6b5e7f (HEAD -> feature, origin/feature) Update welcome.txt
    69eb7a1 Update info.txt
    9bf623a (origin/master, master) Add welcome.txt
    b2a3170 initial commit
    ```

5. **Checkout the `master` Branch:**
   - Switch to the `master` branch in preparation for applying the commit from the `feature` branch.

   ```bash
   git checkout master
   ```

    Output:

    ```
    Switched to branch 'master'
    warning: cancelling a cherry picking in progress
    Your branch is up to date with 'origin/master'
    ```

6. **Cherry-Pick the Commit:**
   - Use the cherry-pick command to apply the specific commit identified from the `feature` branch to the `master` branch. This operation will integrate the changes from the specified commit into the `master` branch.

   ```bash
   git cherry-pick 69eb7a1
   ```

    Output:

    ```
    [master 19481e2] Update info.txt
    Date: Tue Aug 13 11:26:41 2024 +0000
    1 file changed, 1 insertion(+), 1 deletion(-)
    ```

7. **Push the Changes to the Remote Repository:**
   - Finally, push the updated `master` branch to the remote repository to make the changes available to other collaborators.

   ```bash
   git push origin master
   ```

    Output:

    ```
    Enumerating objects: 5, done.
    Counting objects: 100% (5/5), done.
    Delta compression using up to 36 threads
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 313 bytes | 313.00 KiB/s, done.
    Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
    To /opt/ecommerce.git
       9512a8e..8997b11  master -> master
    ```

The output should confirm that the changes have been successfully pushed to the remote repository.
