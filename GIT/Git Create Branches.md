## Problem Statement

Nautilus developers are actively working on one of the project repositories, _/usr/src/kodekloudrepos/beta_. They need to implement new features in a separate branch to keep those changes isolated. The DevOps team is required to:

1. Create a new branch named _xfusioncorp_beta_ from the _master_ branch in the repository located at _/usr/src/kodekloudrepos/beta_ on the Storage server in Stratos DC.

Please ensure no code changes are made.

## Solution

1. **Login to the Storage Server and Gain Root Access:**
   - Connect to the Storage server using SSH.
   - Switch to the root user.

   ```bash
   ssh natasha@ststor01
   sudo su
   ```

2. **Navigate to the Repository Directory:**
   - Change to the directory where the repository is located.

   ```bash
   cd /usr/src/kodekloudrepos/beta
   ```

3. **Verify the Current Branches:**
   - List the branches to confirm you are in the correct repository and to ensure the _master_ branch is available.

   ```bash
   git branch
   ```

   **Expected Output:**

   ```plaintext
   * kodekloud_beta
     master
   ```

4. **Ensure You Are on the Master Branch:**
   - Switch to the _master_ branch if not already on it. This step ensures that the new branch is created from the correct base branch.

   ```bash
   git checkout master
   ```

5. **Create and Switch to the New Branch:**
   - Create a new branch named _xfusioncorp_beta_ from the _master_ branch.
   - Optionally, switch to the new branch to start working on it immediately.

   ```bash
   git branch xfusioncorp_beta
   git checkout xfusioncorp_beta
   ```

   **Alternatively, you can combine the above two commands into a single step:**

   ```bash
   git checkout -b xfusioncorp_beta
   ```

6. **Verify the New Branch Creation:**
   - Confirm that the new branch has been created and is currently checked out.

   ```bash
   git branch
   ```

   **Expected Output:**

   ```plaintext
     kodekloud_beta
     master
   * xfusioncorp_beta
   ```
