

- Many services need configuration files.
- ConfigMap is used to store data and this data can be at later point of time used by your application or your pod or your deployment.
- Secrets solve the same problem but secrets deal with sensitive data. 
- Kubernetes says if you have non-sensitive data then store it in ConfigMap.
- If you have sensitive data then store it in secrets.


### Lab:
`vi test-cm.yaml`
```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-cm
data:
  db-port: "3306"
```

```bash
kubectl apply -f test-cm.yaml
```

`vi test-deployment.yaml`
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: sample-python-app
  name: sample-python-app-deployment
spec:
  replicas: 1 # Assuming a single replica
  selector:
    matchLabels:
      app: sample-python-app
  template:
    metadata:
      labels:
        app: sample-python-app
    spec:
      containers:
      - name: python-app
        image: abhishekf5/python-sample-app-demo:v1
        volumeMounts:
        - name: db-connection
          mountPath: /opt
        ports:
        - containerPort: 8000
      volumes:
      - name: db-connection
        configMap:
          name: test-cm # The name of the ConfigMap is assumed to be 'test-cm'
```


- To see pods
```bash
kubectl get pods
```

- To go inside the pod
```bash
kubectl exec -it PODNAME -- /bin/bash
```

```bash
cat /opt/db-port | more
```

```bash
exit
```

---
### Secrets

```bash
kubectl create secret generic test-secret --from-literal=db-port="3306"
```
```bash
kubectl describe secret test-secret 
```
```bash
kubectl edit secret test-secret 
```


Date: 2-August-2025
