## Problem Statement

 An application deployed on the Kubernetes cluster requires an update with new features developed by the Nautilus application development team. The existing setup includes a deployment named nginx-deployment and a service named nginx-service. Below are the necessary changes to be implemented without deleting the deployment and service.

- Modify the service nodeport from *30008* to *32165*

- Change the replicas count from *1* to *5*

- Update the image from *nginx:1.19* to *nginx:latest*

Here’s a step-by-step guide to implementing the required updates to the `nginx-deployment` and `nginx-service`:

### 1. Update the Service NodePort

1. **Check the Current Service Configuration:**

   ```bash
   kubectl get service
   ```

   Output:

   ```
   NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
   kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        63m
   nginx-service   NodePort    10.96.54.165   <none>        80:30008/TCP   100s
   ```

2. **Update the NodePort:**

   Edit the `nginx-service` to change the NodePort from `30008` to `32165`.

   ```bash
   kubectl edit service nginx-service
   ```

   In the editor, locate and update the `nodePort` value:

   ```yaml
   spec:
     ports:
     - port: 80
       targetPort: 80
       nodePort: 32165
       protocol: TCP
   ```

3. **Verify the Update:**

   ```bash
   kubectl describe service nginx-service
   ```

   Output should reflect the new NodePort value:

   ```
   Name:                     nginx-service
   Namespace:                default
   Labels:                   <none>
   Annotations:              <none>
   Selector:                 app=nginx-app
   Type:                     NodePort
   IP Family Policy:         SingleStack
   IP Families:              IPv4
   IP:                       10.96.54.165
   IPs:                      10.96.54.165
   Port:                     <unset>  80/TCP
   TargetPort:               80/TCP
   NodePort:                 <unset>  32165/TCP
   Endpoints:                10.244.0.5:80
   Session Affinity:         None
   External Traffic Policy:  Cluster
   Events:                   <none>
   ```

### 2. Update the Deployment

1. **Check the Current Deployment Configuration:**

   ```bash
   kubectl get deployments.apps
   ```

   Output:

   ```
   NAME               READY   UP-TO-DATE   AVAILABLE   AGE
   nginx-deployment   1/1     1            1           4m37s
   ```

2. **Update the Image and Replicas:**

   Edit the `nginx-deployment` to update the image and replicas.

   ```bash
   kubectl edit deployment nginx-deployment
   ```

   In the editor, make the following updates:

   - **Change the image from `nginx:1.19` to `nginx:latest`:**

     ```yaml
     spec:
       containers:
       - name: nginx
         image: nginx:latest
     ```

   - **Increase the replicas from `1` to `5`:**

     ```yaml
     spec:
       replicas: 5
     ```

3. **Verify the Deployment Updates:**

   ```bash
   kubectl get deployments.apps
   ```

   Output should show the updated replicas:

   ```
   NAME               READY   UP-TO-DATE   AVAILABLE   AGE
   nginx-deployment   5/5     5            5           7m6s
   ```

### Final Verification

1. **Check the pods to ensure they are running:**

   ```bash
   kubectl get pods
   ```

2. **Confirm the new NodePort is in effect by accessing the service:**

   You can access the service using the new NodePort `32165` in your browser or via `curl`.

By following these steps, you ensure that the `nginx-deployment` and `nginx-service` are updated without deleting them.
