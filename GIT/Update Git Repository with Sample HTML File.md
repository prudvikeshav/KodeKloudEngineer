## Problem Statement

The Nautilus development team has initiated a new project development, establishing various Git repositories to manage each project's source code. Recently, a repository named _/opt/ecommerce.git_ was created. The team has provided a sample _index.html_ file located on the _jump host_ under the _/tmp_ directory. This repository has been cloned to _/usr/src/kodekloudrepos_ on the storage server in the _Stratos DC_.

Copy the sample _index.html_ file from the _jump host_ to the _storage server_ placing it within the cloned repository at _/usr/src/kodekloudrepos/ecommerce_.

Add and commit the file to the repository.

Push the changes to the _master_ branch.

## Solution

1. **Verify the Presence of `index.html` on the Jump Host**

   First, ensure that the `index.html` file exists in the `/tmp` directory of the jump host:

   ```bash
   ls /tmp
   ```

   **Expected Output:**

   ```
   demofile2.json
   index.html
   ```

2. **Copy `index.html` to the Storage Server**

   Use `scp` to securely copy the `index.html` file from the jump host to the `/tmp` directory on the storage server:

   ```bash
   scp /tmp/index.html natasha@ststor01:/tmp
   ```

   **Expected Output:**

   ```
   index.html                                             100%   27    40.5KB/s   00:00
   ```

3. **Log in to the Storage Server**

   SSH into the storage server to perform further actions:

   ```bash
   ssh natasha@ststor01
   ```

4. **Move `index.html` to the Repository Directory**

   Once logged in, move the `index.html` file from the `/tmp` directory to the cloned repository at `/usr/src/kodekloudrepos/ecommerce`:

   ```bash
   mv /tmp/index.html /usr/src/kodekloudrepos/ecommerce
   ```

5. **Verify the File Location**

   Ensure that the `index.html` file has been correctly moved to the repository directory by listing the contents of the directory:

   ```bash
   ls -l /usr/src/kodekloudrepos/ecommerce
   ```

   **Expected Output:**

   ```
   total 24
   drwxr-xr-x 3 root    root    4096 Aug 13 05:23 .
   drwxr-xr-x 3 root    root    4096 Aug 13 05:14 ..
   drwxr-xr-x 8 root    root    4096 Aug 13 05:14 .git
   -rw-r--r-- 1 natasha natasha   27 Aug 13 05:18 index.html
   -rw-r--r-- 1 root    root      34 Aug 13 05:14 info.txt
   -rw-r--r-- 1 root    root      26 Aug 13 05:14 welcome.txt
   ```

6. **Stage and Commit the Changes**

   Use Git to stage and commit the newly added `index.html` file:

   ```bash
   cd /usr/src/kodekloudrepos/ecommerce
   git add index.html
   git commit -m "Added index.html"
   ```

   **Expected Output:**

   ```
   [master 69f4eab] Added index.html
    1 file changed, 1 insertion(+)
    create mode 100644 index.html
   ```

7. **Push the Changes to the `master` Branch**

   Finally, push the commit to the `master` branch of the remote repository:

   ```bash
   git push origin master
   ```

   **Expected Output:**

   ```
   Enumerating objects: 4, done.
   Counting objects: 100% (4/4), done.
   Delta compression using up to 36 threads
   Compressing objects: 100% (2/2), done.
   Writing objects: 100% (3/3), 332 bytes | 332.00 KiB/s, done.
   Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
   To /opt/ecommerce.git
      6f846ce..69f4eab  master -> master
   ```
