## Problem Statement

 An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.18 with the latest updates

 Execute a rolling update for this application, integrating the *nginx:1.18* image. The deployment is named *nginx-deployment*

 Ensure all pods are operational post-update

To execute a rolling update for the `nginx-deployment` and integrate the new `nginx:1.18` image, follow these steps:

## **Solution**

### **1. Check the Current State**

Verify the current state of the deployment and pods to understand what is running before making changes.

**Check the pods:**

```bash
kubectl get pods
```

You should see output similar to:

```
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-989f57c54-dnxjs   1/1     Running   0          53s
nginx-deployment-989f57c54-fjvdw   1/1     Running   0          53s
nginx-deployment-989f57c54-zsfs6   1/1     Running   0          53s
```

**Check the deployment:**

```bash
kubectl get deployments.apps
```

You should see output similar to:

```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           61s
```

### **2. Verify Current Deployment Details**

Ensure you know the current image being used and the update strategy:

```bash
kubectl describe deployments.apps nginx-deployment
```

Look for the image version and strategy type in the output. You might see something like:

```
Containers:
  nginx-container:
    Image:         nginx:1.16
```

### **3. Update the Image**

Update the image from `nginx:1.16` to `nginx:1.18` using the `kubectl set image` command:

```bash
kubectl set image deployments/nginx-deployment nginx-container=nginx:1.18
```

The output will confirm the image update:

```
deployment.apps/nginx-deployment image updated
```

### **4. Monitor the Rollout Status**

Ensure that the rolling update proceeds smoothly and completes successfully:

```bash
kubectl rollout status deployments/nginx-deployment
```

You should see:

```
deployment "nginx-deployment" successfully rolled out
```

### **5. Verify the Update**

Finally, check the deployment to confirm that the new image `nginx:1.18` is being used:

```bash
kubectl describe deployments.apps nginx-deployment
```

You should see the updated image in the output:

```
Containers:
  nginx-container:
    Image:         nginx:1.18
```

### **Summary**

- **Initial Check**: Verify the current pods and deployment status.
- **Verify Current Details**: Check the existing image and deployment strategy.
- **Update Image**: Use `kubectl set image` to update to the new image version.
- **Monitor Rollout**: Ensure the rollout status indicates a successful update.
- **Final Verification**: Confirm the new image is deployed correctly.

By following these steps, you'll successfully perform a rolling update of your `nginx-deployment` to use the updated `nginx:1.18` image.
