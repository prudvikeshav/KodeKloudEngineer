# **Problem Statement:**
There are a number of parameters that are used by the applications. We need to define these as environment variables, so that we can use them as needed within different configs. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about the same.


- Create a pod named envars.

- Container name should be fieldref-container, use image busybox preferable latest tag, use command 'sh', '-c' and args should be

'while true; do
      echo -en '/n';
                                  printenv NODE_NAME POD_NAME;
                                  printenv POD_IP POD_SERVICE_ACCOUNT;
              sleep 10;
         done;'

(Note: please take care of indentations)

- Define Four environment variables as mentioned below:
    a.) The first env should be named as NODE_NAME, set valueFrom fieldref and fieldPath should be spec.nodeName.

    b.) The second env should be named as POD_NAME, set valueFrom fieldref and fieldPath should be metadata.name.

    c.) The third env should be named as POD_IP, set valueFrom fieldref and fieldPath should be status.podIP.

    d.) The fourth env should be named as POD_SERVICE_ACCOUNT, set valueFrom fieldref and fieldPath shoulbe be spec.serviceAccountName.

- Set restart policy to Never.

- To check the output, exec into the pod and use printenv command.

# **Solution:**

The pod yaml for the given requirements

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: envars
spec:
  containers:
  - image: busybox:latest
    name: fieldref-container
    command: ["sh","-c"]
    args:
    - while true; do
      echo -en '/n';
                                  printenv NODE_NAME POD_NAME;
                                  printenv POD_IP POD_SERVICE_ACCOUNT;
              sleep 10;
         done;
    env:
    - name: NODE_NAME
      valueFrom:
            fieldRef:
              fieldPath: spec.nodeName

    - name: POD_NAME
      valueFrom: 
            fieldRef:
              fieldPath: metadata.name

    - name: POD_IP
      valueFrom:
            fieldRef:
              fieldPath: status.podIP

    - name: POD_SERVICE_ACCOUNT
      valueFrom:
            fieldRef:
              fieldPath: spec.serviceAccountName
  restartPolicy: Never
```
To check the pod created
```
kubectl get pods
NAME     READY   STATUS    RESTARTS   AGE
envars   1/1     Running   0          6s
```
Now we need to check the envronment variables in the pod by executing printenv inside the container
```
kubectl exec -it envars sh
```
Output

```
/ # printenv
POD_IP=10.244.0.5
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.96.0.1:443
HOSTNAME=envars
SHLVL=1
HOME=/root
NODE_NAME=kodekloud-control-plane
TERM=xterm
POD_NAME=envars
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
POD_SERVICE_ACCOUNT=default
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_SERVICE_HOST=10.96.0.1
PWD=/
```