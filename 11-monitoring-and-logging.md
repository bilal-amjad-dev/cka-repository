**Monitoring in K8S**

Normally there is no bulit-in monitoring in K8S. But there are many Open Source tools that can be used to monitor K8S. Some of them are:

- Metrics Server
- Prometheus
- Grafana
- Kibana
- ElasticSearch
- Datadog



**Installing Metrics Server**

- To enable Metrics Server in minikube, we can use the following command:

```bash
minikube addons enable metrics-server
```

- To install Metrics Server in a K8S cluster, we can use the following command:
Run this command to apply the latest metrics server manifest:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
This command downloads and applies the Metrics Server manifests, creating all required resources (deployment, service account, roles, etc.).


### 2. Check Metrics Server Pod Status

Check if the Metrics Server pod is running in the kube-system namespace:

```bash
kubectl get pods -n kube-system | grep metrics-server
```

**Possible initial states:**

- 0/1 Running or CrashLoopBackOff or Error → Usually means TLS or connectivity issues with kubelets.

- 1/1 Running → Good, metrics-server pod is healthy.





### 3. Fix TLS Issue by Adding `--kubelet-insecure-tls`
If the metrics-server Pod is stuck at 0/1 Running, especially in local or learning clusters that use self-signed certificates, do this:

- Edit the Metrics Server deployment:


```bash
kubectl -n kube-system edit deployment metrics-server
```


Find the section:

```bash
containers:
- args:
```

Add a new line under `args:`:
```bash
  - --kubelet-insecure-tls
```

For example:
```bash
spec:
  containers:
  - args:
    - --cert-dir=/tmp
    - --secure-port=10250
    - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
    - --kubelet-use-node-status-port
    - --metric-resolution=15s
    - --kubelet-insecure-tls        # <--- ADD THIS LINE
    image: registry.k8s.io/metrics-server/metrics-server:v0.7.1
```

3. **Save and exit the editor**. The Deployment will restart the pod automatically.
4. **Check status:**

```bash
kubectl get pods -n kube-system | grep metrics-server
```
After a minute, it should show 1/1 Running.

5. Test Metrics API:

```bash
kubectl top nodes
```
```bash
kubectl top pods --all-namespaces
```




Date: 30-July-2025
