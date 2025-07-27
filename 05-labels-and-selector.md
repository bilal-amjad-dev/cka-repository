


`pod.yaml`
```bash
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  labels:
    app: my-nginx-app
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```


`service.yaml`
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-nginx-service
spec:
  selector:
    app: my-nginx-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```
