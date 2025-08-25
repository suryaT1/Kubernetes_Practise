https://kubernetes.io/docs/reference/kubectl/quick-reference/   -> cheet sheet

#######################################

kubectl get nodes -> list all current nodes (worker + control plane) 
kubectl config -> for moving different clusters,namespaces and different logins stored in your kubeconfig.
   EXAMPLES: 
        kubectl config get-contexts        # List all saved contexts (Cluster)
		kubectl config use-context prod    # Switch to prod cluster/user/namespace
		kubectl config current-context     # Show current context
		kubectl config set-context --current --namespace=dev  # Change default namespace	
kubectl explain -> built-in cheat sheet for writing YAML manifests correctly under time pressur
  example: kubectl explain pod.spec.containers
kubectl create -f pod.yml -> Create a new resource from the YAML file
  Use case: First-time deployment of something
  
kubectl apply -f pod.yml -> Create or update a resource from the YAML file (declarative way).
  Use case: Continuous deployments, making config changes, GitOps.
  
kubectl describe pod <podname> -> to validate logs

kubectl edit ->open and modify a live Kubernetes resource directly from the command line 
               in your default text editor (usually vi or nano)
	syntax: kubectl edit <resource_type> <resource_name>
	
kubectl exec -it <pod name> --sh -> login into pod

--dry-run=client  -> it wont apply but will show plan wt if that cmnd if executes
kubectl run nginx --image=nginx --dry-run=client -o yaml -> it will shows output in a yaml
kubectl get pods nginx --show-labels  -> it will show the labels of pods
kubectl get pods -o wide -> wide information about pod
kubectl get po -> short cut of pods
kubectl get ep  -> to fetch the endpoints IP's of clusters


Replication Controller | Replicaset | Deployment

kubectl get rc  -> list the replication controllers in kubernetes cluster
kubectl delete rc/nginx-rc -> delete the replication controller names nginx-rc from current workspace.
kubectl scale rs/nginx-rs --replicas=10  -> to create a replication set using cmnds
kubectl scale --replicas=5 -f foo.yml
kubectl scale --help -> list all cmnds usecases with respect to scale
kubectl get all -> all things those are running on cluster
kubectl set image <resource>/<name> <container name>=<new image>
  example : kubectl set image deployment/my-deployment my-container=nginx:1.21
kubectl rollout history deploy/nginx-deploy -> to see all rollout changes
kubectl rollout undo deploy/nginx-deploy -> bring back old image

key notes:
   1. if user deletes rs/rc then those pods also will deleted automatically
   2. if you dont want to delete the pods use the --cascade=orphan this will create rs/rc not the pods
   
Best Practises:
   1. using replication controller is old it will only manage pods which are created by rc
   2. using replicaset is latest and it manages all the pods which are part of tag which is mentioned on configuration
   3. using deployment is best practise it has default rs so it has options to rollback ,changing versions of pod and so on.
   
   
Kubernetes Services:
   
Service is an abstraction that defines a stable way to access a group of Pods
   
ClusterIP (default): Exposes the Service on a cluster-internal IP.
    Example use: Microservices talking to each other.

NodePort: Exposes the Service on each Node’s IP at a static port.
    Example use: Testing services quickly without cloud load balancer.

LoadBalancer: Provisions an external load balancer (from cloud providers like AWS, Azure, GCP).
	Example use: Public-facing applications (web apps, APIs).

ExternalName:Maps a Service to an external DNS name (no selector or Pods involved).
	Example use: Redirecting to an external database like mydb.example.com.
   
   

