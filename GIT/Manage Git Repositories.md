## Problem Statement

A new developer just joined the Nautilus development team and has been assigned a new project for which he needs to create a new repository under his account on Gitea server. Additionally, there is some existing data that need to be added to the repo. Below you can find more details about the task:

Click on the _Gitea UI_ button on the top bar. You should be able to access the _Gitea UI_. Login to Gitea server using username _max_ and password _Max_pass123_.

- a. Create a new git repository _story_beta_ under _max_ user.

- b. SSH into storage server using user _max_ and password _Max_pass123_ and clone this newly created repository under user _max_ home directory i.e /home/_max_.

- c. Copy all files from location _/usr/dba_ to the repository and commit/push your changes to the _master_ branch. The commit message must be _"add stories"_(must be done in single commit).

- d. Create a new branch _max_apps_ from _master_.

- e. Copy a file _story-index-max.txt_ from location _/tmp/stories/_to the repository. This file has a typo, which you can fix by changing the word Mooose to Mouse. Commit and push the changes to the newly created branch. Commit message must be "typo fixed for Mooose" (must be done in single commit).

Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Solution

### 1. Login to Gitea and Create a New Repository

1. **Access Gitea UI:**
   - Open your web browser and navigate to the Gitea server’s URL or click the _Gitea UI_ button.
   - Log in using the username `max` and password `Max_pass123`.

   **Screenshot**:

   ![Gitea Login](https://github.com/prudvikeshav/KodekloudEnginner/blob/main/GIT/images/max%20login.png)

2. **Create a New Repository:**
   - After logging in, find and click the `+` icon or navigate to the `New Repository` page.
   - Enter `story_beta` as the repository name.
   - Fill in other optional fields as needed, such as description or visibility.
   - Click `Create Repository`.

   **Screenshot**:
   ![Create Repository](https://github.com/prudvikeshav/KodekloudEnginner/blob/main/GIT/images/Repo%20creation.png)

### 2. SSH into Storage Server and Clone the Repository

1. **SSH into the Storage Server:**
   - Open a terminal and connect to the storage server:

     ```bash
     ssh max@ststor01
     ```

   - Enter the password `Max_pass123`.

2. **Clone the Repository:**
   - Clone the newly created repository into the home directory:

     ```bash
     git clone http://git.stratos.xfusioncorp.com/max/story_beta.git /home/max/story_beta
     ```

   - **Expected Output:**

     ```
     Cloning into 'story_beta'...
     warning: You appear to have cloned an empty repository.
     Checking connectivity... done.
     ```

### 3. Copy Files and Commit Changes to `master` Branch

1. **Copy Files from `/usr/dba` to Repository:**
   - Copy all files from `/usr/dba` to the repository directory:

     ```bash
     cp -r /usr/dba/* /home/max/story_beta/
     ```

2. **Navigate to the Repository Directory:**
   - Change to the repository directory:

     ```bash
     cd /home/max/story_beta
     ```

3. **Stage and Commit Changes:**
   - Add all files to the staging area:

     ```bash
     git add .
     ```

   - Commit the changes with the specified message:

     ```bash
     git commit -m "add stories"
     ```

   - **Expected Output:**

     ```
     2 files changed, 42 insertions(+)
     create mode 100644 frogs-and-ox.txt
     create mode 100644 lion-and-mouse.txt
     ```

4. **Push Changes to Remote Repository:**
   - Push the changes to the `master` branch:

     ```bash
     git push origin master
     ```

   - **Expected Output:**

     ```
     Counting objects: 4, done.
     Delta compression using up to 36 threads.
     Compressing objects: 100% (4/4), done.
     Writing objects: 100% (4/4), 1.19 KiB | 0 bytes/s, done.
     Total 4 (delta 0), reused 0 (delta 0)
     remote: . Processing 1 references
     remote: Processed 1 references in total
     To <http://git.stratos.xfusioncorp.com/max/story_beta.git>
     - [new branch]      master -> master
     ```

   **Screenshot**:
   ![Add Files and Commit](https://github.com/prudvikeshav/KodekloudEnginner/blob/main/GIT/images/git%20add.png)

### 4. Create a New Branch `max_apps` and Modify a File

1. **Create a New Branch:**
   - Create and switch to a new branch named `max_apps`:

     ```bash
     git checkout -b max_apps
     ```

2. **Copy and Modify the File:**
   - Copy `story-index-max.txt` from `/tmp/stories` to the repository:

     ```bash
     cp /tmp/stories/story-index-max.txt /home/max/story_beta/
     ```

   - Open and edit `story-index-max.txt` to fix the typo (`Mooose` to `Mouse`):

     ```bash
     vi story-index-max.txt
     ```

   - Save the file and exit the editor (in `vi`, use `:wq`).

3. **Stage, Commit, and Push Changes:**
   - Stage the modified file:

     ```bash
     git add story-index-max.txt
     ```

   - Commit the change with the specified message:

     ```bash
     git commit -m "typo fixed for Mooose"
     ```

   - **Expected Output:**

     ```
     1 file changed, 4 insertions(+)
     create mode 100644 story-index-max.txt
     ```

   - Push the changes to the `max_apps` branch:

     ```bash
     git push origin max_apps
     ```

   - **Expected Output:**

     ```
     Counting objects: 3, done.
     Delta compression using up to 36 threads.
     Compressing objects: 100% (3/3), done.
     Writing objects: 100% (3/3), 413 bytes | 0 bytes/s, done.
     Total 3 (delta 0), reused 0 (delta 0)
     remote:
     remote: Create a new pull request for 'max_apps':
     remote:   <http://git.stratos.xfusioncorp.com/max/story_beta/compare/master...max_apps>
     remote:
     remote: . Processing 1 references
     remote: Processed 1 references in total
     To <http://git.stratos.xfusioncorp.com/max/story_beta.git>
     - [new branch]      max_apps -> max_apps
     ```

   **Screenshot**:

   ![Final Commit and Push](<https://github.com/prudvikeshav/KodekloudEnginner/blob/main/GIT/images/Final.png>)
