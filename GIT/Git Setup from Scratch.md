## Problem statement

Some new developers have joined xFusionCorp Industries and have been assigned Nautilus project. They are going to start development on a new application, and some pre-requisites have been shared with the DevOps team to proceed with. Please note that all tasks need to be performed on storage server in Stratos DC.

- a. Install git, set up any values for user.email and user.name globally and create a bare repository _/opt/media.git_.

- b. There is an _update_ hook (to block direct pushes to the _master_ branch) under _/tmp_ on storage server itself; use the same to block direct pushes to the _master_ branch in _/opt/media.git_ repo.

- c. Clone _/opt/media.git_ repo in _/usr/src/kodekloudrepos/media_ directory.

- d. Create a new branch named _xfusioncorp_media_ in repo that you cloned under _/usr/src/kodekloudrepos_ directory.

- e. There is a _readme.md_ file in _/tmp_ directory on storage server itself; copy that to the repo, add/commit in the new branch you just created, and finally push your branch to the origin.

- f. Also create _master_ branch from your branch and remember you should not be able to push to the _master_ directly as per the hook you have set up.
