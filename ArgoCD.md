# ArgoCD

* Still a draft

GitOps - IaC & Automation
Gives the option to only Push the code to Git, and the automation of deploy this changes to the Master happens.

ArgoCD uses Kubernetes as the basis infrastructure, and expand it with CRD.
It get Git URL and clone it, with set of values.
On this repo, there are yaml files of Kubernetes(Helm charts / Kubernetes manifest).
The Argo doing apply the changes fron the repo.

During this backend process, the user needs only push his code to Git, the Argo poll the repo and do the other operations for deploy.

AppOfApps - one App which connectes one repo which includes multiple Apps(Repos of helm charts, etc.)

** Need to refactor this file