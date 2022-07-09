 # Kubernetes OS
---
Kubernetes(K8s) is an orchestrator and Cluster Manager of containers, it is a docker based and come to be a deployment solution for application that developed by micro services.

**K8S includes:**
* Orchestrator
* Cluster manager
* Abstraction layer
* IaC

There is an abstraction for the deployment when you are using K8s with AKS/EKS/Minikube or any other services.
I will try to explain the basis of K8s inside this file.

Kubernetes has many objects inside its internal Operating System, we will list each one of them and try to explain the use and needs for each one.

## Kubernetes Resources(Kinds)

- Pod
- Deployment
- Namespace
- Service
- Ingress
- Daemon Set
- Stateful Set
- CRD
- ConfigMap / Secret

### Kubernetes Resource Structure

Each Kubernetes object defines by a YAML file with 5 fields:

- ApiVersion
- Kind
- Metadata
- Spec
- Status

### Deployment > RS > Pod(s) in Namespace

A collection of Pods, are declared by a number(ReplicaSet) which is the desired state of Pods in the Deployment object - Application infrastructure definition.

The RS object responsible on raising another Pod if some running Pod failed.

Pod cannot be created without referring to a deployment(if you try to create only a Pod, it will create a Deployment that holds this Pod with Replica Set=1).

### Pod

Pod is the most little object in Kubernetes OS, it contains one(or more) container inside.

Each Pod is assign to a Deployment(or similar other kinds) and managed by it.
* Contains one or more containers
* Run on a single host within the same namespace
* The pod can be replicated
* All the containers on the same pod have the same IP
* The connection between the containers are with port
* The pod is temporary - we must think the POD can be terminated and survive restarts
* In basic POD is a piece of configuration that can be transferred between nodes.
* init container - pattern for configuring the pod before the pod is up.
* side-card - container for monitor or doing something during other containers running on the same pod(like Istio).

Pod format name: `DEPLOYMENT_NAME-RS_NUMBER-RANDOM_POD_NAMBER`

### ReplicaSet

This is a basic controller inside the Deployment, that declare how much Pods a Deployment need.

### Deployment

This is a declaration of an isolated micro-service, as part of the entire Application.

It contains inside the Pod and ReplicaSet declaration.

### Namespace

We can separate between Pods inside a Deployment by a logical separator - Namespace.

Note: Namespace is only a logical way to separate between pods by a prefix, it help us to order our Deployment, but it is not a physical separator - Pods from Namespace X can be familiar with Pods from Namespaces Y in the same Deployment.

### Service

Define Layer 3/4(network)’s unify access to a group of POD instances inside the Kubernetes Cluster.
It gives via the `kube-proxy` a way to access the Pod with IP and Port.

There are 4 types of Service:

- **ClusterIP**
Take an Endpoint(virtual IP that implemented by the `kube-proxy`) and assign it a name & IP(optional).
It can be configure with some load balancing.
The match happens with labels and selectors.
Basicly, gives an Internal IP of K8S, for connection between pods.
- **NodePort**
Redirect all the traffic to the external interface of the node, and expose one port as declared for this pod.
We are using NodePort because of nodes(not like Pods) getting public IPs, so it accessible from external and we want to expose specific port to the Pod.
Basicly, giving an access to the pods from external by port.
- **LoadBalancer**
Do the same as NodePort, but using Cloud resources and redirect the traffic to the relevant machines inside the cloud.
The Cloud Control Manager is responsible to get all the requests from `kube-api` and translate it to an api request to the Cloud.
When we are using LoadBalancer type on on-prem cluster, we need to use `metallb` as a load balancing service.
- **ExternalName** 
This is a service without selectors.
It redirects all the traffic to external DNS name.
For example: `nissim.cluster.local` → `google.com`

Service is not take a place anywhere inside the cluster, it is only a routing principle inside the cluster. It manage the connection between the pods inside the cluster.

We defines the connection between Service and Deployment with the selector property in the Service against the label for each Pod in the Deployment.

### Ingress:

Layer 7 load balancing, on top of the Service.

It gives us a way to connect the Pod via browser, the service will be accessible by specific path the declared inside the Ingress manifest via 3 kinds:

- `ImplementationSpecific`: With this path type, matching is up to the IngressClass. Implementations can treat this as a separate `pathType` or treat it identically to `Prefix` or `Exact` path types.
- `Exact`: Matches the URL path exactly and with case sensitivity.
- `Prefix`: Matches based on a URL path prefix split by `/`. Matching is case sensitive and done on a path element by element basis. A path element refers to the list of labels in the path split by the `/` separator. A request is a match for path *p* if every *p* is an element-wise prefix of *p* of the request path.

### StatefulSet:

This is a kind that work as a Deployment, but promise 3 more things:

- The name of the pods under the same replica is ordered with a numerical value, for example if we are deploying mysql, the first pod will be **mysql-0**, the second will be **mysql-1**, etc.
When we will run `kubectl scale` it will save the persistence - **mysql-2**, etc. (in Deployment it is a random number).
- As a best effort the pod that has been replaced, will be deployed on the same node(not a promise, but better than Deployment).
- As a promise, the new Pod will connect to the same Storage if the Pod has been replaced(in Deployment is a best effort).

Another difference is the way to configure the strategy of replacing Pods in Deployment, that configure in StatefulSet to a numeric order(**mysql-0,** than **mysql-1, …**)**.**

Because of these benefits, we are using StatefulSet mainly for Storage deployments.

### DaemonSet:

This is a kind that work as a Deployment, but promises the Pod will be deployed in each Node in the Cluster.

### Job & CronJob:

This Kind is raising a Pod to do something in the Cluster, and after it will finish, it will be terminated. It can be one time Job, or a Job that run in schedule time(CronJob).

### ConfigMap & Secrets:
Config Map and Secrets are injecting the data into the ETCD, it can be changed just with a REST API call.
Secrets encrypts the data by Base64.

### Persistence Volume(PV)
A configuration file for defining static volume.

### Persistence Volume Claim(PVC)
A configuration file to ask amount of volume.

### Persistence Volume Claim Template(PVCT)
A configuration file to create dynamic PVC when we use dynamic storage(like Cloud data services)

### HPA:

A controller that raise the replica of the Pods according to some metrics.

### RBAC:
TBD


---
## Kubernetes Components:

When we are raising up a Kubernetes Cluster, there are a few components that coming up to give us the entire cluster.

Kubernetes uses 2 splitted objects: The `controlplane` & The `dataplane`(docker / containerD).

Controlplane - Where the Kubernetes components are running(etcd, schduler, kube-proxy, etc.).

Dataplane - Where the pods(microservices) are running.

This is a different method of work from Virtualization that need to run together for giving a working node.

On Kubernetes the dataplane is accessible also when the controlplane is down(loosely coupled).

**So what happens when we are raising a cluster up(`minikube start`, `rke up`, etc.) ?**

- It creates a new VM.
- Run kubelet(talk with the dataplane for running containers)
- Run controlplane components(apiservice, kube-proxy, scheduler, controller-manager)
    - No HA/redundancy of services, 1 component for each cluster.
- Save the kubectl context(also called kubeconfig)

## Control Loop

This is the basic method that Kubernetes does for get the desired state of application.

Observe → Diff → Act → → Observe → Diff → Act → …

This loop ensure that all Kubernetes objects are in the desired state of the app, so if something wrong in thew system, Kubernetes act to fix the diff.


## Kubernetes Infrastructure

Kubernetes Nodes are separated to 2 groups: Master(Manager) & Node(Worker).

The Worker nodes - do the work
The master nodes - control it.

The Master Node is also has a Worker capabilities in addition to unique services that it has itself.

A Master Node(controlplane) includes:

- ETCD (etcd) - Key/Value Store for the Cluster, this is the base D.B for the cluster. Must be odd number of nodes(1 as a master, 2 or more as replicas).
- K8s API Server (api) - The K8s API, this is the unit that start each operation in Kubernetes. both inside the cluster and from the user to the cluster.
As a user, you are communicate only with the API. This is a rest API.
- Scheduler (sched.) - Responsible on Pods placement according to CPU/Memory/any user definition inside the manifest(affinity), actually decide which node will run the API call that got from the API Service.
- Controller Manager (c-m) - The basic controller that responsible on the Control Loop.
There are several basic controllers in Kubernetes vanilla, such as RS(ReplicaSet), DS(DeamonSet), etc.
We can also extend the capabilities of Kubernetes by create new Controllers for CRDs(Custom Resource Definition), such as Prometheus-Stack, etc.
- Cloud Controller Manager (c-c-m) - Optional for external cloud controller
- CoreDNS - This is not mandatory object inside the controlplane, but in most use cases we are using CoreDNS(or other DNS as a Service solutions) to be our local DNS manager. By default the local DNS name is `CLUSTER_NAME.cluster.local`.

The Worker Node(dataplane) includes:

- Kubelet (kubelet) - The primary “node agent” that runs on each node, work as Docker(or ContainerD) for each node to ensure containers are running in a pod.
- Kube-proxy (k-proxy) - Work as a network proxy for each node, and reflects services as defined in the K8s API. This unit is responsible on manage the IP & Ports for each node.
For each Service that comes up, the kube-proxy configure the IP table according to the details that declared in the Service manifest.

**A basic flow looks like:**
External/Internal API Call -> received on API Service -> save values on ETCD -> create a Control Loop with the Controller -> the Scheduler decides which node will treat on this API call.


---
# Kubernetes Core

Let’s deep dive to How Kubernetes works.

Kubernetes Core uses 3 basic technologies from the Linux Kernel.

### Namespaces

Namespaces are a method for separation between processes in the linux kernel.

It allows processes to see different view of the system each from the other.

The root namespace called namespace-0 which can see all the tree of the namespaces inside the Linux machine.

There are 8 types of namespaces that are build in the linux kernel:

- Mount - The way to show unique file system for each container.
- ProcessID - The way to hide other processes inside the specific process.
- Network - The way to give separated network(IP tables, routing, etc) for each container.
- Interprocess Communication - The way to connect between processes.
- UTS(hostname & domainname) - each container can use unique identity.
- UserID - Limit the container not run as a root, raise the security of the entire machine.
- Control Group(CGroup)
- Time - Each container can use unique time(clock, timezone, etc).

### Control Group(CGroup)

Control Group is a resource management solution for binding several processes into one group.

For each Control Group, you can mange the following things:

- CPU
- Memory
- BlkIO(how much IO ops the group can do)
- NetPrio(the network priority for this group in the entire system)

### Overlay Filesystem

The way to mount two filesystems into one mounted volume and refer it as layers.

One filesystem will be configure as the upper and the second will be configured as the lower.

With this layers method, when a name of file exists in both filesystems:

- Files - The file in the upper layer will be visible while the lower will be hidden.
- Directories - Will be merged together into the upper layer.

With these tree linux kernel based technologies, Kubernetes can perform its game changer capability to manage on top of a VM multiple processes, give for each its resources as need, assign it to a group with relevant metadata and prioritize the input/output.

### IP Tables

Another linux-based technology that Kubernetes use is IP tables for manage the Network layer.

IP Tables is a user-space utility program that allows configure the IP packet filter rules of the linux kernel firewall.

The filters are based on chain of rules from IP tables, each packet try to find its rule match and route according to this match.

There are three traditional protocol for the routing: Filter, Mangle, NAT.


---
## Useful Commands

We can control our Kubernetes Cluster with `kubectl` command as a CLI interface for Kubernetes.

`KUBECONFIG` is an environment variable that configure the file that `kubectl` talk with.

We can override it or use the default that located in `~/.kube/config`.

`kubectl config get-contexts` - list of Kubernetes Clusters in a linux machine.

`kubectl config use-contexts CONTEXT` - configure with which context you are using.

`kubectl config set-contexts CONTEXT` - configure defaults for context’s properties(namespace, user, etc.).

`kubectl get pods —all-namespaces` - get all pods in all namespaces.

`kubectl create deployment --image=IMAGE_NAME DEPLOYMENT_NAME` - like a `docker run` command, it takes defaults for any parameters for this deployment. It must to declare at least the image name.

`kubectl get deployment DEPLOYMENT_NAME -oyaml` - show the deployment details by a YAML format file.

`kubectl describe pod POD_NAME` - get information about the pod values, last events, and current status.

`kubectl scale deployment DEPLOYMENT_NAME —replicas X` - change replica set to X for the deployment.

`kubectl get nodes` - list all nodes(servers) on your cluster.

`kubectl port-forward POD_NAME PORT_NUMBER:TARGET_PORT_NUMBER` - transfer the Pod traffic from the image port to some port that you want to access with to the traffic.

`kubectl expose deployment DEPLOYMENT_NAME —port PORT_NUMBER` - create a Service to the Deployment and listen to this Service with a PORT_NUMBER.

`kubectl exec -it POD_NAME — bash` - enter a bash shell to the pod env.
