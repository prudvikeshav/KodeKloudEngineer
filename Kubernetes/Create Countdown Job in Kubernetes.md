
## Problem Statement

 The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below

- Create a job named *countdown-devops*

- The spec template should be named *countdown-devops* (under metadata), and the container should be named *container-countdown-devops*

- Utilize image *debian* with latest tag (ensure to specify as debian:latest), and set the restart policy to Never

- Execute the command *sleep 5*

## Solution

To create a Kubernetes Job based on the provided requirements, follow these steps:

### 1. Create the Job Configuration

Save the following YAML configuration to a file named `countdown-job.yaml`:

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

### 2. Apply the Job Configuration

Apply the Job configuration to your Kubernetes cluster using the following command:

```bash
kubectl apply -f countdown-job.yaml
```

This command will create the Job in your cluster as defined in the `countdown-job.yaml` file.

### 3. Verify the Job Creation

To verify that the Job was created and is running as expected, use the following command:

```bash
kubectl get jobs.batch
```

Expected Output:

```
NAME               COMPLETIONS   DURATION   AGE
countdown-devops   0/1           9s         9s
```

### 4. Check Job Details and Logs

To get detailed information about the Job, including status and events, use:

```bash
kubectl describe job countdown-devops
```

To view logs for the Job's pod (helpful for debugging), first, get the pod name:

```bash
kubectl get pods --selector=job-name=countdown-devops
```
