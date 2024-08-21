## **Problem Statement:**

There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.

Create a Deployment named as *ic-deploy-xfusion*.

- Configure spec as replicas should be 1, labels app should be *ic-xfusion*, template's metadata lables app should be the same *ic-xfusion*.

- The initContainers should be named as *ic-msg-xfusion*, use image *fedora*, preferably with latest tag and use command *'/bin/bash'*, *'-c'* and *'echo Init Done - Welcome to xFusionCorp Industries > /ic/news'*. The volume mount should be named as *ic-volume-xfusion* and mount path should be */ic*.

- Main container should be named as *ic-main-xfusion*, use image *fedora*, preferably with latest tag and use command *'/bin/bash', '-c' and 'while true; do cat /ic/news; sleep 5; done'.* The volume mount should be named as *ic-volume-xfusion* and mount path should be */ic*.

- Volume to be named as *ic-volume-xfusion* and it should be an *emptyDir* type.

Here's how to create the Kubernetes Deployment named `ic-deploy-xfusion` with the specified requirements using init containers and main containers:

## **Solution**

### **Deployment YAML Configuration**

The YAML file below configures a Kubernetes Deployment with the required init containers, main containers, and volumes:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-xfusion
  labels:
    app: ic-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-xfusion
  template:
    metadata:
      labels:
        app: ic-xfusion
    spec:
      initContainers:
      - name: ic-msg-xfusion
        image: fedora:latest
        command: ["/bin/bash"]
        args: ["-c", "echo Init Done - Welcome to xFusionCorp Industries > /ic/news"]
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic

      containers:
      - name: ic-main-xfusion
        image: fedora:latest
        command: ["/bin/bash"]
        args: ["-c", "while true; do cat /ic/news; sleep 5; done"]
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic

      volumes:
      - name: ic-volume-xfusion
        emptyDir: {}
```

### **Explanation:**

1. **Deployment Metadata:**
   - **`name`**: The deployment is named `ic-deploy-xfusion`.
   - **`labels`**: Labels are set to `app: ic-xfusion`.

2. **Deployment Specification:**
   - **`replicas`**: Specifies that only one replica should be created.
   - **`selector`**: Match labels ensure the pods managed by this deployment have the `app: ic-xfusion` label.

3. **Pod Template Metadata:**
   - **`labels`**: Same label `app: ic-xfusion` is used for pod templates to match the deployment selector.

4. **Init Containers:**
   - **`name`**: The init container is named `ic-msg-xfusion`.
   - **`image`**: Uses `fedora:latest`.
   - **`command`**: Runs `/bin/bash` with the specified `-c` argument to echo a message to `/ic/news`.
   - **`volumeMounts`**: Mounts the volume `ic-volume-xfusion` to `/ic`.

5. **Main Container:**
   - **`name`**: The main container is named `ic-main-xfusion`.
   - **`image`**: Uses `fedora:latest`.
   - **`command`**: Runs `/bin/bash` with the specified `-c` argument to continuously read and display the contents of `/ic/news`.
   - **`volumeMounts`**: Mounts the volume `ic-volume-xfusion` to `/ic`.

6. **Volumes:**
   - **`name`**: Volume is named `ic-volume-xfusion`.
   - **`emptyDir`**: Defines an empty directory volume that persists only for the lifetime of the pod.

### **Deployment**

To apply this configuration to your Kubernetes cluster, save the YAML to a file (e.g., `ic-deploy-xfusion.yaml`) and then use `kubectl` to apply it:

```bash
kubectl apply -f ic-deploy-xfusion.yaml
```

### **Verification**

1. **Check Deployment:**

    ```bash
    kubectl get deployments
    ```

2. **Check Pods:**

    ```bash
    kubectl get pods
    ```

3. **Check Logs:**

   To ensure the init container's message is properly written, you can check the logs of the main container:

    ```bash
    kubectl logs <pod-name>
    ```

   Replace `<pod-name>` with the actual name of the pod from the `kubectl get pods` command.

By following these steps, you should have a deployment configured with init containers and main containers that will perform the required setup and continuously display the message.
