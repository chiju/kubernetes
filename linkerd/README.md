# Linkerd - Kubernetes

## Linkerd
Linkerd is a light weight **service mesh** for kubernetes and it makes running services easier and safer by giving **runtime debugging, observability, reliability, and security** without requiring any changes to the code.

## Service Mesh

A service mesh is a dedicated infrastructure layer for handling **service-to-service communication**. It’s responsible for the reliable delivery of requests through the complex topology of services that comprise a modern, cloud native application. In practice, the service mesh is typically implemented as an array of **lightweight network proxies** that are deployed alongside application code, without the application needing to be aware.

## Setting Up Linkerd in GKE

### Install kubectl 
```shell
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version
```
### Install Linkerd Client locally

To install the CLI, run:
```bash
curl -sL https://run.linkerd.io/install | sh
```
Next, add  `linkerd`  to the path with:
```bash
export PATH=$PATH:$HOME/.linkerd2/bin
```
Verify the CLI is installed and running correctly with:
```bash
linkerd version
```
### Validating Kubernetes cluster
```bash
linkerd check --pre
```
### Installing Linkerd onto the cluster
```bash
linkerd install | kubectl apply -f -
linkerd check
kubectl -n linkerd get deploy
```
### Accessing Linkerd dashbooard
```bash
linkerd dashboard &
```

### For injecting linkerd to deployements
```bash
kubectl get -n emojivoto deploy -o yaml \
  | linkerd inject - \
  | kubectl apply -f -
```
where **emojivoto** is the namespace, here, all the pods will be recreated by adding an extra proxy container in every pods

For checking everything is working fine
```bash
linkerd -n emojivoto check --proxy
```
## Reference
[installation - official doc](https://linkerd.io/2/getting-started/#step-3-install-linkerd-onto-the-cluster)
