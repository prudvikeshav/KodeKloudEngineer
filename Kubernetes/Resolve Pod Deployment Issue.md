## Problem Statement:

 A junior DevOps team member encountered difficulties deploying a stack on the Kubernetes cluster. The pod fails to start, presenting errors. Let's troubleshoot and rectify the issue promptly

- There is a pod named *webserver*, and the container within it is named *httpd-container*, its utilizing the *httpd:latest* image

 - Additionally, there's a sidecar container named *sidecar-container* using the *ubuntu:latest* image

 - Identify and address the issue to ensure the pod is running and the application is accessible

## Solution:

Check if the pods running.

```
kubectl get pods
NAME        READY   STATUS             RESTARTS   AGE
webserver   1/2     ImagePullBackOff   0          89s
```

 Here we can see the pod is not running Status was ImagePullBackOff. There are two possibilities

 1. Wrong image name

 2. The image was not present

 Now we should see the complete details of pod webserver

```json
thor@jumphost ~$ kubectl describe pod webserver 
Name:             webserver
Namespace:        default
Priority:         0
Service Account:  default
Node:             kodekloud-control-plane/172.17.0.2
Start Time:       Thu, 18 Jul 2024 05:51:26 +0000
Labels:           app=web-app
Annotations:      <none>
Status:           Pending
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  httpd-container:
    Container ID:   
    Image:          httpd:latests
    Image ID:       
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       ImagePullBackOff
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/log/httpd from shared-logs (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-x8t5d (ro)
  sidecar-container:
    Container ID:  containerd://a3c7c130a43f560e2f09ff72366aa42e9d07aa9818ae636be44dbf5105517161
    Image:         ubuntu:latest
    Image ID:      docker.io/library/ubuntu@sha256:2e863c44b718727c860746568e1d54afd13b2fa71b160f5cd9058fc436217b30
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      while true; do cat /var/log/httpd/access.log /var/log/httpd/error.log; sleep 30; done
    State:          Running
      Started:      Thu, 18 Jul 2024 05:51:31 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/log/httpd from shared-logs (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-x8t5d (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  shared-logs:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
  kube-api-access-x8t5d:
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
  Type     Reason     Age                 From               Message
  ----     ------     ----                ----               -------
  Normal   Scheduled  104s                default-scheduler  Successfully assigned default/webserver to kodekloud-control-plane
  Normal   Pulling    103s                kubelet            Pulling image "ubuntu:latest"
  Normal   Pulled     100s                kubelet            Successfully pulled image "ubuntu:latest" in 3.536798142s (3.536807827s including waiting)
  Normal   Created    99s                 kubelet            Created container sidecar-container
  Normal   Started    99s                 kubelet            Started container sidecar-container
  Normal   Pulling    56s (x3 over 103s)  kubelet            Pulling image "httpd:latests"
  Warning  Failed     56s (x3 over 103s)  kubelet            Failed to pull image "httpd:latests": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/library/httpd:latests": failed to resolve reference "docker.io/library/httpd:latests": docker.io/library/httpd:latests: not found
  Warning  Failed     56s (x3 over 103s)  kubelet            Error: ErrImagePull
  Normal   BackOff    18s (x6 over 99s)   kubelet            Back-off pulling image "httpd:latests"
  Warning  Failed     18s (x6 over 99s)   kubelet            Error: ImagePullBackOff
```

 We can clearly in the log that image name was specified wrong httpd:latests insted of httpd:latest. We should edit the pod and modify the image name

```bash
thor@jumphost ~$ kubectl edit pod webserver
pod/webserver edited
```

 Check whether the pod is running or not.

```bash
thor@jumphost ~$ kubectl get pod
NAME        READY   STATUS    RESTARTS      AGE
webserver   2/2     Running   1 (28s ago)   11m
```
