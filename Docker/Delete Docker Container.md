### Problem Statement

#### *A container named kke-container was created by one of the Nautilus project developers on App Server 1. It was solely for testing purposes and now requires deletion. Execute the following task: Delete the kke-container on App Server 1 in Stratos DC.*

### Solution

#### *List the container*

```bash
docker container ls
```

#### *Stop the Container if its in running state*

```bash
docker container stop kke-container
```

#### *Deleting the container*

```bash
docker container rm kke-container
```
