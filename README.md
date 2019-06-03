# Kubernetes network policy 101

## Provision the kubernetes cluster with network policy enabled on GKE
```bash
gcloud container clusters create network-policy-demo101 --machine-type=n1-standard-1 --num-nodes=6 --preemptible --enable-network-policy --zone=your-target-zone

```
