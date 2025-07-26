Kubernetes doesn't deploy containers directly on the worker node.


To deploy a docker container by creating a POD.

```bash
apiVersion: v1
kind: Pod
metadata:
  name: my-first-pod # A name for your pod
spec:
  containers:
  - name: my-container # A name for the container inside the pod
    image: nginx:latest # The Docker image to use (e.g., Nginx web server)
```
