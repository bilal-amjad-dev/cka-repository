
So far in this course we have created Objects such as PODs, Deployments and Services in our cluster. Whatever we have been doing we have been doing in a NAMESPACE.

- This namespace is the default namespace in kubernetes. It is automatically created when kubernetes is setup initially.


- To view pods in all namespaces
```bash
kubectl get pods --all-namespaces
```


- By default, we will be in a default namespace. To switch to a particular namespace permenently run the below command.
```bash
kubectl config set-context $(kubectl config current-context) --namespace=dev
```

- Another way to create a namespace

```bash
kubectl create namespace dev
```


- If you want to make sure that this pod gets you created in the dev env all the time, even if you don't specify in the command line, you can move the --namespace definition into the pod-definition file.
```bash
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  namespace: dev
  labels:
     app: myapp
     type: front-end
spec:
  containers:
  - name: nginx-container
    image: nginx
```


- To limit resources in a namespace, create a resource quota.
```bash
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```

```bash
$ kubectl create -f compute-quota.yaml
```


