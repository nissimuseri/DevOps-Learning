# Add Ingress-Controller & Daily ETCD snapshots on your RKE Cluster

## Ingress Controllers
By default, RKE deploys the NGINX ingress controller on all schedulable nodes.

For configure the cluster to use the ingress-controller on spesific nodes, we will edit the `cluster.yml`.

```
nodes:
- address: 1.1.1.1
  role: [controlplane,worker,etcd]
  user: root
  labels:
    app: ingress

ingress:
  provider: nginx
  node_selector:
    app: ingress
```

* make sure the label of the relevant nodes match the node selector.

save the file, and deploy the changes by running:
> rke up

For more information:
[RKE-addons-ingress-controller](https://rancher.com/docs/rke/latest/en/config-options/add-ons/ingress-controllers/)


## Daily ETCD Snapshots
By default, RKE configured to take a local snapshot for each 12h.
You can configure it to take the snapshot in other frequency, the unit is hour.
Just change the appropriate field in the yaml file:
```
services:
  etcd:
    backup_config:
      interval_hours: 24
```

It is also possible to save the snapshots on S3 Bucket.

The local snapshots are saved on: `/opt/rke/etcd-snapshots`.
For more information:
[RKE-addons-etcd-snapshots](https://rancher.com/docs/rke/latest/en/etcd-snapshots/recurring-snapshots/)