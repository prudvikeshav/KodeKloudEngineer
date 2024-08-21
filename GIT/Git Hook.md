## Problem Statement

The Nautilus application development team was working on a git repository _/opt/ecommerce.git_ which is cloned under _/usr/src/kodekloudrepos_ directory present on _Storage server_ in Stratos DC. The team want to setup a hook on this repository, please find below more details:

Merge the _feature_ branch into the _master_ branch`, but before pushing your changes complete below point.

Create a _post-update_ hook in this git repository so that whenever any changes are pushed to the _master_ branch, it creates a release tag with name _release-2023-06-15_, where _2023-06-15_ is supposed to be the current date. For example if today is _20th June, 2023_ then the release tag must be _release-2023-06-20_. Make sure you test the hook at least once and create a release tag for today's release.

Finally remember to push your changes.

## Solution

### 1. SSH into the Server

First, connect to the server where the Git repository is hosted:

```bash
ssh natasha@ststor01
sudo su
```

### 2. Navigate to the Repository Directory

Change to the Git repository’s `hooks` directory:

```bash
cd /opt/ecommerce.git/hooks
```

### 3. Create the `post-update` Hook Script

Create the `post-update` hook file using your preferred text editor. Here we use `vi`, but you can use `nano` or any other text editor:

```bash
vi post-update
```

### 4. Edit the `post-update` Hook Script

Add the following script to the `post-update` hook file. This script will:

- Check if the `master` branch was updated.
- Create a tag with the current date.
- Push the tag to the remote repository.

```bash
#!/bin/bash

# Get the current date in YYYY-MM-DD format
current_date=$(date +%Y-%m-%d)

# Define the tag name
tag_name="release-$current_date"

# Iterate over each ref updated
for ref in "$@"; do
    if [[ "$ref" == *"refs/heads/master"* ]]; then
        echo "Creating tag $tag_name on master branch"
        # Create a new tag for the latest commit on master
        git tag "$tag_name" "$(git rev-parse HEAD)"
        # Push the new tag to the remote repository
        git push origin "$tag_name"
    fi
done
```

This script:

- Retrieves the current date.
- Defines a tag name based on the current date.
- Checks if the update was made to the `master` branch.
- Tags the latest commit on the `master` branch.
- Pushes the tag to the remote repository.

### 5. Make the Script Executable

Ensure the `post-update` script has executable permissions:

```bash
sudo chmod +x post-update
```

### 6. Merge the Feature Branch into Master

Navigate to the cloned repository directory and merge the `feature` branch into the `master` branch:

```bash
cd /usr/src/kodekloudrepos/ecommerce
git checkout master
git pull origin master
git merge feature
```

**Expected Output:**

```
Updating 00092bb..5388672
Fast-forward
feature.txt | 1 +
1 file changed, 1 insertion(+)
create mode 100644 feature.txt
```

### 7. Push the Changes to Master

Push the changes to the remote `master` branch:

```bash
git push origin master
```

**Expected Output:**

```
To /opt/ecommerce.git
00092bb..5388672  master -> master
```

### 8. Verify the Tag Creation

After pushing, check if the new tag has been created and pushed by listing the tags:

```bash
git tag
```

You should see a tag in the format `release-YYYY-MM-DD` corresponding to today’s date.

### 9. Push Local Changes (if applicable)

If you made any local changes (e.g., modifying the `post-update` hook script), commit and push them:

```bash
git add .
git commit -m "Set up post-update hook for release tagging"
git push origin master
```
