## Problem Statement:
The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:


- Create a PersistentVolume named as *pv-nautilus*. Configure the spec as storage class should be *manual*, set capacity to *4Gi*, set access mode to *ReadWriteOnce*, volume type should be hostPath and set path to */mnt/data* (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

- Create a PersistentVolumeClaim named as *pvc-nautilus*. Configure the spec as storage class should be *manual*, request *1Gi* of the storage, set access mode to *ReadWriteOnce*.

- Create a pod named as *pod-nautilus*, mount the persistent volume you created with claim name *pvc-nautilus* at document root of the web server, the container within the pod should be named as *container-nautilus* using image *nginx* with latest tag only (remember to mention the tag i.e nginx:latest).

- Create a node port type service named *web-nautilus* using node port *30008* to expose the web server running within the pod.

## Solution:
We need to create a persisitant volume and persistant volume claim with the given requirements.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nautilus
spec:
  storageClassName: manual
  capacity:
    storage: 4Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nautilus
spec:
  resources:
    requests:
      storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
```
```
kubectl apply -f pv.yaml 
persistentvolume/pv-nautilus created
persistentvolumeclaim/pvc-nautilus created
```
To check the persistant volume created.
```
kubectl get pv
```
```
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
pv-nautilus   4Gi        RWO            Retain           Available           manual                  7s
```
To Check the persistant volume claim created.

```
kubectl get pvc
```
```
NAME           STATUS   VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nautilus   Bound    pv-nautilus   4Gi        RWO            manual         13s
```
Now we need to create a pod where we will be utilizing the created persitant volume for data storage for pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: pod-nautilus
  name: pod-nautilus
spec:
  containers:
  - image: nginx:latest
    name: container-nautilus
    volumeMounts: 
    - name: pv-nautilus
      mountPath: /mnt/data
    resources: {}
  volumes:
  - name: pv-nautilus
    persistentVolumeClaim:
        claimName: pvc-nautilus
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
```
kubectl get pods
NAME           READY   STATUS    RESTARTS   AGE
pod-nautilus   1/1     Running   0          11s
```
Now we need to create an nodeport service for exposing the webserver running in the pod.
```
kubectl create service nodeport web-nautilus --node-port=30008 --tcp=80:80 --dry-run=client -o yaml
```
```yaml

apiVersion: v1
kind: Service
metadata:
  labels:
    app: web-nautilus
  name: web-nautilus
spec:
  ports:
  - name: web-nautilus 
    nodePort: 30008
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: pod-nautilus
  type: NodePort
status:
  loadBalancer: {}
```
To check the nodeport service created.
```
kubectl get svc
```
```
NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP        80m
web-nautilus   NodePort    10.96.232.237   <none>        80:30008/TCP   5s
```

