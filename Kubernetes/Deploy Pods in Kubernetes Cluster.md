## Problem Statement

 The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:*

 Create a pod named **pod-httpd** using the **httpd** image with the latest tag. Ensure to specify the tag as **httpd:latest**.

 Set the app label to **httpd_app**, and name the container as **httpd-container**.

 Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

## Solution

 We need to  create Kubernetes pod named pod-httpd using the `httpd:latest` image. The pod should have the container named `httpd-container` and should be labeled with `httpd_app`.

Yaml Configuration:

```yaml
 Pod-httpd.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: httpd_app
  name: pod-httpd
spec:
  containers:
  - image: httpd:latest
    name: httpd_container
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

Apply the Pod Configuration:
Save the YAML File:

Save the configuration to a file named pod-httpd.yaml.
Apply the pod yaml file.

```bash
kubectl apply -f pod-httpd.yaml 
pod/pod-httpd created
```

### Verification

1.Check Pod Status:

- Verify that the pod is created and is running:

  ```bash
  kubectl get pods
  ```

- Look for pod-httpd in the output. Ensure its status is Running.

2.Describe the Pod:

- Get detailed information about the pod:

  ```bash
  kubectl describe pod pod-httpd
  ```

- This command provides detailed information about the pod’s status, events, and configuration.

3.Check Container Logs:

- View logs from the container to ensure it is running correctly:

  ```bash
  kubectl logs pod-httpd -c httpd-container
  ```

- This helps in troubleshooting any issues that might be occurring within the container.

### Conclusion

By following these steps, you will successfully create and verify a Kubernetes pod named pod-httpd with the specified configuration. The pod will run the httpd:latest image, with appropriate labels and container naming.
