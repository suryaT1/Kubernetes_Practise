#  Kubernetes Kubectl Cheat Sheet

Quick reference for commonly used `kubectl` commands.  
Based on the official [Kubernetes Docs](https://kubernetes.io/docs/reference/kubectl/quick-reference/).

## Cluster & Context Management
- `kubectl get nodes` → list all current nodes (worker + control plane)  
- `kubectl config` → for moving different clusters, namespaces and different logins stored in your kubeconfig.  
  - `kubectl config get-contexts` → List all saved contexts (Cluster)  
  - `kubectl config use-context prod` → Switch to prod cluster/user/namespace  
  - `kubectl config current-context` → Show current context  
  - `kubectl config set-context --current --namespace=dev` → Change default namespace  

## Explain & YAML
- `kubectl explain` → built-in cheat sheet for writing YAML manifests correctly  
  - Example: `kubectl explain pod.spec.containers`  
- `kubectl create -f pod.yml` → Create a new resource from the YAML file (first-time deployment)  
- `kubectl apply -f pod.yml` → Create or update a resource from the YAML file (declarative way, GitOps)  

## Inspect & Edit
- `kubectl describe pod <podname>` → Validate logs / detailed info  
- `kubectl edit <resource_type> <resource_name>` → Open and modify a live Kubernetes resource in default editor (vi/nano)  

## Pods & Exec
- `kubectl exec -it <pod name> -- sh` → Login into pod  
- `kubectl get pods nginx --show-labels` → Show pod labels  
- `kubectl get pods -o wide` → Wide info (node, IP, etc.)  
- `kubectl get po` → Shortcut for pods  
- `kubectl get ep` → Fetch the endpoints IPs of services  

## Dry Run
- `--dry-run=client` → Won’t apply, only show plan if executed  
- `kubectl run nginx --image=nginx --dry-run=client -o yaml` → Show YAML output without creating resource  

## Replication Controller | ReplicaSet | Deployment
- `kubectl get rc` → List replication controllers  
- `kubectl delete rc/nginx-rc` → Delete replication controller named nginx-rc  
- `kubectl scale rs/nginx-rs --replicas=10` → Scale replicaset to 10  
- `kubectl scale --replicas=5 -f foo.yml` → Scale using YAML file  
- `kubectl scale --help` → List all use cases for scale  
- `kubectl get all` → List everything running in cluster  
- `kubectl set image <resource>/<name> <container>=<new image>`  
  - Example: `kubectl set image deployment/my-deployment my-container=nginx:1.21`  
- `kubectl rollout history deploy/nginx-deploy` → Show rollout history  
- `kubectl rollout undo deploy/nginx-deploy` → Rollback to old image  

### Key Notes
1. Deleting `rs/rc` also deletes pods automatically  
2. Use `--cascade=orphan` to delete rs/rc but keep pods  

### Best Practices
1. **ReplicationController** → Old, only manages pods created by it  
2. **ReplicaSet** → Latest, manages pods by labels  
3. **Deployment** → Best practice (includes RS, rollback, scaling, versioning)  

## Kubernetes Services
Service is an abstraction that defines a stable way to access a group of Pods.  

- **ClusterIP (default):** Exposes service on a cluster-internal IP.  
  Example: Microservices talking to each other.  

- **NodePort:** Exposes service on each Node’s IP at a static port.  
  Example: Quick testing without cloud LB.  

- **LoadBalancer:** Provisions external LB (AWS, Azure, GCP).  
  Example: Public-facing apps or APIs.  

- **ExternalName:** Maps service to external DNS name.  
  Example: Redirect to external DB like `mydb.example.com`.  

---

