### Rolling updates 

- Normally when we create a deployment, it creates a ReplicaSet and the ReplicaSet creates the pods. And it will also add Revision History to the deployment.
- When we update the deployment, it will create a new ReplicaSet and the new ReplicaSet will create the new pods with the updated image. And the Revision History will be updated with the new revision.
- Every time we update the deployment, it will create a new ReplicaSet and the new ReplicaSet will create the new pods with the updated image. And the old ReplicaSet will delete the old pods.

- Rolling Updates in Kubernetes are a deployment strategy that allows you to update applications without downtime by incrementally replacing old pods with new ones.

- Ensures **zero downtime** for users during application upgrades.


### Deployment Strategy:

- **Recreate** - It will kill all the old pods and create new pods with the updated image. There will be a Downtime in this strategy.
- **RollingUpdate** - It will create new pods with the updated image and then kill the old pods one by one. This is the default strategy.

### Rollout Commands:

- To check the rollout status, we can use the following command:
```bash
kubectl rollout status deployment <deployment-name>
```

- To check the rollout history, we can use the following command:
```bash
kubectl rollout history deployment <deployment-name>
```

- To rollback to the previous version, we can use the following command:
```bash
kubectl rollout undo deployment <deployment-name>
```

---


### Lab

`vi nginx-deployment.yaml`

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21.1
        ports:
        - containerPort: 80
```
- Here you can see the image is `nginx:1.21.1`

```bash
kubectl apply -f nginx-deployment.yaml
```

- **Verify pods are running:**
```bash
kubectl get pods -l app=nginx
```


- **Start a rolling update by changing the nginx image version from `1.21.1` to `1.22.0`:**
```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.22.0
```

- **Check the rollout status to monitor progress:**
```bash
kubectl rollout status deployment/nginx-deployment
```

It means:
- Kubernetes finished updating your deployment called nginx-deployment.
- All the new pods you requested (for example, with a new image) are now running and healthy.
- The old pods (from the previous version) have been completely replaced.

`cat nginx-deployment.yaml`
- Here you can see the image has changed the nginx image version from `1.21.1` to `1.22.0`

- **Watch pods update and replace old ones:**
```bash
kubectl get pods -l app=nginx
```

- **Verify Rollout History**
```bash
kubectl rollout history deployment/nginx-deployment
```

--- 


- **Rollback to Previous Version (if needed)**
```bash
kubectl rollout undo deployment/nginx-deployment
```

- **Check pods to confirm they reverted:**
```bash
kubectl get pods -l app=nginx
```






Date: 30-July-2025
