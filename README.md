## Upgrading and How the Helm Chart Relates to the Deployment Method in the Docs 

The official quickstart instructions instructions [here](https://argo-cd.readthedocs.io/en/stable/getting_started/) work by just downloading a bunch of resources and applying them on the cluster. These resources are generated using kustomize for each release. The helm chart developers say that they want to be as close as possible to this in their [notes on upgrading the helmchart](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd#synchronizing-changes-from-original-repository).
