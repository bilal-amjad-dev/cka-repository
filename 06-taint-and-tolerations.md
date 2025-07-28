Taint and tolerations are only meant to restrict nodes from accepting certain pods

Syntax:
```bash
kubectl taint nodes <node-name> key=value:taint-effect
```

There are 3 taint effects
- NoSchedule
- PreferNoSchedule
- NoExecute


```bash
kubectl taint nodes node1 gpu=true:NoSchedule
```


- To see this taint, run the below command
```bash
kubectl describe node node1 | grep Taint
```
- If you want to remove the taint then add – at the end. 
```bash
Kubectl taint node node01 gpu=ture:NoSchedue-
```

`vi pod.yaml`

```bash
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: redis
  name: redis
spec:
  containers:
  - image: redis
    name: redis
  tolerations:
  - key: "gpu"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"

```

- `kubectl apply -f pod.yaml`
- To see on which node the pod has been created
```bash
kubectl get pods -o wide
```
  
  

Date: 27-July-2025
