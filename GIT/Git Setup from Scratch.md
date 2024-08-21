## Problem statement

Some new developers have joined xFusionCorp Industries and have been assigned Nautilus project. They are going to start development on a new application, and some pre-requisites have been shared with the DevOps team to proceed with. Please note that all tasks need to be performed on storage server in Stratos DC.

- a. Install git, set up any values for user.email and user.name globally and create a bare repository _/opt/media.git_.

- b. There is an _update_ hook (to block direct pushes to the _master_ branch) under _/tmp_ on storage server itself; use the same to block direct pushes to the _master_ branch in _/opt/media.git_ repo.

- c. Clone _/opt/media.git_ repo in _/usr/src/kodekloudrepos/media_ directory.

- d. Create a new branch named _xfusioncorp_media_ in repo that you cloned under _/usr/src/kodekloudrepos_ directory.

- e. There is a _readme.md_ file in _/tmp_ directory on storage server itself; copy that to the repo, add/commit in the new branch you just created, and finally push your branch to the origin.

- f. Also create _master_ branch from your branch and remember you should not be able to push to the _master_ directly as per the hook you have set up.

## Solution

1. **SSH into the Server:**

   ```bash
   ssh natasha@ststor01
   sudo su
   ```

2. **Install Git:**

   ```bash
   yum install git -y
   ```

3. **Verify Git Installation:**

   ```bash
   git --version
   ```

   Output should confirm Git is installed, e.g., `git version 2.43.5`.

4. **Configure Global Git Settings:**

   ```bash
   git config --global user.email "natasha@ststor01.stratos.xfusioncorp.com"
   git config --global user.name "natasha"
   ```

5. **Create a Bare Git Repository:**

   ```bash
   git init --bare /opt/media.git
   ```

   Output should confirm the repository is created, e.g., `Initialized empty Git repository in /opt/media.git/`.

6. **Set Up the Update Hook:**

   ```bash
   sudo cp /tmp/update /opt/media.git/hooks/
   sudo chmod +x /opt/media.git/hooks/update
   ```

7. **Clone the Bare Repository:**

   ```bash
   git clone /opt/media.git /usr/src/kodekloudrepos/media
   ```

   The warning about the empty repository is expected since it is bare.

8. **Navigate to the Cloned Repository:**

   ```bash
   cd /usr/src/kodekloudrepos/media
   ```

9. **Create and Switch to the New Branch:**

   ```bash
   git checkout -b xfusioncorp_media
   ```

   Note: This command creates the branch and checks it out in one step. The previous approach you used was incorrect because `git branch` only creates a branch but does not switch to it.

10. **Copy the `readme.md` File:**

    ```bash
    cp /tmp/readme.md .
    ```

11. **Add and Commit the Changes:**

    ```bash
    git add readme.md
    git commit -m "added readme.md"
    ```

    Output should confirm the commit, e.g., `[xfusioncorp_media 5fbedf6] added readme.md`.

12. **Push the `xfusioncorp_media` Branch to Origin:**

    ```bash
    git push origin xfusioncorp_media
    ```

    Output should confirm the push, e.g., `[new branch] xfusioncorp_media -> xfusioncorp_media`.

13. **Create the `master` Branch from `xfusioncorp_media`:**

    ```bash
    git checkout -b master
    git push origin master
    ```

    **Note:** Since the `update` hook is in place, you should not be able to push to the `master` branch directly. The push will be rejected by the hook, which is the expected behavior.
