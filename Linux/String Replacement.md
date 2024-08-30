# Problem Statement
Within the Stratos DC, the backup server holds template XML files crucial for the Nautilus application. Before utilization, these files require valid data insertion. As part of routine maintenance, system admins at xFusionCorp Industries employ string and file manipulation commands.

Your task is to substitute all occurrences of the string *Random* with *Cloud* within the XML file located at */root/nautilus.xml* on the backup server.

solution:


### Summary of Enhanced Commands

1. **Connect to the server:**

   ```bash
   ssh clint@stbkp01
   ```

2. **Switch to root user:**

   ```bash
   sudo su
   ```

3. **Count occurrences of `Random`:**

   ```bash
   cat /root/nautilus.xml | grep 'Random' | wc -l
   ```
   OutPut:
   ```
   66
   ```

4. **Create a backup of the file:**

   ```bash
   cp /root/nautilus.xml /root/nautilus.xml.bak
   ```

5. **Replace `Random` with `Cloud`:**

   ```bash
   sed -i 's/Random/Cloud/g' /root/nautilus.xml
   ```

6. **Verify there are no remaining `Random` strings:**

   ```bash
   cat /root/nautilus.xml | grep 'Random' | wc -l
   ```
    OutPut:
    ```
    0
    ```
7. **Verify the number of `Cloud` strings:**

   ```bash
   cat /root/nautilus.xml | grep 'Cloud' | wc -l
   ```
    Output:
    ```
    66
    ```
