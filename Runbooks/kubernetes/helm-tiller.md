# Helm

This runbook holds all the knowledge needed to install and troubleshoot helm/tiller on k8s cluster.
Some info is aimed to be used for RV infra only.

## Adding helm to the cluster

Because of RBAC, we need to create an account for tiller/helm.

```
kubectl --context <gke-context> create serviceaccount --namespace kube-system tiller
kubectl --context <gke-context> create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
```

After which it becomes possible to install helm `helm --kube-context <gke-context> init --service-account tiller --wait`.
