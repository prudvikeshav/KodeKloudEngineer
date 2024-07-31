# **Problem Statement:**
The Nautilus DevOps team want to deploy a PHP website on Kubernetes cluster. They are going to use Apache as a web server and Mysql for database. The team had already gathered the requirements and now they want to make this website live. Below you can find more details:



  1) Create a config map php-config for php.ini with variables_order = "EGPCS" data.


  2) Create a deployment named lamp-wp.


  3) Create two containers under it. First container must be httpd-php-container using image webdevops/php-apache:alpine-3-php7 and second container must be mysql-container from image mysql:5.6. Mount php-config configmap in httpd container at /opt/docker/etc/php/php.ini location.


  4) Create kubernetes generic secrets for mysql related values like myql root password, mysql user, mysql password, mysql host and mysql database. Set any values of your choice.


  5) Add some environment variables for both containers:


     - a) MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD and MYSQL_HOST. Take their values from the secrets you created. Please make sure to use env field (do not use envFrom) to define the name-value pair of environment variables.


  6) Create a node port type service lamp-service to expose the web application, nodePort must be 30008.


  7) Create a service for mysql named mysql-service and its port must be 3306.


  8) We already have /tmp/index.php file on jump_host server.


     - a) Copy this file into httpd container under Apache document root i.e /app and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters. Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.


     - b) You must be able to access this index.php on node port 30008 at the end, please note that you should see Connected successfully message while accessing this page.

# **Solution:**

First we need to create kubernetes generic secrets for mysql related values.

```bash
kubectl create secret generic mysql-root-pass --from-literal=password=R00t
kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_pop --from-literal=password=BruCStnMT5
kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db5
kubectl create secret generic mysql-host --from-literal=host=mysql-service
```
Now we shall create a configmap php-config for php.ini with variables_order = "EGPCS" data.
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: php-config
data:
  php.ini: |
    variables_order = "EGPCS"

```
After creating the configmap we shall create a deployment lamp-wp with the requirements given

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
A service nodeport service to be created for expose the website and a mysql service required to connect to database.

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

Now we need to copy the contents present in /tmp/index.php into the container httpd-php=container.

```
kubectl cp /tmp/index.php lamp-wp-7c9cfc699c-kvfhd:/app
```
```
Defaulted container "httpd-php-container" out of: httpd-php-container, mysql-container
```
The copy was successful we need to check wheather the webiste working on clicking on website button.
we can see the website hasnt connected. its because the dummy values are present in index.php. we need to update them.

Replace the dummy values with the below
```
$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];
```
After replacing we can connect to the website.
