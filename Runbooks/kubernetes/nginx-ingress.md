# NGINX-ingress

## Installation

Installation can be simply done by helm:

```
helm install --name nginx-ingress stable/nginx-ingress --set rbac.create=true --set controller.publishService.enabled=true
```
