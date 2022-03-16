# Example to kube-prometheus-stack upgrade

It use `minikube` to provider cluster to execute steps below.

```shell
$ minikube start
$ kubectl apply -f https://k8s.io/examples/application/deployment.yaml
$ helm install kube-monitoring prometheus-community/kube-prometheus-stack --create-namespace --version 18.1.1 -n kube-monitoring
$ kubens kube-monitoring
$ kc get pods
$ minikube service kube-monitoring-grafana -n kube-monitoring
```

Open new terminal, because the service up with node port Grafana. As sign in application, it's necessary see secrets and decode base64 password.

```shell
$ kc get secrets kube-monitoring-grafana -o jsonpath={.data.admin-password} | base64 -d
prom-operator
```

## Upgrade versions

[steps for anothers versions upgrades](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#upgrading-an-existing-release-to-a-new-major-version)

## From 18.x to 19.x

kubeStateMetrics.serviceMonitor.namespaceOverride was removed. Please use kube-state-metrics.namespaceOverride instead.

[Verify release version](https://github.com/prometheus-community/helm-charts/releases) and find release out later, in this case 19.3.0

```shell
helm upgrade --atomic --timeout 10m -i kube-monitoring prometheus-community/kube-prometheus-stack --create-namespace --version 19.3.0 -n kube-monitoring
```
Next versions upgrades, you should see links above and execute command again.