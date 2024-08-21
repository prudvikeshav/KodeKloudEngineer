## Problem Statement

The Nautilus DevOps team want to deploy a static website on Kubernetes cluster. They are going to use Nginx, phpfpm and MySQL for the database. The team had already gathered the requirements and now they want to make this website live. Below you can find more details:

- Create some secrets for MySQL.

- Create a secret named *mysql-root-pass* wih key/value pairs as below:

    ```
     name: password
     value: R00t
    ```

- Create a secret named *mysql-user-pass* with key/value pairs as below:

    ```
     name: username
     value: kodekloud_cap
    ```

    ```
     name: password
     value: ksH85UJjhb
    ```

- Create a secret named *mysql-db-url* with key/value pairs as below:

    ```
     name: database
     value: kodekloud_db8
    ```

- Create a secret named *mysql-host* with key/value pairs as below:

    ```
     name: host
     value: mysql-service
    ```

- Create a config map *php-config* for php.ini with variables_order = *"EGPCS"* data.
- Create a deployment named *lemp-wp*.
- Create two containers under it. First container must be *nginx-php-container* using image *webdevops/php-nginx:alpine-3-php7* and second container must be *mysql-container* from image *mysql:5.6*. Mount *php-config* configmap in nginx container at */opt/docker/etc/php/php.ini* location.

- Add some environment variables for both containers:

  - MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD and MYSQL_HOST. Take their values from the secrets you created. Please make sure to use env field (do not use envFrom) to define the name-value pair of environment variables.

- Create a node port type service *lemp-service* to expose the web application, nodePort must be *30008*.

- Create a service for mysql named *mysql-service* and its port must be *3306*.

- We already have a */tmp/index.php* file on jump_host server.

  - Copy this file into the nginx container under document root i.e */app*and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters. Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.

  - Once done, you must be able to access this website using *Website* button on the top bar, please note that you should see Connected successfully message while accessing this page.

To deploy the static website using Kubernetes with Nginx, PHP-FPM, and MySQL, follow these detailed steps:

## Solution

### 1. **Create Kubernetes Secrets**

Secrets are crucial for securely managing sensitive data like passwords and database credentials. We will create the following secrets:

- **MySQL Root Password**: This secret contains the root password for MySQL.
- **MySQL User Credentials**: This secret includes both the username and password for a MySQL user.
- **MySQL Database Name**: This secret holds the name of the MySQL database.
- **MySQL Host**: This secret specifies the hostname of the MySQL service.

Execute these commands to create the secrets:

```bash
# Create secret for MySQL root password
kubectl create secret generic mysql-root-pass --from-literal=password=R00t

# Create secret for MySQL user credentials
kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_cap --from-literal=password=ksH85UJjhb

# Create secret for MySQL database name
kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db8

# Create secret for MySQL host
kubectl create secret generic mysql-host --from-literal=host=mysql-service
```

### 2. **Create ConfigMap for PHP Configuration**

A ConfigMap allows you to manage configuration data. Here, we will create a ConfigMap for `php.ini` to set PHP configuration parameters.

Save the following content to a file named `php-config.yaml`:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: php-config
data:
  php.ini: |
    variables_order = "EGPCS"
```

Apply the ConfigMap:

```bash
kubectl apply -f php-config.yaml
```

### 3. **Create the Deployment**

The deployment will manage two containers: one for Nginx with PHP and one for MySQL. We will also mount the ConfigMap in the Nginx container and set environment variables from the secrets.

Save the following content to a file named `lemp-wp-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: lemp-wp
  name: lemp-wp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lemp-wp
  template:
    metadata:
      labels:
        app: lemp-wp
    spec:
      containers:
      - name: nginx-php-container
        image: webdevops/php-nginx:alpine-3-php7
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
      - name: mysql-container
        image: mysql:5.6
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
```

Apply the deployment:

```bash
kubectl apply -f lemp-wp-deployment.yaml
```

### 4. **Create Services**

Services will expose the application and MySQL database within the Kubernetes cluster.

Save the following content to a file named `services.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: lemp-service
spec:
  selector:
    app: lemp-wp
  ports:
  - port: 80
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
    app: lemp-wp
  ports:
  - port: 3306
    targetPort: 3306
    protocol: TCP
  type: ClusterIP
```

Apply the services:

```bash
kubectl apply -f services.yaml
```

### 5. **Copy and Configure `index.php`**

To ensure the application works correctly, you need to copy the `index.php` file into the Nginx container and update it to use environment variables.

1. **Copy `index.php` to the Container:**

   Find the Nginx pod name:

   ```bash
   kubectl get pods
   ```

   Copy the file into the container:

   ```bash
   kubectl cp /tmp/index.php <nginx-pod-name>:/app
   ```

2. **Update `index.php`:**

   Access the Nginx container:

   ```bash
   kubectl exec -it <nginx-pod-name> -- /bin/sh
   ```

   Navigate to the `/app` directory and open `index.php`. Replace the dummy values with environment variables:

   ```php
   $dbname = $_ENV["MYSQL_DATABASE"];
   $dbuser = $_ENV["MYSQL_USER"];
   $dbpass = $_ENV["MYSQL_PASSWORD"];
   $dbhost = $_ENV["MYSQL_HOST"];
   ```

3. **Save the Changes and Exit:**

   After making the changes, exit the container.

### 6. **Access the Website**

You should now be able to access the website via the NodePort service. Open your browser and navigate to:

```
http://<node-ip>:30008
```

You should see the "Connected successfully" message if everything has been set up correctly.

### Summary

In this setup, you've successfully configured and deployed a static website using Kubernetes, Nginx with PHP-FPM, and MySQL. The process involved creating Kubernetes secrets for secure data management, configuring PHP settings via a ConfigMap, setting up a deployment with appropriate containers, and exposing the services. The final step ensured that the `index.php` file dynamically uses environment variables for database connectivity.
