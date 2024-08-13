 **Problem Statement:**
There are a number of parameters that are used by the applications. We need to define these as environment variables, so that we can use them as needed within different configs. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about the same.


- Create a pod named *envars*.

- Container name should be *fieldref-container*, use image *busybox* preferable latest tag, use command *'sh'*, *'-c'* and args should be

*'while true; do
      echo -en '/n';
                                  printenv NODE_NAME POD_NAME;
                                  printenv POD_IP POD_SERVICE_ACCOUNT;
              sleep 10;
         done;'*

(Note: please take care of indentations)

- Define Four environment variables as mentioned below:
    a.) The first env should be named as NODE_NAME, set valueFrom fieldref and fieldPath should be *spec.nodeName*.

    b.) The second env should be named as POD_NAME, set valueFrom fieldref and fieldPath should be *metadata.name*.

    c.) The third env should be named as POD_IP, set valueFrom fieldref and fieldPath should be *status.podIP*.

    d.) The fourth env should be named as POD_SERVICE_ACCOUNT, set valueFrom fieldref and fieldPath shoulbe be *spec.serviceAccountName*.

- Set restart policy to *Never*.

- To check the output, exec into the pod and use *printenv* command.

To configure and deploy the Kubernetes Pod with the specified environment variables and settings, follow the solution provided below:

## **Solution**

### **1. Create the Pod Configuration**

Create a YAML configuration file for the pod named `envars` that meets the requirements outlined. Save this YAML configuration to a file named `envars-pod.yaml`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: envars
spec:
  containers:
  - image: busybox:latest
    name: fieldref-container
    command: ["sh", "-c"]
    args:
    - |
      while true; do
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

### **2. Apply the YAML File**

Deploy the pod using the `kubectl apply` command:

```bash
kubectl apply -f envars-pod.yaml
```

### **3. Verify the Pod Status**

Check the status of the pod to ensure it's running correctly:

```bash
kubectl get pods
```

You should see output similar to:

```
NAME     READY   STATUS    RESTARTS   AGE
envars   1/1     Running   0          6s
```

### **4. Check the Environment Variables**

To inspect the environment variables within the running pod, execute the `printenv` command inside the container:

```bash
kubectl exec -it envars -- sh
```

Once inside the pod, run:

```sh
printenv
```

### **Expected Output**

You should see the environment variables along with other system environment variables:

```
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

### **Explanation**

- **Pod Name**: `envars`
- **Container Name**: `fieldref-container`
- **Image**: `busybox:latest`
- **Command**: Executes a shell command that continually prints environment variables every 10 seconds.
- **Environment Variables**:
  - `NODE_NAME`: Set to the node name where the pod is running.
  - `POD_NAME`: Set to the name of the pod.
  - `POD_IP`: Set to the IP address of the pod.
  - `POD_SERVICE_ACCOUNT`: Set to the service account name assigned to the pod.
- **Restart Policy**: Set to `Never` to ensure the pod does not restart automatically.

This setup ensures that the pod continuously prints the environment variables, making it easy to verify their values.
