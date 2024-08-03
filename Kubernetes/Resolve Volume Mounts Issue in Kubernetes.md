## Problem Statement

We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:

The pod name is *nginx-phpfpm* and configmap name is *nginx-config*. Identify and fix the problem.

Once resolved, copy */home/thor/index.php* file from the *jump host* to the *nginx-container* within the nginx document root. After this, you should be able to access the website using *Website* button on the top bar.

## Solution

First we shall check the pods are running.

```
kubectl get pods
NAME           READY   STATUS    RESTARTS   AGE
nginx-phpfpm   2/2     Running   0          113s
```

To View the Services present.

```
kubectl get svc
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP          10m
nginx-service   NodePort    10.96.233.77   <none>        8099:30008/TCP   3m3s
```

Here we can see the port nginx listens at 8099 and nodeport 30008.

Now we shall check the any configmaps created.

```
kubectl get configmaps 
```

```
NAME               DATA   AGE
kube-root-ca.crt   1      12m
nginx-config       1      4m40s
```

We shall check if any errors in nginx-config.

```
kubectl describe configmaps nginx-config 
```

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

Now we shall check any issues in nginx-phpfpm container.

```
kubectl describe pod nginx-phpfpm 
```

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

We can see that the mount file path was specified wrong

```
Mounts:
      /etc/nginx/nginx.conf from nginx-config-volume (rw,path="nginx.conf")
     ** /usr/share/nginx/html from shared-files (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-fgcgs (to)
```

We should change */usr/share/nginx/html* to */var/www/html*

```
kubectl apply -f /tmp/kubectl-edit-365337046.yaml  --force
pod/nginx-phpfpm configured
```

Now we need to copy the */home/thor/index.php* from local to *nginx-container*.

```
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container
```
