
## Problem Statement:

 The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below

-  Create a job named *countdown-devops*

-  The spec template should be named *countdown-devops* (under metadata), and the container should be named *container-countdown-devops*

-  Utilize image *debian* with latest tag (ensure to specify as debian:latest), and set the restart policy to Never

-  Execute the command *sleep 5*

## Solution:

 Create a job with the given details.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-devops
spec:
  template:
    metadata:
      name: countdown-devops
    spec:
      restartPolicy: Never
      containers:
      - name: container-countdown-devops
        image: debian:latest
        command:
        - bash
        - -c
        - sleep 5
```

 To view the created jobs.

```bash
kubectl get jobs.batch
```

```
NAME               COMPLETIONS   DURATION   AGE
countdown-devops   0/1           9s         9s
```

 Build the docker image with Docker file.

```bash
docker build -t apache:test .
```
