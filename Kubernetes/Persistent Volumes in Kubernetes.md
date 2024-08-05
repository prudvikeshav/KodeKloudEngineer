## Problem Statement

The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:

- Create a PersistentVolume named as *pv-nautilus*. Configure the spec as storage class should be *manual*, set capacity to *4Gi*, set access mode to *ReadWriteOnce*, volume type should be hostPath and set path to */mnt/data* (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

- Create a PersistentVolumeClaim named as *pvc-nautilus*. Configure the spec as storage class should be *manual*, request *1Gi* of the storage, set access mode to *ReadWriteOnce*.

- Create a pod named as *pod-nautilus*, mount the persistent volume you created with claim name *pvc-nautilus* at document root of the web server, the container within the pod should be named as *container-nautilus* using image *nginx* with latest tag only (remember to mention the tag i.e nginx:latest).

- Create a node port type service named *web-nautilus* using node port *30008* to expose the web server running within the pod.

## Solution

We need to deploy a web application on a Kubernetes cluster and requires the use of Persistent Volumes (PV) and Persistent Volume Claims (PVC) to handle storage requirements. Below are the detailed steps and configurations for setting up the necessary resources:

1. **Persistent Volume (PV) Configuration:**
   - **Name**: `pv-nautilus`
   - **Storage Class**: `manual`
   - **Capacity**: `4Gi`
   - **Access Mode**: `ReadWriteOnce`
   - **Volume Type**: `hostPath`
   - **Path**: `/mnt/data` (This path is specified on the host and is assumed to be available on the nodes.)

   The Persistent Volume will be used to provide a block of storage to the application. The `hostPath` type allows the volume to be backed by a directory on the node’s filesystem.

   ```yaml
   apiVersion: v1
   kind: PersistentVolume
   metadata:
     name: pv-nautilus
   spec:
     storageClassName: manual
     capacity:
       storage: 4Gi
     accessModes:
       - ReadWriteOnce
     hostPath:
       path: /mnt/data
     persistentVolumeReclaimPolicy: Retain
   ```

2. **Persistent Volume Claim (PVC) Configuration:**
   - **Name**: `pvc-nautilus`
   - **Storage Class**: `manual`
   - **Requested Storage**: `1Gi`
   - **Access Mode**: `ReadWriteOnce`

   The Persistent Volume Claim will request storage from the previously defined Persistent Volume. It ensures that the required storage capacity and access mode are available for the application.

   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: pvc-nautilus
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 1Gi
     storageClassName: manual
   ```

3. **Pod Configuration:**
   - **Name**: `pod-nautilus`
   - **Container Name**: `container-nautilus`
   - **Image**: `nginx:latest`
   - **Volume Mount**: Mount the Persistent Volume Claim (`pvc-nautilus`) to `/usr/share/nginx/html`, which is the default document root for Nginx.

   The pod is configured with an Nginx container that uses the mounted volume as its document root. This setup allows the web server to serve files from the Persistent Volume.

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: pod-nautilus
   spec:
     containers:
     - image: nginx:latest
       name: container-nautilus
       volumeMounts:
       - name: pv-nautilus
         mountPath: /usr/share/nginx/html
     volumes:
     - name: pv-nautilus
       persistentVolumeClaim:
         claimName: pvc-nautilus
     restartPolicy: Always
   ```

4. **NodePort Service Configuration:**
   - **Name**: `web-nautilus`
   - **Type**: `NodePort`
   - **Port**: `80` (for internal cluster access)
   - **NodePort**: `30008` (external port for accessing the service from outside the cluster)
   - **Target Port**: `80` (matches the port exposed by the Nginx container)

   The NodePort service will expose the Nginx web server to external traffic through port `30008`, allowing access to the application from outside the Kubernetes cluster.

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: web-nautilus
   spec:
     type: NodePort
     ports:
     - port: 80
       targetPort: 80
       nodePort: 30008
     selector:
       app: pod-nautilus
   ```

### Applying the Configuration

1. **Create Persistent Volume and Persistent Volume Claim:**

   ```bash
   kubectl apply -f pv.yaml
   kubectl apply -f pvc.yaml
   ```

2. **Create Pod:**

   ```bash
   kubectl apply -f pod.yaml
   ```

3. **Create NodePort Service:**

   ```bash
   kubectl apply -f service.yaml
   ```

### Verification

**Check Persistent Volumes:**

```bash
kubectl get pv
```

```
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
pv-nautilus   4Gi        RWO            Retain           Available           manual                  7s
```

**Check Persistent Volume Claims:**

```bash
kubectl get pvc
```

```

NAME           STATUS   VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nautilus   Bound    pv-nautilus   4Gi        RWO            manual         13s

```

**Check Pods:**

```bash
kubectl get pods
```

```
NAME           READY   STATUS    RESTARTS   AGE
pod-nautilus   1/1     Running   0          11s
```

**Check Services:**

```bash
kubectl get svc
```

```

NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP        80m
web-nautilus   NodePort    10.96.232.237   <none>        80:30008/TCP   5s

```

Ensure all resources are created and running as expected. The web application should be accessible through the NodePort service on port `30008`.
