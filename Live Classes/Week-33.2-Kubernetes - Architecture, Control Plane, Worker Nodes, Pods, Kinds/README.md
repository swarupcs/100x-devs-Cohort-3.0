# Kubernetes - Architecture, Control Plane, Worker Nodes, Pods, Kinds

## 1. Kubernetes Cluster
A **Kubernetes Cluster** is a set of node machines for running containerized applications. If you're running Kubernetes, you're running a cluster. At a minimum, a cluster contains a control plane and one or more compute machines, or worker nodes.

## 2. Master Nodes vs Worker Nodes
- **Master Node (Control Plane):** The master node is responsible for managing the state of the cluster. It makes global decisions about the cluster (e.g., scheduling), and detects/responds to cluster events (e.g., starting up a new pod when a deployment's replicas field is unsatisfied).
- **Worker Node:** Worker nodes are the machines (VMs, physical servers, etc.) where the actual workloads (containers/pods) run. Every cluster has at least one worker node.

## 3. Architecture of Master and Worker Nodes

### The Control Plane (Master Node) Components
- **kube-apiserver:** The front end of the Kubernetes control plane. It exposes the Kubernetes API. All communication between components goes through the API server.
- **etcd:** A consistent and highly-available key-value store used as Kubernetes' backing store for all cluster data.
- **kube-scheduler:** Watches for newly created Pods with no assigned node, and selects a node for them to run on based on resource requirements, hardware constraints, affinity specifications, etc.
- **kube-controller-manager:** Runs controller processes. Controllers include the Node controller (notices when nodes go down), Job controller, Endpoints controller, and Service Account & Token controllers.

### Worker Node Components
- **kubelet:** An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod by communicating with the API server.
- **kube-proxy:** A network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept. It maintains network rules on nodes allowing network communication to your Pods from network sessions inside or outside of your cluster.
- **Container Runtime:** The software that is responsible for running containers (e.g., containerd, CRI-O).

## 4. Imperative vs Declarative Commands
- **Imperative Commands:** You tell Kubernetes *how* to do something. You run specific commands to change the state immediately (e.g., `kubectl run nginx --image=nginx`). Useful for quick tests and debugging.
- **Declarative Commands (Desired State):** You tell Kubernetes *what* you want (the desired state), usually via a YAML/JSON manifest file, and Kubernetes figures out how to achieve it (e.g., `kubectl apply -f manifest.yaml`). This is the recommended approach for production because manifests can be version-controlled (Infrastructure as Code).

## 5. Pods
A **Pod** is the smallest and simplest Kubernetes object. A Pod represents a set of running containers on your cluster. 
- A Pod typically encapsulates one application container, but in some cases, it can encapsulate multiple tightly coupled containers that share resources (like a shared network namespace and storage volumes).
- Pods are ephemeral; they can be created, destroyed, and replaced, but they are not self-healing by themselves (which is why we use higher-level controllers like Deployments or ReplicaSets).

## 6. Important Commands

```bash
# Imperatively run a pod
kubectl run nginx --image=nginx --port=80

# List all pods in the current namespace (short form: k get pods)
kubectl get pods

# Watch pods continuously for status changes
kubectl get pods -w

# List all nodes in the cluster
kubectl get nodes

# Delete a pod imperatively
kubectl delete pod nginx

# Get detailed information and events about a specific pod
kubectl describe pod nginx

# View logs of a specific pod
kubectl logs <pod-name>

# Apply a declarative manifest file
kubectl apply -f nginx.yaml
```
*(Note: `k` is a common alias for `kubectl`, which can be set via `alias k=kubectl`)*

## 7. Manifest Configuration (Kinds)

In Kubernetes, you define resources using YAML manifest files. Every manifest file has 4 required root fields:
1. `apiVersion`: Which version of the Kubernetes API you're using to create this object.
2. `kind`: What kind of object you want to create (e.g., Pod, Deployment, Service, ConfigMap).
3. `metadata`: Data that helps uniquely identify the object, including a `name` string, `UID`, and optional `namespace` and `labels`.
4. `spec`: The desired state for the object (e.g., containers, images, ports, replicas).

**Example: `manifest.yml` for a basic Pod**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
```

**Creating/Applying and Managing the manifest:**
```bash
# Create or update resources defined in a manifest
kubectl apply -f manifest.yml

# Viewing the created pod
kubectl get pods

# Deleting the pod by name
kubectl delete pod nginx

# Deleting the pod by passing the manifest file
kubectl delete -f manifest.yml
```
