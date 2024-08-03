## Problem Statement:

The Nautilus Application Development team is planning to deploy one of the php-based applications on Kubernetes cluster. As per the recent discussion with DevOps team, they have decided to use nginx and phpfpm. Additionally, they also shared some custom configuration requirements. Below you can find more details. Please complete this task as per requirements mentioned below:



- Create a service to expose this app, the service type must be *NodePort*, nodePort should be *30012*.


- Create a config map named *nginx-config* for *nginx.conf* as we want to add some custom settings in nginx.conf.


     - Change the default port *80* to *8096* in nginx.conf.


     - Change the default document root */usr/share/nginx* to */var/www/html* in nginx.conf.


     - Update the directory index to *index  index.html index.htm index.php* in nginx.conf.


- Create a pod named *nginx-phpfpm*.


- Create a shared volume named *shared-files* that will be used by both containers (nginx and phpfpm) also it should be a *emptyDir* volume.


- Map the ConfigMap we declared above as a volume for nginx container. Name the volume as *nginx-config-volume*, mount path should be */etc/nginx/nginx.conf* and subPath should be nginx.conf


- Nginx container should be named as *nginx-container* and it should use *nginx:latest* image. PhpFPM container should be named as *php-fpm-container* and it should use *php:8.3-fpm-alpine* image.


- The shared volume *shared-files* should be mounted at */var/www/html* location in both containers. Copy */opt/index.php* from jump host to the nginx document *root* inside the nginx container, once done you can access the app using *App* button on the top bar.


- Before clicking on finish button always make sure to check if all pods are in running state.
## Solution:

We need to create an configmap _nginx-config_ where it listens to port _8096_ and nodeport service where it exposes at port _30012_ and a pod with two containers _php-fpm-container_ and _nginx-container_ with given images above.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  nginx.conf: |
    events {} 
    http {
      server {
        listen 8096;
        index index.html index.htm index.php;
        root  /var/www/html;
        location ~ \.php$ {
          include fastcgi_params;
          fastcgi_param REQUEST_METHOD $request_method;
          fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
          fastcgi_pass 127.0.0.1:9000;
        }
      }
    }
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx-phpfpm
  labels:
    name: nginx-phpfpm
spec:
  volumes: 
  - name: shared-files
    emptyDir: {}
  - name: nginx-config-volume
    configMap:
      name: nginx-config
  containers:
  - name: nginx-container
    image: nginx:latest
    volumeMounts:
    - name: nginx-config-volume
      mountPath: /etc/nginx/nginx.conf
      subPath: nginx.conf
    - name: shared-files
      mountPath: /var/www/html

  - name: php-fpm-container
    image: php:7.4-fpm-alpine
    volumeMounts:
    - name: shared-files
      mountPath: /var/www/html
    
---

kind: Service
apiVersion: v1
metadata:
  name: nginx-phpfpm
spec:
  selector:
    name: nginx-phpfpm
  type: NodePort
  ports:
  - port: 8096
    targetPort: 8096
    nodePort: 30012

```
Now we need to copy the file from local _/opt/index.php_ to _nginx-container_ to the root _/var/www/html_
```
kubectl cp /opt/index.php nginx-phpfpm:/var/www/html
```