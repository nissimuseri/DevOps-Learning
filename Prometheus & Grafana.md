# Prometheus

** Still a draft **

Prometheus is a monitor tool.
It helps us as an operator for Kubernetes.

The change from the old way is using polling instead of pushing.

## Scrap
Scrap is the basic operation of Prom.
For each Scrap, we will pull the metrics from the servers that sent us their metrics(configured port of metrics).
1 Prom server is listen to multiple servers.
We can configure different schedule times for each server.

## Prometheus Server
Prometheus is using P.Q.L(Prometheus Query Language) to query the servers for each Scrap.
Prometheus server is a D.B that saves the metrics from all servers.
The Server can send the metrics to other platforms to show the metrics on graphs in another readable way.

## Metrics
Each metric is a JSON that contains the following fields:
* timestamp
* value = number
* name = string
* labels

## Prometheus Exporter
Prometheus exporter is the way to open the metric port on the servers.
The exporter is responsible only to open the metrics.
The Prometheus server is responsible for all other operations.
This principle raises our scalability, availability, and ease to use.

## Big Architecture
Prometheus server also has the ability to open metrics for another Prometheus server, for managing multiple Prometheus servers into one central place.
Thanos - like a big Prometheus server, but in other architecture.
We are using it to take business metrics, etc.

## Basic Architecture
The basic use in Prometheus is to connect it to Grafana, which draws graphs.
