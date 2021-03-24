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

## Check diff of the upgrade

1. install Helm `diff` plugin  
    ```
    helm plugin install https://github.com/databus23/helm-diff
    ```
2. can now set `diff` in front of the command to see what happens:  
   ```shell
   helm diff upgrade --install --namespace helm-test --values .infra/kubernetes/example.com/environments/dev.yaml example.com .infra/kubernetes/example.com/
   ```
3. diff shows up:   
    ```patch
    helm-test, examplecom-app, ConfigMap (v1) has changed:
    # Source: examplecom/templates/app-ConfigMap.yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
    labels:
    app: 'examplecom-example.com'
    chart: 'examplecom-0.1.0'
    heritage: 'Helm'
    release: 'example.com'
    name: 'examplecom-app'
    data:
    APP_DEBUG: '0'
    APP_ENV: 'prod'
    APP_OPCACHE_STAT: '0'
    -     APP_URL_BASE: 'staging.example.com'
    +     APP_URL_BASE: 'dev.example.com'
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

## Check it works

Note: if autocompletion doesn't work for you, get the exact pod name with

```shell
kubectl -n helm-test get pods
```

Access the container within K8s and check what environment variables it has set:

```shell
$ kubectl -ti -n helm-test exec examplecom-app-<TAB><TAB> -- sh
/ > env
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.0.0.1:443
APP_DEBUG=0
HOSTNAME=examplecom-app-6b5dc5f6f5-w75v7
SHLVL=1
HOME=/root
APP_OPCACHE_STAT=0
TERM=xterm
KUBERNETES_PORT_443_TCP_ADDR=10.0.0.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
APP_URL_BASE=staging.example.com
KUBERNETES_PORT_443_TCP=tcp://10.0.0.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
APP_ENV=prod
KUBERNETES_SERVICE_HOST=10.0.0.1
PWD=/
```
