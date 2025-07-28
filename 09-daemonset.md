
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
  name: monitoring-daemon
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    metadata:
     labels:
       app: monitoring-agent
    spec:
      containers:
      - name: monitoring-agent
        image: monitoring-agent
```

Date: 28-July-2025
