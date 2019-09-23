# k8s Demo

## Cluster Setup

```sh
gcloud services enable container.googleapis.com --project onx-elocke

gcloud beta container clusters create demo-static \
    --addons=Istio \
    --istio-config=auth=MTLS_PERMISSIVE \
    --zone=us-central1-f \
    --machine-type=n1-standard-2 \
    --num-nodes=5 \
    --enable-autoscaling \
    --min-nodes=3 \
    --max-nodes=10 \
    --num-nodes=5 \
    --project onx-elocke

kubectl create clusterrolebinding admin-binding --clusterrole=cluster-admin --user=$(gcloud info --format="value(config.account)")

kubectl apply -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

In another terminal: `kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040`

## Stage 1 Demo

```sh
kubectl apply -f ./step1/guestbook-all-in-one.yaml
kubectl describe services frontend -n guestbook
```

## Stage 2 Demo

```sh
kubectl apply -f ./step2/complete-demo.yaml
kubectl describe services front-end-lb -n sock-shop
kubectl apply -f ./step2/manifests-monitoring/
kubectl describe services grafana -n monitoring
```

## Stage 3 Demo

```sh
kubectl create ns hipster-shop
kubectl label namespace hipster-shop istio-injection=enabled
kubectl apply -f ./step3/istio-manifests.yaml
kubectl apply -f ./step3/kubernetes-manifests.yaml
kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```
