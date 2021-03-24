# Usage

Install / upgrade for `dev`:
```shell
helm upgrade --install --namespace helm-test --values .infra/kubernetes/example.com/environments/dev.yaml example.com .infra/kubernetes/example.com/
```

Install / upgrade for `staging`:                                kubectl -n helm-test describe configmap examplecom-app
```shell
helm upgrade --install --namespace helm-test --values .infra/kubernetes/example.com/environments/staging.yaml example.com .infra/kubernetes/example.com/
```

Check status:
```shell
helm list --namespace helm-test
```

Uninstall:
```shell
helm uninstall --namespace helm-test example.com
```

## Check the values in the `ConfigMap`

View as a list:

```shell
kubectl -n helm-test describe configmap examplecom-app
```

Edit as YAML:

```shell
kubectl -n helm-test edit configmap examplecom-app
```
