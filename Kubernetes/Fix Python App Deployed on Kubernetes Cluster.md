**Problem Ststement**
One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.



- The deployment name is python-deployment-nautilus, its using poroko/flask-demo-appimage. The deployment and service of this app is already deployed.

- nodePort should be 32345 and targetPort should be python flask app's default port.
