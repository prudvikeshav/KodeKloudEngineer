**Problem Statement**

#### The Nautilus DevOps team is setting up recurring tasks on different schedules. Currently, they're developing scripts to be executed periodically. To kickstart the process, they're creating cron jobs in the Kubernetes cluster with placeholder commands. Follow the instructions below

- #### Create a cronjob named datacenter

- #### Set Its schedule to something like */10* ** *. You can set any schedule for now

- ####  Name the container cron-datacenter

- #### Utilize the httpd image with latest tag (specify as httpd:latest)

- #### Execute the dummy command echo Welcome to xfusioncorp

- #### Ensure the restart policy is OnFailure

**Solution**

#### TO create a cronjob

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: datacenter
spec:
  schedule: "*/10 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-datacenter
            image: httpd:latest
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - echo Welcome to xfusioncorp!
          restartPolicy: OnFailure
```

#### To check the cron job created

```bash
kubectl get cronjobs.batch
```

```
NAME         SCHEDULE       SUSPEND   ACTIVE   LAST SCHEDULE   AGE
datacenter   */10* ** *   False     0        23s             3m41s
```
