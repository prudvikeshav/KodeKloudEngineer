## Problem Statement

The Nautilus DevOps team want to deploy a PHP website on Kubernetes cluster. They are going to use Apache as a web server and Mysql for database. The team had already gathered the requirements and now they want to make this website live. Below you can find more details:

  1) Create a config map *php-config* for php.ini with variables_order = "EGPCS" data.

  2) Create a deployment named *lamp-wp*.

  3) Create two containers under it. First container must be *httpd-php-container* using image *webdevops/php-apache:alpine-3-php7* and second container must be *mysql-container* from image *mysql:5.6*. Mount php-config configmap in httpd container at */opt/docker/etc/php/php.ini* location.

  4) Create kubernetes generic secrets for *mysql* related values like *myql root password*, *mysql user*,*mysql password*, *mysql host* and *mysql database*. Set any values of your choice.

  5) Add some environment variables for both containers:

     - a) MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD and MYSQL_HOST. Take their values from the secrets you created. Please make sure to use env field (do not use envFrom) to define the name-value pair of environment variables.

  6) Create a node port type service lamp-service to expose the web application, nodePort must be 30008.

  7) Create a service for mysql named *mysql-service* and its port must be *3306*.

  8) We already have */tmp/index.php* file on jump_host server.

     - a) Copy this file into httpd container under Apache document root i.e */app* and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters. Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.

     - b) You must be able to access this *index.php* on node port *30008* at the end, please note that you should see Connected successfully message while accessing this page.

## Solution

To deploy a PHP website using Apache and MySQL on Kubernetes, follow these detailed steps:

### 1. Create the ConfigMap

Create a `ConfigMap` for the `php.ini` file with the specified configuration.

File: `php-config.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: php-config
data:
  php.ini: |
    variables_order = "EGPCS"

```

Apply the `ConfigMap`:

```
kubectl apply -f php-config.yaml
```

### 2. Create Kubernetes Secrets

Create the necessary Kubernetes secrets for MySQL configuration.

```bash
kubectl create secret generic mysql-root-pass --from-literal=password=R00t
kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_pop --from-literal=password=BruCStnMT5
kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db5
kubectl create secret generic mysql-host --from-literal=host=mysql-service
```

### 3. Create the Deployment

Create the deployment with two containers: one for Apache with PHP and one for MySQL.

File: `lamp-wp-deployment.yaml`

`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: lamp-wp
  name: lamp-wp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lamp-wp
  strategy: {}
  template:
    metadata:
      labels:
        app: lamp-wp
    spec:
      containers:
      - image: webdevops/php-apache:alpine-3-php7
        name: nginx-php-container
        ports:
        - containerPort: 80
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: mysql-host
              key: host
        volumeMounts:
        - name: php-ini
          mountPath: /opt/docker/etc/php/php.ini
          subPath: php.ini
      - image: mysql:5.6
        name: mysql-container
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: mysql-host
              key: host
      volumes:
      - name: php-ini
        configMap:
          name: php-config
status: {}
```

### 4. Create the Services

Create a NodePort service to expose the web application and a ClusterIP service for MySQL.

File: `services.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: lamp-service
spec:
  selector:
    app: lamp-wp
  ports:
  -  port: 80
     targetPort: 80
     nodePort: 30008
  type: NodePort
  

---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  selector:
    app: lamp-wp
  ports:
  -  port: 3306
     targetPort: 3306
     protocol: TCP
  type: ClusterIP
```

### 5. Copy the `index.php` File

Copy the `index.php` file to the `httpd-php-container` in the deployment. Replace the dummy MySQL credentials in the file with environment variables.

Command to copy the file:

```
kubectl cp /tmp/index.php lamp-wp-7c9cfc699c-kvfhd:/app
```

Update the index.php file with environment variables:

Replace the dummy values in index.php with:

```
$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];
```

After replacing we can connect to the website.

### 6. Access the Application

Access the application through the NodePort on the IP of any node in your cluster.

Example URL:

```bash
http://<node-ip>:30008
```

You should see the "Connected successfully" message if everything is set up correctly.

### Summary

- Created a ConfigMap for php.ini.
- Created Kubernetes secrets for MySQL configuration.
- Deployed a Deployment with Apache and MySQL containers.
- Created NodePort and ClusterIP services.
- Copied and updated index.php file to use environment variables.
- Ensure that the correct namespaces and resource names are used according to your Kubernetes setup.
