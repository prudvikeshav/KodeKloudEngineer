# **Problem Statement**

Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.

T- he deployment name is redis-deployment. The pods are not in running state right now, so please look into the issue and fix the same.

# **Solution:**

First check the pods that are present

```
kubectl get pods
```

```
NAME                                READY   STATUS              RESTARTS   AGE
redis-deployment-54cdf4f76d-lvj2m   0/1     ContainerCreating   0          52s
```

As we know a redis deployment has been deployed before and we need to check why it is failing so we need to describe deployment to check configuration of the deployment

```
kubectl describe deployments.apps redis-deployment 
```

```
Name:                   redis-deployment
Namespace:              default
CreationTimestamp:      Tue, 30 Jul 2024 04:46:52 +0000
Labels:                 app=redis
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=redis
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=redis
  Containers:
   redis-container:
    Image:      redis:alpin
    Port:       6379/TCP
    Host Port:  0/TCP
    Requests:
      cpu:        300m
    Environment:  <none>
    Mounts:
      /redis-master from config (rw)
      /redis-master-data from data (rw)
  Volumes:
   data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
   config:
    Type:          ConfigMap (a volume populated by a ConfigMap)
    Name:          redis-conig
    Optional:      false
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      False   MinimumReplicasUnavailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  <none>
NewReplicaSet:   redis-deployment-54cdf4f76d (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  98s   deployment-controller  Scaled up replica set redis-deployment-54cdf4f76d to 1
```

From the describe of the deployment we can see the wrong image name was specified. that needs to be updated. In order find all errors we can check on events.

```
kubectl get  events
```

```
LAST SEEN   TYPE      REASON                    OBJECT                                   MESSAGE
9m12s       Normal    Starting                  node/kodekloud-control-plane             Starting kubelet.
9m11s       Normal    NodeHasSufficientMemory   node/kodekloud-control-plane             Node kodekloud-control-plane status is now: NodeHasSufficientMemory
9m11s       Normal    NodeHasNoDiskPressure     node/kodekloud-control-plane             Node kodekloud-control-plane status is now: NodeHasNoDiskPressure
9m11s       Normal    NodeHasSufficientPID      node/kodekloud-control-plane             Node kodekloud-control-plane status is now: NodeHasSufficientPID
9m11s       Normal    NodeAllocatableEnforced   node/kodekloud-control-plane             Updated Node Allocatable limit across pods
9m1s        Normal    RegisteredNode            node/kodekloud-control-plane             Node kodekloud-control-plane event: Registered Node kodekloud-control-plane in Controller
8m53s       Normal    Starting                  node/kodekloud-control-plane
8m52s       Normal    NodeReady                 node/kodekloud-control-plane             Node kodekloud-control-plane status is now: NodeReady
7m43s       Normal    Scheduled                 pod/redis-deployment-54cdf4f76d-lvj2m    Successfully assigned default/redis-deployment-54cdf4f76d-lvj2m to kodekloud-control-plane
90s         Warning   FailedMount               pod/redis-deployment-54cdf4f76d-lvj2m    `**MountVolume.SetUp failed for volume "config" : configmap "redis-conig" not found**`
69s         Warning   FailedMount               pod/redis-deployment-54cdf4f76d-lvj2m    Unable to attach or mount volumes: unmounted volumes=[config], unattached volumes=[], failed to process volumes=[]: timed out waiting for the condition

7m43s       Normal    SuccessfulCreate          replicaset/redis-deployment-54cdf4f76d   Created pod: redis-deployment-54cdf4f76d-lvj2m
7m43s       Normal    ScalingReplicaSet         deployment/redis-deployment              Scaled up replica set redis-deployment-54cdf4f76d to 1
```

Now we can also observe the configMap name was specified wrong. its also be updated.
