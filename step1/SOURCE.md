# Deploy a simple guestbook app

## Source

* https://github.com/kubernetes/examples/tree/master/guestbook
* https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

## Runbook

```sh
kubectl apply -f guestbook-all-in-one.yaml
kubectl describe services frontend -n guestbook
```

Scale frontend

```sh
kubectl -n guestbook scale deployment frontend --replicas=5
kubectl -n guestbook get pods
kubectl -n guestbook scale deployment frontend --replicas=2
kubectl -n guestbook get pods
```
