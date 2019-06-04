# Kubernetes network policy 101

## Provision the kubernetes cluster with network policy enabled on GKE
```bash
# create the cluster
> gcloud container clusters create network-policy-demo101 --machine-type=n1-standard-1 --num-nodes=6 --preemptible --enable-network-policy --zone=your-target-zone

# check the cluster status
> gcloud container clusters describe network-policy-demo101 --zone your-target-zone |grep -i -C5 networkpolicy
...
networkPolicy:
  enabled: true
  provider: CALICO
...

```

## Create workload for test
- hello-client-pass
  - it is a deployment labeled with *`app=pass`*
- hello-client-fail
  - it is a deployment labeled with *`app=fail`*

Those two deployments are in one yaml: [hello-client.yaml](./hello-client.yaml)

- hello-server
  - Deployment and service labeled with *`hello-server`*
  - The workload manifest is [hello-server.yaml](./hello-server.yaml)