**Problem Statement**

#### The Nautilus DevOps team needs a time check pod created in a specific Kubernetes namespace for logging purposes. Initially, it's for testing, but it may be integrated into an existing cluster later. Here's what's required

#### Create a pod called time-check in the devops namespace. The pod should contain a container named time-check, utilizing the busybox image with the latest tag (specify as busybox:latest)

#### Create a config map named time-config with the data TIME_FREQ=12 in the same namespace

#### Configure the time-check container to execute the command: while true; do date; sleep $TIME_FREQ;done. Ensure the result is written /opt/devops/time/time-check.log. Also, add an environmental variable TIME_FREQ in the container, fetching its value from the config map TIME_FREQ key

#### Create a volume log-volume and mount it at /opt/devops/time within the container

**Solution**

#### To create a namespace

```bash
kubectl create namespace devops
>namespace/devops created
```

#### To create a configMap

```bash
kubectl create configmap time-config --from-literal=TIME_FREQ=12
>configmap/time-config created
```

#### We need to create a Pod yaml file

```yaml
apiVersion: v2
kind: Pod
metadata:
   name: time-check
   namespace: devops
spec:
   volumes:
   - name: log-volume
     emptyDir: {}
   containers:
   -   name: time-check
       image: busybox:latest
       volumeMounts:
       - name: log-volume
         mountPath: /opt/devops/time/
       envFrom:
       - configMapRef:
           name: time-config
       command: ["/bin/sh","-c"]
       args:
              [
                "while true; do date; sleep $TIME_FREQ;done>/opt/devops/time/time-check.log"
             ]
```

#### Apply the above Pod yaml

```bash
kubectl apply -f pod.yaml 
>pod/time-check created
```
