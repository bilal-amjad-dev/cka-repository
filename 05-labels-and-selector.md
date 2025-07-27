


`pod.yaml`
```bash
 apiVersion: v1
 kind: Pod
 metadata:
  name: simple-webapp
  labels:
    app: App1
    function: Front-end
 spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
    - containerPort: 8080
```


`service.yaml`
```bash
  apiVersion: v1
  kind: Service
  metadata:
   name: my-service
  spec:
   selector:
     app: App1
   ports:
   - protocol: TCP
     port: 80
     targetPort: 9376
```
