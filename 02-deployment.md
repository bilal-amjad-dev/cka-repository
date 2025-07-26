Why Deployment if we have opportunity of pod?
- Rolling Updates and Rollbacks: Deployment allow you to update your application without downtime. You can also rollback to a previous deployment if something goes wrong.
- Replica Sets: Deployments create Replica Sets which are used to ensure that a specified number of pod replicas are running at any given time.
- Scaling: Deployments allow you to scale your application up or down.



Create a new pod with the nginx image.
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: nginx:latest
```
