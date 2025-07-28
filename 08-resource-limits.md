
Your pod:

- **Has 1Gi reserved** (via requests)—it’s guaranteed this much memory.
- **Can use more than 1Gi** if it needs and the node has extra memory.
- **Cannot use more than 2Gi** because you set a limit. If it tries to use 3Gi, Kubernetes will kill the pod (not let it use 3Gi).


Same Logic for CPU

Your YAML also has:

- requests: cpu: "1": Guarantees 1 CPU core.
- limits: cpu: "2": Caps usage at 2 CPU cores.
- If the pod tries to use more than 2 CPUs (e.g., 3 CPUs), it won’t be killed but will be throttled (slowed down) to stay within 2 CPUs.

```bash
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
 containers:
 - name: simple-webapp-color
   image: simple-webapp-color
   ports:
    - containerPort:  8080
   resources:
     requests:
      memory: "1Gi"
      cpu: "1"
     limits:
       memory: "2Gi"
       cpu: "2"
```

Date: 28-July-2025
