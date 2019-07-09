# A Single Helm Chart to Deploy Multiple Microservices with Different ConfigMaps

## Why We Need This

If you are running a more than 10+ or even 20+ micro-service architecture, you either will have:

- one huge chart to rule them all, in which case, it's getting harder to read and manage and adjust

or 

- one helm chart for each service, which makes it more than 10+ or 20+ charts, in which case, you get a lot of duplicated chart code.

For flexibility, lean and agile, we use the second method, but we want to get rid of the duplicated code, because it causes inconsistency.

The main idea is to use one single chart with configurable templates to manage deployment, service and hpa to manage all services, while using configmap to differentiate them all.

## Prepare

### Docker

Image used: `ironcore864/hello-world-py`, a simple python helo world with reading one env var.

To build and push:

```
docker build . -t ironcore864/hello-world-py
docker push ironcore864/hello-world-py
```

### K8s

Assuming you have a k8s and helm installed.

## Deploy Service A

```
kubectl apply -f configmap-ms-one.yaml
helm install -n ms-one -f ./values-ms-one.yaml ./mychart
```

## Deploy Service B

```
kubectl apply -f configmap-ms-two.yaml
helm install -n ms-two -f ./values-ms-two.yaml ./mychart
```

## Test

```
kubectl proxy
# open another tab
curl http://127.0.0.1:8001/api/v1/namespaces/default/services/ms-one/proxy/
curl http://127.0.0.1:8001/api/v1/namespaces/default/services/ms-two/proxy/
```
