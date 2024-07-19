#### One of the DevOps team member was trying to install a WordPress website on a LAMP stack which is essentially deployed on Kubernetes cluster. It was working well and we could see the installation page a few hours ago. However something is messed up with the stack now due to a website went down. Please look into the issue and fix it

#### FYI, deployment name is lamp-wp and its using a service named lamp-service. The Apache is using http default port and nodeport is 30008. From the application logs it has been identified that application is facing some issues while connecting to the database in addition to other issues. Additionally, there are some environment variables associated with the pods like MYSQL_ROOT_PASSWORD, MYSQL_DATABASE,  MYSQL_USER, MYSQL_PASSWORD, MYSQL_HOST

#### Also do not try to delete/modify any other existing components like deployment name, service name, types, labels etc

kubectl get deployments.apps
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
lamp-wp   1/1     1            1           14m

thor@jumphost ~$ kubectl get svc
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        59m
lamp-service    NodePort    10.96.14.0     <none>        8080:30008/TCP   15m
mysql-service   ClusterIP   10.96.89.233   <none>        3306/TCP       15m

kubectl edit svc

kubectl get svc
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        59m
lamp-service    NodePort    10.96.14.0     <none>        80:30008/TCP   15m
mysql-service   ClusterIP   10.96.89.233   <none>        3306/TCP       15m

kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
lamp-wp-56c7c454fc-556v2   2/2     Running   0          16m

kubectl logs lamp-wp-56c7c454fc-556v2
