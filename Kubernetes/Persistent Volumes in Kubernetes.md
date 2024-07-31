# **Problem Statement:**
The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:


- Create a PersistentVolume named as pv-nautilus. Configure the spec as storage class should be manual, set capacity to 4Gi, set access mode to ReadWriteOnce, volume type should be hostPath and set path to /mnt/data (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

- Create a PersistentVolumeClaim named as pvc-nautilus. Configure the spec as storage class should be manual, request 1Gi of the storage, set access mode to ReadWriteOnce.

- Create a pod named as pod-nautilus, mount the persistent volume you created with claim name pvc-nautilus at document root of the web server, the container within the pod should be named as container-nautilus using image nginx with latest tag only (remember to mention the tag i.e nginx:latest).

- Create a node port type service named web-nautilus using node port 30008 to expose the web server running within the pod.