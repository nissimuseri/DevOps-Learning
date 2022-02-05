# Helm

* Need to validate all the contects on this page is correct.

### Helm is a Kubernetes package manager

The Helm gives us multiple advantages:
- DRY(Don’t Repeat Yourself) 
- Separation of concern - We separate between the repeated things to the changed things, it is usually the separation between the application and the configuration.
- IAC(Infrastructure as Code)
- Release management - Manage the package versions and use tags to mark the specific release.
- Automation - Simple things when you create the deployment

The basic purpose - Helm customizes raw Yaml files as a template for multiple uses on different deploy applications with different values.

### Helm Package tree:

Helm Package includes 2 directories with 6 files:
- Chart.yaml
- charts dir
- templates dir
    - NOTES.txt
    - _helpers.tpl
    - deployment.yaml
    - service.yaml
- values.yaml

* Files with an underscore(_) are used to separate the logic of functions from the yaml files. The functions are written in “Go Templates” and are in use inside the yaml files for reuse and simple things.
* NOTES.txt is for printing data to the user when the Chard was created.
* service.yaml is used to configure the Kubernetes resource.
* values.yaml - includes the default values
* it is highly recommended to add default values for any variables.
* charts directory - contains dependencies and sub-charts
* sub-chard can get values from the values.yaml of the parent.

## Useful Commands:

```helm repo add```
Download the package from the remote repo

```helm install -f site-values.yaml —set port=9999```
Install the package, you can pass other values by yaml file or spesific port

```helm upgrate```
Use this command to update the chart after you 

```helm package```
Convert the chart to artifact

```helm create <name>```
Create a chart with the name

```helm template```
Render chart templates

```helm list```
List all the installed charts

### Dictionary:

template - manifest
chart - tree of dirs
package - artifact
release - name of packege that installed
version - of the chart(in the end of the chart file)
revision- how many times I upgrated this chart

When we talk about versions on Helm, it can be confused.
It could be the Helm version, package version(release), or running version(revision).