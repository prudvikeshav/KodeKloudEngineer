## Problem Statement:

The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.

- Create a pod named *print-envars-greeting*.

- Configure spec as, the container name should be *print-env-container* and use *bash* image.

- Create three environment variables:

  - a. GREETING and its value should be *Welcome to*

  - b. COMPANY and its value should be *xFusionCorp*

  - c. GROUP and its value should be *Industries*

- Use command *["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']* (please use this exact command), also set its restartPolicy policy to *Never* to avoid crash loop back.

- You can check the output using kubectl logs -f print-envars-greeting command.
## Solution:
The Pod File with the  above requirements:
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    name: print-envars-greeting
spec:
  containers:
  - image: bash
    name: print-envars-greeting
    env:
    - name: GREETING
      value: Welcome to
    - name: COMPANY
      value: xFusionCorp
    - name: GROUP
      value: Industries
    command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
