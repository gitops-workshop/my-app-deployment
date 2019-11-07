# Kustomize

Build and test:

```
kustomize build base
kustomize build base > kubectl apply -f -
```

```
kubectl get all
```

You should see:

```
NAME         READY   STATUS    RESTARTS   AGE
pod/my-app   1/1     Running   0          73s
```

```
kubectl port-forward pod/my-app 8080:8080
```

Open http://localhost:8080

```
kustomize build o
kustomize build base > kubectl apply -f -

```

### Clean Up

```
kustomize build overlays/dev/|kubectl delete -f -
```
