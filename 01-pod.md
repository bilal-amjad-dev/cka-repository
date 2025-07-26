Kubernetes doesn't deploy containers directly on the worker node.


To deploy a docker container by creating a POD.

```bash
kubectl run nginx --image nginx
```
To get the list of pods

```bash
kubectl get pods
```
