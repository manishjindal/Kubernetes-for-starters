# Deployment

## Imperative way


- Create Deployment 

```
kubectl.exe run <deployment-name> --image=docker.io/mjindal/myflaskapp-forminikube:latest --port=5000
```

- Update Deployment

```
kubectl set image deployments/<deployment-name> <deployment-name>=mjindal/myflaskapp:latest
```

- Delete Deployment

```
./kubectl delete deployment <deployment-name>
```


## Declarative way

- Sample Deployment File

```
apiVersion: apps/v1beta1
kind: Deployment
metadata: 
  name: static-site-pod 		# Name of the Deployment - unique name
spec:
  replica: 1
  template:
    metadata:
      labels:
        apps: static-site-pod   # This will search for pod with the label hello-world-1 (if already exist it will use otherwise will create from below docker image)
    spec:
      containers:
      - name: hello   			# Name of the container (name doesn't matter)
        image: docker.io/seqvence/static-site:latest
        ports:
          - containerPort: 80	# Port exposed by conatiner
```


- Create Deployment 

```
kubectl.exe create -f <deployment-yaml/json-file> --validate=false
```

- Update Deployment

```
kubectl apply -f <updated-deployment-json/yaml-file> --record
```


- Delete Deployment

```
./kubectl delete deployment <deployment-name>
```