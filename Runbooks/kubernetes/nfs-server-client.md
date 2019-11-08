# NFS-server

## Installation

### GCE

The following snippet can help with provisioning a NFS-server with service on the google Cloud
Engine for use with google kubernetes engine. Before using the snippet, please ensure the
`<disk-name>` is correlated with a google cloud compute disk name. it is possible to make one through
the google cli tool `gcloud compute disks create --size=<disk-size>GB <disk-name>`

```
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nfs-server
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      role: nfs-server
  template:
    metadata:
      labels:
        role: nfs-server
    spec:
      containers:
      - name: nfs-server
        image: gcr.io/google_containers/volume-nfs:0.8
        ports:
          - name: nfs
            containerPort: 2049
          - name: mountd
            containerPort: 20048
          - name: rpcbind
            containerPort: 111
        securityContext:
          privileged: true
        volumeMounts:
          - mountPath: /exports
            name: mypvc
      volumes:
        - name: nfs-pvc
          gcePersistentDisk:
            pdName: <disk-name>
            fsType: ext4
---
apiVersion: v1
kind: Service
metadata:
  name: nfs-server
  namespace: kube-system
spec:
  ports:
    - name: nfs
      port: 2049
    - name: mountd
      port: 20048
    - name: rpcbind
      port: 111
  selector:
    role: nfs-server
---
```

# NFS-client

## Installation

To install the nfs-client to the cluster, it's possible to use helm:

```
helm --kubeconfig ~/.kube/gcloud upgrade --install nfs-client-provisioner --namespace kube-system \
  stable/nfs-client-provisioner --set nfs.path=/ --set nfs.server=<nfs-server-ip>
```

NOTICE: Don't forget to change the `<nfs-server-placeholder>`.

TODO: Review if it is possible to use the service name instead of the server-ip
