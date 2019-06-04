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

## Create network policy resource for pod level
Make the *`hello-server`* workload only be accessed by the pod labeled with *`app=pass`*, aka pod "hello-client-pass"

You should find the normal output in the "hello-client-pass" pod:
```bash
--2019-06-04 09:17:24--  http://hello-server.default.svc:8080/
Resolving hello-server.default.svc (hello-server.default.svc)... 10.19.250.137
Connecting to hello-server.default.svc (hello-server.default.svc)|10.19.250.137|:8080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 68 [text/plain]
```
On the other hand, you should find the timeout message in the "hello-client-fail" pod:
```bash
--2019-06-04 09:26:58--  (try:100)  http://hello-server.default.svc:8080/
Connecting to hello-server.default.svc (hello-server.default.svc)|10.19.250.137|:8080... failed: Connection timed out.
Retrying.
```

## Default network policy (TODO)
- deny all traffic from other namespaces
- egress