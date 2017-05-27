# Pod

## Imperative way


- When you run following command it will create exactly one pod associated with the deployment.

```
kubectl.exe run <pod-name> --image=docker.io/mjindal/myflaskapp-forminikube:latest --port=5000
```

- See your Pod
```
kubectl get pods
```

- Delete Pod
-- Pod will be deleted but the pod will be there without any Pod.

```
./kubectl delete pod <pod-name>
```


## Declarative way

With declarative approach you can create Pods alone without pod

- Sample Pod.yml file

```
apiVersion: v1
kind: pod                         
metadata: 
  name: static-site-pod                                 # Pod name
spec:
  containers:
    - name: hello                                       # Container name inside Pod
      image: docker.io/seqvence/static-site:latest      # Image to create the container
      ports:
        - containerPort: 80                             # Container expose port to the outside word.

```

-- One Pod can have multiple container connected to each other or exposing different-different port to the outside word.


- Create Pod 

```
kubectl.exe create -f <pod-yaml/json-file> --validate=false
```

- Update Pod

```
kubectl apply -f <updated-pod-json/yaml-file> --record
```


- Delete Pod

```
./kubectl delete pod <pod-name>
```

### The beauty of declarative  approach is that same commands are used for everything (Pod, Replica Controller, Deployment, Service) only content inside the file gets changed!