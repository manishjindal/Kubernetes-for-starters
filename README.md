# Pod
- A Pod is the basic building block of Kubernetes–the smallest and simplest unit in the Kubernetes object model that you create or deploy.
- Like in Docker container is the smallest unit, in k8s Pod is the smallest unit.
- Pod is a wrapper around container in k8s.
- A pod can be single container or multi-container.
- Example: if you have static web-app running inside a container in docker environment, in k8s that single container will run inside Pod.
- Let's take a case where your application is made of multiple container ( client, server ) client in one container and server in different container than these two container can run inside your Pod and can communicate with each other.


```
kubectl create -f pod.yml
```

# Replica Controller (rc,rcs)
- A ReplicationController ensures that a specified number of pod “replicas” are running at any one time.
- In the example here replicaController.yml, We tell rc what is the Pod label and how many Pods we want with that label.
- If there are sufficient Pods present that it will not create any Pod but if the desired number of Pods are not available than it will create more Pods.
--If pod doesn't exist it will create as per the spec in yml or json file

```
kubectl create -f replicaController.yml 
```


# Deployment

- A Deployment provides declarative updates for Pods and ReplicaSets (the next-generation ReplicationController).
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

- A Kubernetes Service is an abstraction which defines a logical set of Pods and a policy by which to access them from outside world.
- Let say there is a pod running your web application, to expose this web application you need services.
- It gives you a way to access your Pods, load balancing among your Pods.
- It hides how it manages the Pods internally.

```
kubectl expose  deployment/hello-world --name=hello-world --target-port=80 --type=NodePort
```

## Declarative way

```
kubectl create -f Service.yml
```


kubectl describe service static-site-svc


# Summary 

- Pods are the basic unit which actually have your application up and running.
- Deployments are more about updating the app and managing the app (like : how many replica you want and what Pod with label to choose for running the application)
- Service is all about exposing your application to outside world.



# Getting Started 

## SetUp
- To Start with Kubernests follow the following instruction
- Install minikube 
- Install kubectl
- Put these in your Path.
- start minikube 

```
minikube start
```


## Deploy first static website inside minikube


```
kubectl.exe run static-site --image=docker.io/seqvence/static-site:latest --port=80
```

- This will create the deployment and one Pod for this deployment

- Check your deployment

```
kubectl.exe get deployments

O/p

NAME                     DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
static-site              1         1         1            1           24s
```

- Check Pod associated with this deployment

```
kubectl.exe get pods

Output:

NAME                                      READY     STATUS    RESTARTS   AGE
static-site-3755874646-1nvz2              1/1       Running   0          2m
``` 

- Try deleting the Pod associated with this deployment

```
kubectl.exe delete pod static-site-3755874646-1nvz2

Output:
pod "static-site-3755874646-1nvz2" deleted
```

- Now again check the Pods
-- Your current pod must be deleted or in terminating mode and new pod will be created to support the deployment.

```
kubectl.exe get pods

output:

NAME                                      READY     STATUS              RESTARTS   AGE
static-site-3755874646-1nvz2              1/1       Terminating         0          2m
static-site-3755874646-x6mk1              0/1       ContainerCreating   0          4s
```


## Expose your application to outside world

- To expose this application to outside world you need to create service.

- Create service

```
kubectl expose deployment/static-site --type="NodePort" --port 80

output:
service "static-site" exposed
```

- Check your service

```
kubectl.exe get service

output:

NAME                     CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
static-site              10.0.0.31    <nodes>       80:30634/TCP     5s
```
- Access your service from browser

From the get service you can see the expose port here is 30634, it can be anything, it is generated dynamically.
We can also explicitly tell on which Port we want to expose.
Now you should be able to acces the service from your <ip:exposed-port>
Alternatively you can use following command to check the URL 

```
minikube service static-site --url
```



