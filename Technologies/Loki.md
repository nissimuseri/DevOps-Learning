# Loki

** Still a draft **

It's ike Prometheus, just for logging.

There are 3 ways to get information:
* Traces
* Logs
* Metrics

### Logs vs Metrics:
Metrics are numbers.
Logs are text.

Traces is a combination of logs and metrics.


There are 5 types of log streams(log levels):
* INFO
* DEBUG
* ERROR
* TRACE
* WARNING

There are 2 ways to print data about the application/infra:
* STDIN
* STDERR

## Loki vs Other solution?

### ELK Stack as another option:
Elastic search is indexing the data
Log stash is doing manipulation on the logs
Kibana is for presenting the logs


### Loki Architecture:
* Promtail is responsible to collect the logs
* Grafana is responsible on present the logs in a visual way(table/graph)


The difference between Loki and ELK is the way we choose to index the data.

ELK is indexing all the data.
Loki is indexing only the metadata(labels), and zipping the content data.

This solution gives us a simpler way to save logs(both on storage and compute)
