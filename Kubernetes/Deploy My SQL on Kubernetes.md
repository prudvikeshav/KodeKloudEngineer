# **Problem Statement:**

A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:



- Create a PersistentVolume mysql-pv, its capacity should be 250Mi, set other parameters as per your preference.


- Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as mysql-pv-claim and request a 250Mi of storage. Set other parameters as per your preference.


- Create a deployment named mysql-deployment, use any mysql image as per your preference. Mount the PersistentVolume at mount path /var/lib/mysql.


- Create a NodePort type service named mysql and set nodePort to 30007.


- Create a secret named mysql-root-pass having a key pair value, where key is password and its value is YUIidhb667, create another secret named mysql-user-pass having some key pair values, where frist key is username and its value is kodekloud_joy, second key is password and value is dCV3szSGNA, create one more secret named mysql-db-url, key name is database and value is kodekloud_db2


- Define some Environment variables within the container:


    - name: MYSQL_ROOT_PASSWORD, should pick value from secretKeyRef name: mysql-root-pass and key: password


    - name: MYSQL_DATABASE, should pick value from secretKeyRef name: mysql-db-url and key: database


    - name: MYSQL_USER, should pick value from secretKeyRef name: mysql-user-pass key key: username


    - name: MYSQL_PASSWORD, should pick value from secretKeyRef name: mysql-user-pass and key: password