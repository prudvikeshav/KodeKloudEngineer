**Problem statement:**

#### The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details

- ####  Create a deployment named grafana-deployment-devops using any grafana image for Grafana app. Set other parameters as per your choice

- ####  Create NodePort type service with nodePort 32000 to expose the app

**Solution:**

#### TO create a deployment

```bash
kubectl create deployment grafana-deployment-devops --image=grafana/grafana
```

#### To view the created container

```bash
kubectl get pods
```

```
NAME                                       READY   STATUS    RESTARTS   AGE
grafana-deployment-devops-9b97dff6-kjxf2   1/1     Running   0          25s
```

#### Create a service with nodePort

```bash
kubectl create svc nodeport grafana-service-devops --tcp=80:3000
```

```
service/grafana-service-devops created
```

#### to view the created service

```bash
kubectl get svc
```

```
NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
grafana-service-devops   NodePort    10.96.158.245   <none>        80:32000/TCP   77s
kubernetes               ClusterIP   10.96.0.1       <none>        443/TCP        72m
```

#### Check the service created with desired choices

```bash
kubectl describe svc grafana-service-devops
```

```
Name:                     grafana-service-devops
Namespace:                default
Labels:                   app=grafana-service-devops
Annotations:              <none>
Selector:                 app=grafana-deployment-devops
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.96.158.245
IPs:                      10.96.158.245
Port:                     grafana  80/TCP
TargetPort:               3000/TCP
NodePort:                 grafana  32000/TCP
Endpoints:                10.244.0.6:3000
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```
