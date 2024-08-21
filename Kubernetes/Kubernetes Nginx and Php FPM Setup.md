## Problem Statement

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
To deploy a PHP-based application on a Kubernetes cluster with custom Nginx and PHP-FPM configurations, follow these steps:

### Solution

#### 1. **Create the ConfigMap for Nginx Configuration**

The `nginx-config` ConfigMap will contain the custom Nginx configuration. Specifically, this configuration will:

- Change the listening port from 80 to 8096.
- Set the document root to `/var/www/html`.
- Update the directory index to include `index.html`, `index.htm`, and `index.php`.

Save the following YAML content to a file named `nginx-config.yaml`:

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
        root /var/www/html;
        location ~ \.php$ {
          include fastcgi_params;
          fastcgi_param REQUEST_METHOD $request_method;
          fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
          fastcgi_pass 127.0.0.1:9000;
        }
      }
    }
```

Apply the ConfigMap:

```bash
kubectl apply -f nginx-config.yaml
```

#### 2. **Create the Pod with Nginx and PHP-FPM Containers**

We will create a Pod named `nginx-phpfpm` with two containers:

- **Nginx Container**: Uses the `nginx:latest` image and mounts the `nginx-config` ConfigMap.
- **PHP-FPM Container**: Uses the `php:8.3-fpm-alpine` image.

Both containers will share an emptyDir volume named `shared-files`, which will be mounted at `/var/www/html` in both containers.

Save the following YAML content to a file named `nginx-phpfpm-pod.yaml`:

```yaml
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
    image: php:8.3-fpm-alpine
    volumeMounts:
    - name: shared-files
      mountPath: /var/www/html
```

Apply the Pod configuration:

```bash
kubectl apply -f nginx-phpfpm-pod.yaml
```

#### 3. **Create the NodePort Service**

This service will expose the Nginx and PHP-FPM application. It will use port 8096 internally and expose it on port 30012.

Save the following YAML content to a file named `nginx-phpfpm-service.yaml`:

```yaml
apiVersion: v1
kind: Service
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

Apply the Service configuration:

```bash
kubectl apply -f nginx-phpfpm-service.yaml
```

#### 4. **Copy the `index.php` File to the Nginx Container**

To ensure the application is properly deployed, copy the `index.php` file from the jump host to the Nginx container's document root.

Use the following command to copy the file:

```bash
kubectl cp /opt/index.php nginx-phpfpm:/var/www/html
```

#### 5. **Verify Pod Status**

Ensure that all pods are running correctly:

```bash
kubectl get pods
```

Make sure the `nginx-phpfpm` pod is in the `Running` state before proceeding.

#### 6. **Access the Application**

You should be able to access the application by navigating to:

```
http://<node-ip>:30012
```

Where `<node-ip>` is the IP address of one of your Kubernetes nodes.

### Summary

You have configured and deployed a PHP-based application using Nginx and PHP-FPM on Kubernetes. The process included creating a ConfigMap for Nginx, setting up a Pod with shared volumes for Nginx and PHP-FPM, exposing the application through a NodePort service, and copying the necessary PHP file into the Nginx container. Ensure all pods are running correctly before accessing the application.
