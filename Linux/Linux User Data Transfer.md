# Problem Statement

---
Due to an accidental data mix-up, user data was unintentionally mingled on Nautilus App Server 2 at the */home/usersdata* location by the Nautilus production support team in Stratos DC. To rectify this, specific user data needs to be filtered and relocated. Here are the details:

- Locate all files (excluding directories) owned by user *mark* within the */home/usersdata* directory on *App Server 2*. Copy these files while preserving the directory structure to the */beta* directory

---

## Solution

---

1. **SSH into the server and switch to the root user:**

```bash
ssh steve@stapp02
sudo su
```

2.**Locate all files owned by user *mark*  within the */home/usersdata* ,  Copy */beta* directory.**
To copy files owned by the user *mark* from */home/usersdata* to */beta* while preserving the directory structure using only the find command, you can use the following approach.

```bash
find /home/usersdata/ -type f  -user mark -exec cp {} /beta \;
```

3.**Verify if the files got copied:**

```plain
ls /beta/ | wc -l
1409
```
