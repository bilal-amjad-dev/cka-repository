- There are 2 phases involved in configuring ConfigMaps.
  - First, create the configMaps
  - Second, Inject then into the pod.


    - There are three methods to inject it into pod are:
      - `env` **with** `configMapKeyRef` (for single key injection)
      - `envFrom` **with** `configMapRef` (for all key injection)
      - `volumes` **with** `configMap` (for file system injection)

### Lab of KodeKloudhub:
**Step 1: Create ConfigMap**
`vi config-map.yaml`
```bash
apiVersion: v1
kind: ConfigMap
metadata:
 name: app-config
data:
 APP_COLOR: blue
 APP_MODE: prod
```
```bash
kubectl apply -f config-map.yaml
```

```bash
kubectl get cm
```

**Step 2: Inject then in the pod/deployment**
`vi my-pod.yaml`

```bash
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
spec:
 containers:
 - name: simple-webapp-color
   image: simple-webapp-color
   ports:
   - containerPort: 8080
   envFrom:
   - configMapRef:
       name: app-config
```
```bash
kubectl create -f my-pod.yaml
```













- Many services need configuration files.
- ConfigMap is used to store data and this data can be at later point of time used by your application or your pod or your deployment.
- Secrets solve the same problem but secrets deal with sensitive data. 
- Kubernetes says if you have non-sensitive data then store it in ConfigMap.
- If you have sensitive data then store it in secrets.


### Lab of Sir Abhishek Veeramalla:
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
- By doing this, our pods are not deleted. 

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

Source: Abhishek Veeramalla
KodeKloudhub
Date: 2-August-2025
