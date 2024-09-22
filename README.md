# prometheus-monitoring
learning prometheus for monitoring with grafana
## Pre-requisites
1. Kubernetes cluster
2. [helm](https://helm.sh/docs/intro/using_helm/)
## Kubernetes Cluster Setup
1. create a namespace in the cluster
```shell
kubectl create ns monitoring
```
2. set default namespace to current context (if you don't want to specify namespace with every command)
```shell
kubectl config set-context --current --namespace=monitoring
```
## Commands
1. add helm repo for prometheus
```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
2. add helm repo for grafana
```shell
helm repo add grafana https://grafana.github.io/helm-charts
```
3. update repo
```shell
helm repo update
```
4. install prometheus
```shell
helm install prometheus prometheus-community/prometheus --set prometheus-node-exporter.hostRootFsMount.enabled=false
```
5. expose prometheus service through NodePort
```shell
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext
```
6. install grafana
```shell
helm install grafana grafana/grafana
```
7. expose grafana
```shell
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext
```
8. decode password for grafana login
```shell
echo “password_value” | openssl base64 -d ; echo
```
9. decode username for grafana login
```shell
echo “username_value” | openssl base64 -d ; echo
```
10. login to grafana and add `Data Source` providing the `prometheus-server-ext` url we got after installation (at step 5) `http://host.docker.internal:{NodePort}`
11. now we can create dashboard in grafana 