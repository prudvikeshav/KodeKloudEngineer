## Problem Statement

A new developer just joined the Nautilus development team and has been assigned a new project for which he needs to create a new repository under his account on Gitea server. Additionally, there is some existing data that need to be added to the repo. Below you can find more details about the task:

Click on the _Gitea UI_ button on the top bar. You should be able to access the _Gitea UI_. Login to Gitea server using username _max_ and password _Max_pass123_.

- a. Create a new git repository _story_ecommerce_ under _max_ user.

- b. SSH into storage server using user _max_ and password _Max_pass123_ and clone this newly created repository under user _max_ home directory i.e /home/_max_.

- c. Copy all files from location _/usr/dba_ to the repository and commit/push your changes to the _master_ branch. The commit message must be _"add stories"_(must be done in single commit).

- d. Create a new branch _max_demo_ from _master_.

- e. Copy a file _story-index-max.txt_ from location _/tmp/stories/_to the repository. This file has a typo, which you can fix by changing the word Mooose to Mouse. Commit and push the changes to the newly created branch. Commit message must be "typo fixed for Mooose" (must be done in single commit).

Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

Here’s an enhanced and detailed solution to accomplish the given tasks, including more precise instructions and details for each step:

---

## Problem Statement

A new developer needs to create a new repository on the Gitea server and perform several tasks involving repository management and file updates. The tasks are as follows:

1. Create a new Git repository named `story_ecommerce` under the user account `max` on the Gitea server.
2. SSH into the storage server using the credentials provided and clone the newly created repository.
3. Copy files from a specific location to the repository, commit, and push the changes.
4. Create a new branch from `master`, make modifications to a file, and push the changes to the new branch.

## Solution

### 1. Login to Gitea and Create a New Repository

1. **Access Gitea UI:**
   - Open your web browser and click on the _Gitea UI_ button on the top bar or navigate directly to the Gitea server’s URL.
   - Login using the username `max` and password `Max_pass123`.

   **Screenshot**

2. **Create a New Repository:**
   - Once logged in, click on the `+` icon or navigate to the `New Repository` page.
   - Enter the repository name as `story_ecommerce`.
   - Fill in other required fields (e.g., description, visibility) as needed. For a simple setup, you may leave default options.
   - Click the `Create Repository` button.

   **Screenshot**:
   ![forked](https://github.com/prudvikeshav/KodekloudEnginner/blob/main/GIT/images/Repo%20creation.png)

### 2. SSH into Storage Server and Clone the Repository

1. **Login to the Storage Server:**
   - Open a terminal and SSH into the storage server using the provided credentials:

     ```bash
     ssh max@ststor01
     ```

   - Enter the password `Max_pass123` when prompted.

2. **Clone the Repository:**
   - Clone the newly created repository into the home directory of user `max`:

     ```bash
     git clone http://git.stratos.xfusioncorp.com/max/story_ecommerce.git /home/max/story_ecommerce
     ```

   - **Expected Output:**

     ```
     Cloning into 'story_ecommerce'...
     warning: You appear to have cloned an empty repository.
     Checking connectivity... done.
     ```

### 3. Copy Files and Commit Changes to `master` Branch

1. **Copy Files from `/usr/dba` to Repository:**
   - Copy all files from `/usr/dba` into the repository directory:

     ```bash
     cp -r /usr/dba/* /home/max/story_ecommerce/
     ```

2. **Commit and Push Changes:**
   - Navigate to the repository directory:

     ```bash
     cd /home/max/story_ecommerce
     ```

   - Stage and commit the changes:

     ```bash
     git add .
     git commit -m "add stories"
     ```

   - Push the changes to the `master` branch:

     ```bash
     git push origin master
     ```

   **Screenshot**: Capture the terminal showing the `git add`, `git commit`, and `git push` commands, and confirm successful push to the `master` branch.

### 4. Create a New Branch `max_demo` and Modify a File

1. **Create a New Branch from `master`:**
   - Create and switch to the new branch `max_demo`:

     ```bash
     git checkout -b max_demo
     ```

2. **Copy and Modify the File:**
   - Copy the file `story-index-max.txt` from `/tmp/stories` to the repository:

     ```bash
     cp /tmp/stories/story-index-max.txt /home/max/story_ecommerce/
     ```

   - Open and edit the file to fix the typo (change `Mooose` to `Mouse`):

     ```bash
     vi story-index-max.txt
     ```

   - Save and exit the editor (in `vi`, this is done with `:wq`).

3. **Stage, Commit, and Push Changes:**
   - Stage and commit the changes:

     ```bash
     git add story-index-max.txt
     git commit -m "typo fixed for Mooose"
     ```

   - Push the changes to the new branch `max_demo`:

     ```bash
     git push origin max_demo
     ```

   **Screenshot**: Capture the terminal showing the `git checkout -b`, `git add`, `git commit`, and `git push` commands, along with the confirmation of successful push to the `max_demo` branch.

### Summary of Commands

Here’s a summary of the commands used:

```bash
# SSH into the storage server
ssh max@ststor01

# Clone the repository
git clone http://git.stratos.xfusioncorp.com/max/story_ecommerce.git /home/max/story_ecommerce

# Copy files from /usr/dba to the repository
cp -r /usr/dba/* /home/max/story_ecommerce/

# Navigate to the repository directory
cd /home/max/story_ecommerce

# Stage, commit, and push changes to master
git add .
git commit -m "add stories"
git push origin master

# Create and switch to a new branch
git checkout -b max_demo

# Copy and edit the file
cp /tmp/stories/story-index-max.txt /home/max/story_ecommerce/
vi story-index-max.txt

# Stage, commit, and push changes to the new branch
git add story-index-max.txt
git commit -m "typo fixed for Mooose"
git push origin max_demo
```

Ensure to take screenshots or record your actions as required for verification and review purposes.
