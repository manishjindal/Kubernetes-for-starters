# Pod
- Create Pods - very basic unit in k8s.
- A pod can be single container pod or it can be combination of multiple containers

```
kubectl create -f pod.yml
```

# Replica Controller
- Replica controller manages, how many pods you want.
- RC will select a pod if exist based on the label and will maintain desired pods.
--If pod doesn't exist it will create as per the spec in yml or json file

```
kubectl create -f replicaController.yml 
```


# Deployment
- Replica Controller is old way to manage the desired state of the pod.
- With Deployment you can manage the desired state of the pod, deployment is not supported in apiVersion: v1 but use apiVersion: apps/v1beta1 for   deployment.
- Deployment is more about rolling updates and rollback.

```
kubectl create -f deploy.yml --validate=false
```

##Rolling out updates with deployment 

```
kubectl apply -f deploy.yml --record
```

# Service
## Service gets single IP,DNS,Port in kubernetes that never changes, supporting pods to a service keeps changing (goes-down and comes-up with new ip).

```
kubectl expose  deployment/hello-world --name=hello-world --target-port=80 --type=NodePort
```

## Declarative way

```
kubectl create -f Service.yml
```


kubectl describe service static-site-svc