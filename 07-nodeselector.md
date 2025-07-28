
- In node selector, our pod is created in our mentioned node.
- In the pod’s YAML definition, add a nodeSelector field specifying the same key-value pair.

Using `nodeSelector` is a two-step process:

Step 1: Label a Node
First, you need to add a label to one or more of your cluster nodes. You do this with a kubectl command. Let's say you have a node named `my-gpu-node` that has a GPU, and you want to label it accordingly.

```bash
kubectl label node my-gpu-node gpu=true
```


Step 2: Add the `nodeSelector` to the Pod

`pod-with-nodeselector.yaml`

```bash
apiVersion: v1
kind: Pod
metadata:
  name: my-gpu-pod
spec:
  containers:
  - name: cuda-container
    image: nvidia/cuda:11.4.0-base
    # This Pod requires a GPU
  nodeSelector:
    gpu: "true"
```


Summary
In short, `nodeSelector` is the simplest way to constrain Pods to specific nodes by following this clear pattern:

- Label your nodes with descriptive key-value pairs.
- Select those labels in your Pod's `nodeSelector` field.


Date: 28-July-2025
