## Problem Statement

Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details:

SSH into _storage server_ using user _max_ and password _Max_pass123_. Under _/home/max_ you will find the story-blog repository. Try to push the changes to the origin repo and fix the issues. The _story-index.txt_ must have titles for all 4 stories. Additionally, there is a typo in The _Lion and the Mooose_ line where _Mooose_ should be _Mouse_.

Click on the _Gitea_ UI button on the top bar. You should be able to access the _Gitea_ page. You can login to _Gitea_ server from UI using username _sarah_ and password _Sarah_pass123_ or username _max_ and password _Max_pass123_.

Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Solution

### 1. SSH into the Storage Server

1. **Connect to the Storage Server:**
   - Open a terminal and SSH into the storage server using the provided credentials:

     ```bash
     ssh max@ststor01
     ```

   - Enter the password `Max_pass123` when prompted.

2. **Navigate to the Repository Directory:**
   - Change to the directory where the repository `story-blog` is located:

     ```bash
     cd /home/max/story-blog
     ```

### 2. Check Repository Status and Identify Issues

1. **Check Repository Status:**
   - Verify the status of your branch and check if there are any issues:

     ```bash
     git status
     ```

   - **Output Example:**

     ```
     On branch master
     Your branch is ahead of 'origin/master' by 1 commit.
       (use "git push" to publish your local commits)
     nothing to commit, working directory clean
     ```

### 3. Update `story-index.txt` to Include Titles and Fix Typo

1. **Edit `story-index.txt`:**
   - Open the `story-index.txt` file to add titles for all four stories and correct the typo:

     ```bash
     vi story-index.txt
     ```

   - Make sure each story has a title and correct the typo (`Mooose` to `Mouse`).

2. **Save and Exit:**
   - Save the changes and exit the editor (`:wq` in `vi`).

### 4. Commit and Push Changes

1. **Stage and Commit Changes:**
   - Stage the modified file:

     ```bash
     git add story-index.txt
     ```

   - Commit the changes with a descriptive message:

     ```bash
     git commit -m "Added titles for all stories and fixed typo in 'The Lion and the Mooose'"
     ```

2. **Push Changes to the Remote Repository:**
   - Attempt to push the changes to the remote repository:

     ```bash
     git push origin master
     ```

   - **If you encounter an error:**
     - You may see an error indicating that the push was rejected because the remote contains work you do not have locally. This requires pulling changes from the remote repository and resolving any conflicts.

### 5. Resolve Merge Conflicts

1. **Pull Changes from Remote Repository:**
   - Pull the latest changes from the remote repository to update your local branch:

     ```bash
     git pull origin master
     ```

   - **Expected Output:**

     ```
     remote: Enumerating objects: 4, done.
     remote: Counting objects: 100% (4/4), done.
     remote: Compressing objects: 100% (3/3), done.
     remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
     Unpacking objects: 100% (3/3), done.
     From http://git.stratos.xfusioncorp.com/sarah/story-blog
      * branch            master     -> FETCH_HEAD
        0ffc765..f953859  master     -> origin/master
     Auto-merging story-index.txt
     CONFLICT (add/add): Merge conflict in story-index.txt
     Automatic merge failed; fix conflicts and then commit the result.
     ```

2. **Resolve Merge Conflicts:**
   - Open `story-index.txt` to resolve conflicts:

     ```bash
     vi story-index.txt
     ```

   - Resolve conflicts by keeping the correct lines and removing unwanted ones. After resolving conflicts, save and exit.

3. **Stage and Commit Resolved Changes:**
   - Stage the resolved file:

     ```bash
     git add story-index.txt
     ```

   - Commit the resolved merge:

     ```bash
     git commit -m "Resolved merge conflicts"
     ```

4. **Push Resolved Changes to Remote Repository:**
   - Push the changes to the remote repository:

     ```bash
     git push origin master
     ```

   - **Expected Output:**

     ```
     Counting objects: 10, done.
     Delta compression using up to 36 threads.
     Compressing objects: 100% (10/10), done.
     Writing objects: 100% (10/10), 1.43 KiB | 0 bytes/s, done.
     Total 10 (delta 3), reused 0 (delta 0)
     remote: Processing 1 references
     remote: Processed 1 references in total
     To http://git.stratos.xfusioncorp.com/sarah/story-blog.git
      f953859..993d40d  master -> master
     ```

### 6. Verify Changes Using Gitea UI

1. **Log in to Gitea UI:**
   - Open your web browser and access the Gitea UI by clicking the Gitea button or navigating to the Gitea server URL.
   - Log in using username `sarah` or `max` with the respective passwords.

   ![login](https://github.com/prudvikeshav/KodekloudEnginner/blob/main/GIT/images/max%20login.png)

2. **Verify the Repository:**
   - Navigate to the `story-blog` repository and confirm that `story-index.txt` has been updated correctly with all titles and the typo has been fixed.

   **Screenshot**:

   ![final](https://github.com/prudvikeshav/KodekloudEnginner/blob/main/GIT/images/Final%20repo.png)
