**Problem Statement**

#### We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it

- #### Create a pod named volume-share-datacenter

- #### For the first container, use image debian with latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-datacenter-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/media

- #### For the second container, use image debian with the latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-datacenter-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/games

- #### Volume name should be volume-share of type emptyDir

- #### After creating the pod, exec into the first container i.e volume-container-datacenter-1, and just for testing create a file media.txt with any content under the mounted path of first container i.e /tmp/media

- #### The file media.txt should be present under the mounted path /tmp/games on the second container volume-container-datacenter-2 as well, since they are using a shared volume

**Solution:**

#### We need to create pod

```yaml
apiVersion: apps/v1
kind: Pod
metadata:
   name: volume-share-datacenter
spec:
   containers:
   -  name: volume-container-datacenter-1
      image: debian:latest
      command: 
      - "/bin/sh"
      - "-c" 
      - sleep 60
      volumeMounts:
      -  name: volume-share
         mountPath: /tmp/media
    -  name: volume-container-datacenter-2
      image: debian:latest
      command: 
      - "/bin/sh"
      - "-c"
      - sleep 60
      volumeMounts:
      -  name: volume-share
         mountPath: /tmp/games
   volumes:
   -  name: volume-share
      emptyDir: {}
```

#### Apply the pod

```bash
kubectl apply -f pod.yaml
```

```
pod/volume-share-datacenter created
```

#### To view created pods

```bash
kubectl get pods
```

```
NAME                      READY   STATUS    RESTARTS   AGE
volume-share-datacenter   2/2     Running   0          22s
```

#### We need create a file media.txt on container 1

kubectl exec -it volume-share-datacenter /bin/bash -c volume-container-datacenter-1

```
root@volume-share-datacenter:/# cd /tmp/media/
root@volume-share-datacenter:/tmp/media# touch media.txt
```

#### We need to check the  wheather the file is present at /tcp/games on container 2

```bash
kubectl exec -it volume-share-datacenter /bin/bash -c volume-container-datacenter-2
```

```
root@volume-share-datacenter:/# cd /tmp/games/
root@volume-share-datacenter:/tmp/games# ls
media.txt
```
