# MyFlaskApp

This will show the pod id where the app is running.
First we will deploy the flask app version 1.0 which doesn't show the pod id than we will update the flaskapp image.

## Create Deployment 

### CLI
```
kubectl.exe run <deployment-name> --image=docker.io/mjindal/myflaskapp:1.0 --port=5000

Example:
kubectl.exe run myflaskapp --image=docker.io/mjindal/myflaskapp:1.0 --port=5000

```

### Declarative Way 

* First Create yaml file (myflaskapp.yml)

```
apiVersion: apps/v1beta1
kind: Deployment
metadata: 
  name: static-site-pod     # Name of the Deployment - unique name
spec:
  replica: 1
  template:
    metadata:
      labels:
        apps: myflaskapp   # This will search for pod with the label hello-world-1 (if already exist it will use otherwise will create from below docker image)
    spec:
      containers:
      - name: hello         # Name of the container (name doesn't matter)
        image: docker.io/mjindal/myflaskapp:1.0
        ports:
          - containerPort: 5000 # Port exposed by conatiner
```

* now create deployment using myflaskapp.yml

```
kubectl.exe create -f <deployment-yaml/json-file> --validate=false

Example:

kubectl.exe create -f myflaskapp.yml --validate=false
```

## Expose your application to outside world

- To expose this application to outside world you need to create service.

- Create service

```
kubectl expose deployment/myflaskapp --type="NodePort" --port 5000

output:
service "static-site" exposed
```

- Check your service

```
kubectl.exe get service

output:

NAME                     CLUSTER-IP   EXTERNAL-IP   PORT(S)             AGE
myflaskapp               10.0.0.31    <nodes>       85000:30634/TCP     5s
```
- Access your service from browser

From the get service you can see the expose port here is 30634, it can be anything, it is generated dynamically.
We can also explicitly tell on which Port we want to expose.
Now you should be able to acces the service from your <ip:exposed-port>
Alternatively you can use following command to check the URL 

```
minikube service static-site --url
```

## Update Deployment


### CLI

```
kubectl set image deployments/<deployment-name> <deployment-name>=mjindal/myflaskapp:latest

Example:

kubectl set image deployments/myflaskapp myflaskapp=mjindal/myflaskapp:latest
```

### Declarative 

* update myflaskapp.yml file image to point mjindal/myflaskapp:latest

```
apiVersion: apps/v1beta1
kind: Deployment
metadata: 
  name: static-site-pod     # Name of the Deployment - unique name
spec:
  replica: 1
  template:
    metadata:
      labels:
        apps: myflaskapp   # This will search for pod with the label hello-world-1 (if already exist it will use otherwise will create from below docker image)
    spec:
      containers:
      - name: hello         # Name of the container (name doesn't matter)
        image: docker.io/mjindal/myflaskapp:latest
        ports:
          - containerPort: 5000 # Port exposed by conatiner
```

* Run following command to apply updates.

```
kubectl apply -f <updated-deployment-json/yaml-file> --record

Example:

kubectl apply -f myflaskapp.yml --record
```