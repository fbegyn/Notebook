# Runbook: gcloud kubernetes engine

## Setting up gcloud CLI tool

We first need to add the apt repo to our sources `/etc/apt/sources.list.d/google-cloud-sdk.list`

```
deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdkmain
```

We also need to import the gcloud gpg key to be able to download from the repo

```
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
```

When the gpg key is imported, it is possible to download the gcloud utility.

```
sudo apt update && sudo apt install google-cloud-sdk
```

If there are issues with downloading it, make sure `apt-transport-https` and `ca-certificates`
packages are installed.

Initiliazing the `gcloud` tool is simple, just type `gcloud init`. This will prompt you to log in and
to select an existing project or create a new project.

## Creating a gcloud k8s cluster

A cluster can easily be started by using the following command

```
gcloud container clusters create <cluster-name> --zone <zone>
```

When the cluster has been booted up and is ready, the kubectl config can be rendered by using the
command `gcloud container clusters get-credentials <cluster-name> --zone <zone>`, which
will append the config to `~/.kube/config`.

### Adding helm to the cluster

Because of RBAC, we need to create an account for tiller/helm.

```
kubectl --context <gke-context> create serviceaccount --namespace kube-system tiller
kubectl --context <gke-context> create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
```

After which it becomes possible to install helm `helm --kube-context <gke-context> init --service-account tiller --wait`.

## Setting up a service account

A service account for Terraform can be set up by using the following command to create the acocunt
and subsequently create the credentials config.

```
gcloud iam service-accounts create [NAME]
gcloud projects add-iam-policy-binding [PROJECT_ID] --member "serviceAccount:[NAME]@[PROJECT_ID].iam.gserviceaccount.com" --role "roles/owner"
gcloud iam service-accounts keys create [FILE_NAME].json --iam-account [NAME]@[PROJECT_ID].iam.gserviceaccount.com
```

## Set gcloud CLI tool default zone

`gcloud config set compute/zone <zone>`

## Set gcloud CLI tool default cluster

`gcloud config set container/cluster <cluster-name>`

## Overview of the k8s engine

### Clusters

`gcloud container clusters list [--zone <zone>] [--region <region>]`

### Node pools

`gcloud container node-pools list [--zone <zone>] [--region <region>] [--cluster <cluster-name>]`

## Adding a node pool to the cluster

Find out which machine are available and in which zone `gcloud compute machine-types list`.

### GPU enabled

A node pool can be added to the cluster with the following command

```
cloud container node-pools create <pool-name> --accelerator type=<nvidia-tesla-v100>,count=1 --zone
<zone> --cluster <cluster-name> --machine-type <machine type> --image-type <image> [--num-nodes <spawn nodes> --min-nodes <min-nodes> --max-nodes
<max-nodes> --enable-autoscaling]
```

`<machine type>` is an identifier from the list `gcloud compute machine-types list`.
`<image>` is one of `COS_CONTAINERD`, `UBUNTU_CONTAINERD`, `COS` or `UBUNTU`.

The part between `[...]` specifies the optional choice to have autoscaling enabled for this pool. It
specifies the nodes to spawn `--num-nodes`, the minimum amount of node to spawn `--min-nodes` and the
max amount of nodes that are allowed to run `--max-nodes`.

The `type` and `count` keywords specify which type of accelerator can be used and count is how many
are assigned to each node. Possible options for `type` are the following:

* `nvidia-tesla-k80`
* `nvidia-tesla-p100`
* `nvidia-tesla-p4`
* `nvidia-tesla-v100`
* `nvidia-tesla-t4`

Please check the [availability](https://cloud.google.com/compute/docs/gpus/#gpus-list) of accelerators in your computing zone.
