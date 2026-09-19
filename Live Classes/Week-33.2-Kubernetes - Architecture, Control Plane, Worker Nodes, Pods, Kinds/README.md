1. Kubernetes Cluster
2. Master nodes vs Wroker nodes
3. Architecture of master and worker
4. Declarative commands / desired state commands
5. Pods

commands

kubectl run ngnix --image=ngnix --port=80
k get pods
kubectl get nodes
kubectl delete pod ngnix
kubectl apply -f nginx.yaml


k gets pods -w

k describe pod ngnix

k logs podname