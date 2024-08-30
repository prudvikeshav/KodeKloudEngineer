# Problem Statement

Within the Stratos DC, the Nautilus storage server hosts a directory named */data*, serving as a repository for various developers non-confidential data. Developer *anita* has requested a copy of their data stored in */data/anita*. The System Admin team has provided the following steps to fulfill this request:

a. Create a compressed archive named *anita.tar.gz* of the */data/anita* directory.

b. Transfer the archive to the */home* directory on the Storage Server.

## Solution

1. **SSH into the server**:

   ```bash
   ssh natasha@ststor01
   ```

2. **Switch to root** (if needed):

   ```bash
   sudo su
   ```

3. **Change to the directory**:

   ```bash
   cd /data/anita
   ```

4. **Create the archive**:

   ```bash
   tar -czvf /home/anita.tar.gz  /data/anita/
   ```

5. **Verify the archive**:

   ```bash
   ls -l /home/anita.tar.gz
   ```

6. **Exit**:

   ```bash
   exit
   exit
   ```
