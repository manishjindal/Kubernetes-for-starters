# Running Static Site inside minikube cluster

## CLI

* Create Deployment `kubectl.exe run static-site --image=docker.io/seqvence/static-site:latest --port=80`

    * This will create one deployment and one pod associated with the deployment.

* Expose this to outside world 
    * Create service from the deployment `kubectl expose deployment/static-site --type="NodePort" --port 80`

* Access the service from your browser `minikube service static-site --url`

## Declarative Approach

### Create Deployment using yaml/json file

```
apiVersion: apps/v1beta1
kind: Deployment
metadata: 
  name: static-site-pod         # Name of the Deployment - unique name
spec:
  replica: 1
  template:
    metadata:
      labels:
        apps: static-site-pod   # This will search for pod with the label hello-world-1 (if already exist it will use otherwise will create from below docker image)
    spec:
      containers:
      - name: hello         # Name of the container (name doesn't matter)
        image: docker.io/seqvence/static-site:latest
        ports:
          - containerPort: 80 # Port exposed by conatiner
```

* Save the above file as deployment.yml and run following command to create deployment

```
kubectl create -f deployment.yml
```

### Create Service using yaml/json file

```
apiVersion: v1
Kind: Service
metadata:
    name: static-site-svc             # Service name
    labels:
        app: static-site-svc-label    # Service label
spec:
    type: NodePort                    # Load Balancer, it can be your own load balancer
    ports:
    - ports: 80                       # Exposed container port inside the pod
      nodePort: 30001                 # Exposed port to the outside world.
      protocol: TCP                   # this will be TCP always 
    selector:
      app: static-site-pod            # All Pods which matches this name will be part of this service, 
                                      # if number of pods increases the service will
                                      # Select all those node which matches the label and will perform the load balancing 

```

* Save the above file as service.yml and run the following command to create service

```
kubectl create -f service.yml
```