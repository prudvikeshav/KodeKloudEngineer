## Problem Statement:

The Nautilus DevOps team is working on setting up some pre-requisites for an application that will send greetings to different users. There is a sample deployment that needs to be tested. Below is a scenario that needs to be configured on a Kubernetes cluster. Please find below more details about it.

- Create a pod named *print-envars-greeting*.
- Configure the spec such that the container name should be *print-env-container* and use the *bash* image.
- Create three environment variables:
  - a. GREETING with the value *Welcome to*
  - b. COMPANY with the value *xFusionCorp*
  - c. GROUP with the value *Industries*
- Use the command *["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']* (please use this exact command) and set its restart policy to *Never* to avoid crash loop back.
- You can check the output using the command `kubectl logs -f print-envars-greeting`.

## Solution:

To fulfill the requirements, we need to create a Kubernetes Pod specification file that meets all the given criteria. Below is the Pod file with the required configuration and a detailed explanation of each part.

### Pod File with the above requirements

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
  labels:
    name: print-envars-greeting
spec:
  containers:
  - image: bash
    name: print-env-container
    env:
    - name: GREETING
      value: Welcome to
    - name: COMPANY
      value: xFusionCorp
    - name: GROUP
      value: Industries
    command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
  restartPolicy: Never
```

### Verification:

After creating the Pod using the above YAML file, you can verify its output using the following command:

```bash
kubectl logs -f print-envars-greeting
```

This command retrieves the logs from the Pod, where you should see the output:

```
Welcome to xFusionCorp Industries
```

This confirms that the environment variables were correctly set, and the command executed as expected.

