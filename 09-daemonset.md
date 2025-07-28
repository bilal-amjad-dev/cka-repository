
**What is a DaemonSet?**

A **DaemonSet** is a Kubernetes controller that guarantees that a copy of a specific Pod runs on **every node** in your cluster.

Think of it as a **"one Pod per node"** rule. A DaemonSet's purpose is to ensure that a cluster-wide service is always running on every single host.


**Why and When to Use It?**

DaemonSets are essential for applications that need to perform a system-level task on all nodes. They are used for services that should not be run in a random location but must be present on every machine.

Common use cases include:

- **Logging Agents:** Running a log collector (like Fluentd or Logstash) on every node to gather logs from all containers and send them to a central server.

- **Monitoring Agents:** Running a monitoring agent (like Prometheus Node Exporter) on every node to collect system metrics (CPU, memory, disk) and report them.

- **Cluster Storage Daemons:** Running a distributed storage agent (like Ceph) on every node to provide storage to the cluster.

- **Security Agents:** Running a security or network policy agent on every node to enforce cluster-wide rules.


If a Pod is for a regular **application** (like a web server), you should use a **Deployment** because it manages a ReplicaSet to maintain the Pod count.

Now, to answer your question: **What kind of Pod does a DaemonSet run?**

A DaemonSet runs a Pod that is not an application for end-users, but rather a **system-level** or **infrastructure-level** Pod. Its job is to provide a service for the cluster itself.

A DaemonSet runs Pods that are:

**Monitoring Pods:** To check the health, CPU, and memory of every node.

**Logging Pods:** To collect logs from all containers on a node and send them to a central location.

**Security Pods:** To enforce security policies on every host.

These Pods exist to support the cluster's operation, not to run an end-user application.

**To put it simply:**

A **Deployment** runs an **Application** for end-users. (e.g., a website)

A **DaemonSet** runs a **Utility** for the cluster's infrastructure. (e.g., a security camera in every room)

---


**DaemonSets vs. Other Controllers**

| Controller   | What It Does                              | Example Use Case                        |
|--------------|-------------------------------------------|-----------------------------------------|
| **ReplicaSet** | Ensures a **specific number** of Pods are running. | Running 3 copies of a web server        |
| **Deployment**  | Manages ReplicaSets for rolling updates.   | Updating a web server to a new version   |
| **DaemonSet**    | Ensures **one Pod on every node**.             | Running a log collector on every machine |


```bash
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: prometheus-node-exporter
  labels:
    app: node-exporter
spec:
  selector:
    matchLabels:
      app: node-exporter
  template:
    metadata:
      labels:
        app: node-exporter
    spec:
      containers:
      - name: node-exporter
        image: prom/node-exporter:v1.3.1 
```




- Run the command kubectl get daemonsets --all-namespaces

```bash
kubectl get daemonsets --all-namespaces
```

- Run the command kubectl get all --all-namespaces and identify the types

```bash
kubectl get all --all-namespaces
```


- Run the command kubectl describe daemonset kube-proxy --namespace=kube-system

```bash
kubectl describe daemonset kube-proxy --namespace=kube-system
```


### Question: 

If you have 50 nodes in your cluster, a DaemonSet will create a Pod on every one of those **50 nodes**.

The DaemonSet controller's job is to ensure this "one Pod per node" rule is always maintained. If you later add a 51st node, the DaemonSet will automatically create a new Pod on it. If you remove a node, it will automatically remove the Pod that was running on it.

Date: 28-July-2025
