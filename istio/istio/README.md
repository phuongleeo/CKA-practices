# Profile dump
```sh
  $ istioctl profile dump demo > istio-operator.yaml
```
# Generate manifest
```sh
  $ istioctl manifest generate > generated-manifest.yaml
```

# Operator manifest
```sh
  $ istioctl manifest generate -f istio-operator.yaml > manifest-operator-generate.yaml
```
# Diff Old New manifest
```sh
  $ istioctl manifest diff generated-manifest.yaml manifest-operator-generate.yaml
```

# Install
```sh
  $ istioctl validate -f istio-operator.yaml
  $ istioctl install -f istio-operator.yaml

This will install the Istio 1.21.2 "demo" profile (with components: Istio core, Istiod, Ingress gateways, and Egress gateways) into the cluster. Proceed? (y/N) y
✔ Istio core installed
✔ Istiod installed
✔ Egress gateways installed
✔ Ingress gateways installed
✔ Installation complete
Made this installation the default for injection and validation.
```

# Install bookinfo app
```sh
$ kubectl label namespace default istio-injection=enabled
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.22/samples/bookinfo/platform/kube/bookinfo.yaml
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.22/samples/bookinfo/networking/bookinfo-gateway.yaml
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.21/samples/addons/kiali.yaml
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.22/samples/addons/prometheus.yaml
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.22/samples/addons/grafana.yaml
```