# How to deploy a RKE(Runcher Kubernetes Engine) cluster on CentOS:

## Step 1 - Install RKE on the VM:

```
wget -O rke https://github.com/rancher/rke/releases/download/v0.3.1/rke_linux-amd64
chmod +x rke
sudo mv rke /usr/local/bin
```

Check the RKE version to ensure it was installed:

```
rke --version
```

## Step 2 - Docker Installation:

Check which docker version is installed:

```
docker --version
```

if the version is above 19.03, remove and reinstall the specific relevant version, as the requrirements of RKE.
(supported versions are [1.13.x 17.03.x 17.06.x 17.09.x 18.06.x 18.09.x 19.03.x] )

### Docer uninstall:

```
sudo yum remove docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

### Docker 19.03 installation:

Run:

```
sudo yum install docker-ce-19.03.11 docker-ce-cli-19.03.11 containerd.io
```

Check if the docker was install correctly:

```
docker --verision
```
 If you got 19.03 - it was ok.
 
## Step 3 - Configue the RKE cluster:

RKE configuration is detailed on cluster.yml file.
Create this cluster.yml configuration file with this command:

```
rke config --name cluster.yml
```

Make sure you enter the correct inputs for managing a cluster with:
* 1 or more nodes
* correct IP address for each node
* SSH user of the host

For example, if you want to create a local cluster on your machine(with 1 node),
This node will be also the Control Plane, the Worker, and the ETCD hosts.

```
[+] SSH Address of host (1) [none]: localhost
[+] SSH User of host (localhost) [ubuntu]: my_user
[+] Is host () a Control Plane host (y/n)? [y]: y
[+] Is host () a Worker host (y/n)? [n]: y
[+] Is host () an etcd host (y/n)? [n]: y
```

(For our needs and capability, we have created the cluster with 2 nodes).

## Step 4 - Raise the RKE cluster up:
After you have finished to enter all the details, the cluster.yml file will be create.
For running this cluster, you have to enable the execution of `docker.soc`:

```
sudo chmod 666 /var/run/docker.sock
```

After that, raise the cluster up:

```
rke up
```

Make sure the cluster is up and you can see the running Kubernetes resources
by switching the context:

```
kubectl --kubeconfig kube_config_cluster.yml get nodes
cp kube_config_cluster.yml ~/.kube/config
```

And now, you can access the resources on the usually way:

```
kubectl get nodes
```
