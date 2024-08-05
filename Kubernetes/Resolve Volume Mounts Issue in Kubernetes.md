## Problem Statement

We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:

The pod name is *nginx-phpfpm* and the configmap name is *nginx-config*. Identify and fix the problem.

Once resolved, copy the */home/thor/index.php* file from the *jump host* to the *nginx-container* within the nginx document root. After this, you should be able to access the website using the *Website* button on the top bar.

## Solution

To solve the issue, we need to follow several steps to diagnose the problem and apply the necessary fixes. Below is a detailed explanation of each step along with the commands used.

### 1. Check the Pods

First, verify the status of the pods to ensure they are running.

```bash
kubectl get pods
```

Output:

```
NAME           READY   STATUS    RESTARTS   AGE
nginx-phpfpm   2/2     Running   0          113s
```

The pod *nginx-phpfpm* is running and both containers are ready.

### 2. Check the Services

Next, check the services to confirm the ports on which Nginx is listening.

```bash
kubectl get svc
```

Output:

```
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP          10m
nginx-service   NodePort    10.96.233.77   <none>        8099:30008/TCP   3m3s
```

The Nginx service listens on port 8099, with NodePort 30008.

### 3. Check ConfigMaps

Verify the configmaps to ensure the Nginx configuration is correctly loaded.

```bash
kubectl get configmaps
```

Output:

```
NAME               DATA   AGE
kube-root-ca.crt   1      12m
nginx-config       1      4m40s
```

Check the contents of the *nginx-config* configmap.

```bash
kubectl describe configmaps nginx-config
```

Output:

```
Name:         nginx-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
nginx.conf:
----
events {
}
http {
  server {
    listen 8099 default_server;
    listen [::]:8099 default_server;

    # Set nginx to serve files from the shared volume!
    root /var/www/html;
    index  index.html index.htm index.php;
    server_name _;
    location / {
      try_files $uri $uri/ =404;
    }
    location ~ \.php$ {
      include fastcgi_params;
      fastcgi_param REQUEST_METHOD $request_method;
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
      fastcgi_pass 127.0.0.1:9000;
    }
  }
}

BinaryData
====

Events:  <none>
```

The configuration in the configmap seems correct.

### 4. Check Pod Details

Examine the details of the *nginx-phpfpm* pod to identify any issues.

```bash
kubectl describe pod nginx-phpfpm
```

Output:

```
Name:             nginx-phpfpm
Namespace:        default
Priority:         0
Service Account:  default
Node:             kodekloud-control-plane/172.17.0.2
Start Time:       Sat, 03 Aug 2024 13:27:06 +0000
Labels:           app=php-app
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  php-fpm-container:
    Container ID:   containerd://d5c768fd0c6836321dc937698f5932322d6770af72ccf05c012601c07fa2e53c
    Image:          php:7.2-fpm-alpine
    Image ID:       docker.io/library/php@sha256:2e2d92415f3fc552e9a62548d1235f852c864fcdc94bcf2905805d92baefc87f
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sat, 03 Aug 2024 13:27:15 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-fgcgs (ro)
      /var/www/html from shared-files (rw)
  nginx-container:
    Container ID:   containerd://cd9e5fe44a619d726b0b05573d1f3369a49b260aaeca2d09aa5aaa50c6268f8d
    Image:          nginx:latest
    Image ID:       docker.io/library/nginx@sha256:6af79ae5de407283dcea8b00d5c37ace95441fd58a8b1d2aa1ed93f5511bb18c
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sat, 03 Aug 2024 13:27:22 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /etc/nginx/nginx.conf from nginx-config-volume (rw,path="nginx.conf")
      /usr/share/nginx/html from shared-files (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-fgcgs (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  shared-files:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
  nginx-config-volume:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      nginx-config
    Optional:  false
  kube-api-access-fgcgs:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  2m38s  default-scheduler  Successfully assigned default/nginx-phpfpm to kodekloud-control-plane
  Normal  Pulling    2m37s  kubelet            Pulling image "php:7.2-fpm-alpine"
  Normal  Pulled     2m30s  kubelet            Successfully pulled image "php:7.2-fpm-alpine" in 7.585973665s (7.585997968s including waiting)
  Normal  Created    2m30s  kubelet            Created container php-fpm-container
  Normal  Started    2m29s  kubelet            Started container php-fpm-container
  Normal  Pulling    2m29s  kubelet            Pulling image "nginx:latest"
  Normal  Pulled     2m22s  kubelet            Successfully pulled image "nginx:latest" in 7.203594653s (7.203614392s including waiting)
  Normal  Created    2m22s  kubelet            Created container nginx-container
  Normal  Started    2m22s  kubelet            Started container nginx-container
```

### 5. Identify the Issue

The mounting path for the shared volume in the Nginx container is incorrect. It should be `/var/www/html` instead of `/usr/share/nginx/html`.

Incorrect:

```
/usr/share/nginx/html from shared-files (rw)
```

Correct:

```
/var/www/html from shared-files (rw)
```

### 6. Fix the Mount Path

To fix the mount path, update the Pod configuration and apply the changes.

```bash
kubectl apply -f /tmp/kubectl-edit-365337046.yaml --force
```

Output:

```
pod/nginx-phpfpm configured
```

### 7. Copy the PHP File

After fixing the issue, copy the `index.php` file from the jump host to the `nginx-container` within the Nginx document root.

```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container
```

### Verification

To verify that the issue is resolved and the website is accessible, use the *Website* button on the top bar. The website should now be functional, serving the PHP file correctly.

This concludes the detailed solution to investigate and rectify the Nginx and PHP
