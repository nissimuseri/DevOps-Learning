# Kubernetes

** Need to validate all the contects on this page is correct **

### K8s is an orchestrator and Cluster Manager of containers.

## K8S includes:
* Orchestrator
* Cluster manager
* Abstraction layer
* IaC

There is an abstraction for the deployment when you are using K8s with AKS/EKS/Minikube or any other services.
 
## Kubernetes Architecture:
### Node: 
* Can be Master or Node
* For each node, we need to run a runtime(docker or container-d on new versions)
* On each Master, we are running an API Service(which was developed on Go).
	* The API Server managed who is the “Chief”
	* It should be Odd numbers of Masters, for choosing “Chief” when the original was dropped. Usual is 1/3/5 Masters
	* All data is stored in ETCD’s key-value store.
	* All the connection between the Masters is on Rest API.
	* The services don’t need to be on the same node.
* Each Master contains ETCD for managing the Key-Values.
	* The connection with the ETCD is Read/Write.
	* 

* Each Master contains Scheduler
* Responsible on schedule API calls between the nodes
* Decide which node will run the API call that got from the API Service.
* Each Master contains Controller
	* Responsible for control loops, such as adding more pods according to RS.
* Each Node is managed by Kubelet - the agent the manages the specific Node and connects with the Master.
* Every node’s networking is being managed by the Kube-Proxy service.
	* It is like an IP route table for the load balancing on a distributed system


External/Internal API Call -> received on API Service -> save values on ETCD -> create a Control Loop with the Controller -> the Scheduler decides which node will treat on this API call.

A control loop/remediation loop:
This loop is the basic of K8s.
It observes the desired state and the current state and calculates the diff - and makes it compared for getting the desired.
The truth is at ETCD.

We managing the K8s with YAML files.
Kubectl translates the YAML to JSON for using REST.
Basic Yaml files of K8s contain apiVersion, kind, metadata, spec.


The deployment contains Replica-Set of Pods.

### POD:
* Contains one or more containers
* Run on a single host within the same namespace
* The pod can be replicated
* All the containers on the same pod have the same IP
* The connection between the containers are with port
* The pod is temporary - we must think the POD can be terminated and survive restarts
* In basic POD is a piece of configuration that can be transferred between nodes.
* A POD can be managed as:
	* Deployment
	* ReplicaSet
	* StatefullSet
	* DeamonSet
	* Jobs and Cron-Jobs


* init container - pattern for configuring the pod before the pod is up.
* side-card - container for monitor or doing something during other containers running on the same pod(like Istio).

### Service:
Define unify access to a group of POD instances.

* Can use selectors.
* Can be route external to k8s cluster or namespace.
* Service types:
	* ClusterIP - Internal IP of K8S, for connection between pods.
	* NodePort - For accessing the pods from external by port.
	* Load-Balancer(needs cloud controller) - Use cloud LB and route it internally to the pods on the cluster.q

### Health Check vs Ready Check:
Ready = Health and can get more data.


### ConfigMap vs Secrets:
Secrets encrypt the data by Base64
Config Map and Secrets are injecting the data into the ETCD, it can be changed just with a REST API call.

### Ingress:
Layer 7 routing.
You can mention multiple paths, and move according to the path to multiple backends.

## Storage Management:
Storage Class creates for us a PV to serve the PVC.

### Storage Class
Define a volume from External Service(AWS, GCP, Linux file system, etc.)

### Persistence Volume(PV)
A configuration file for defining static volume.

### Persistence Volume Claim(PVC)
A configuration file to ask amount of volume.

### Persistence Volume Claim Template(PVCT)
A configuration file to create dynamic PVC when we use dynamic storage(like Cloud data services)

### HPA - horizontal pod autoscaling:
Wrapped the RS to monitor and take statistics to change the replica count according to the needs.

## Kubernetes Authorization

Authorization is the way to give access to a resource.

### RBAC vs ABAC:

#### ABAC:
When you create an attribute, you declare which access he will get.
When this attribute wants to access some resource, the resource will give him access just if he is on the list of the allowed attributes.

#### RBAC:
The allow is based on role, you can attach/detach a role to a user.

Kubernetes is used RBAC.

### Subjects:
- Service Account
- Users

### Service Account:
- Managed by Kubernetes
- Namespace
- Token as a secret
- Can be attached to pods
- Username = metadata.name

### Token:
* Used for Service Account
* String to compare with encryption/decryption for authentication.
* After authentication, we got access to the resource.

### Certification:
* External CA approves the authentication of the user to trust him.
* Includes: username, UID, groups.
