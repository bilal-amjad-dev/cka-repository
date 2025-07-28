- A Static Pod is a special kind of Pod. The main Kubernetes "brain" (the API server) doesn't know about it directly. Instead, the `kubelet` (the manager on one single computer) handles it all by itself. Instead, the kubelet watches each static Pod (and restarts it if it fails).
- Static Pods are always bound to one Kubelet on a specific node.
- So we have to create the static pod definition file on some specific location on the node and kubelet will take care of the rest.


Date: 28-July-2025
