# Cert-manager

This runbook holds information about the cert-manager and what to do for it.

## Installation

Cert-manager can be installed through 

```
## The crd need to present BEFORE installing the cert-manager
$ kubectl apply --validate=false\
    -f
    https://raw.githubusercontent.com/jetstack/cert-manager/release-0.11/deploy/manifests/00-crds.yaml
$ helm repo add jetstack https://charts.jetstack.io
$ helm install --name my-release --namespace cert-manager jetstack/cert-manager<Paste>
```

## Adding issuers

When cert-manager is installed, we need to add Issuers to the cluster. We can add the default Let's
Encrypt issues to the clustr through the following snippet. NOTICE: change the email placeholders for
the certs to be valid.

```
---
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: '<email>'
    privateKeySecretRef:
      name: letsencrypt
    solvers:
    - http01:
        ingress:
          class: nginx
---
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: '<email>'
    privateKeySecretRef:
      name: letsencrypt-staging
    solvers:
    - http01:
        ingress:
          class: nginx
---
```

Afterwards the Let's Encrypt can be set up with the config located under
`configs/lets-encrypt.yaml`.

NOTICE: Version mismatch between old and new `certmanger.k8s.io/cluster-issuer` need to be replaced
 with `cert-manager.io/cluster-issues` in the deployment yamls.
