# Replica Controller

- Sample ReplicaController.yml file

```
apiVersion: v1
kind: ReplicationController
metadata: 
  name: static-site-rc 							  # Name of the Replica Controller
spec:
  replica: 10								         	# Number of Replicas
  selector:
    app: static-site-pod  						# name of the pod which will be used by replica controller to create the replica
  template:
    metadata:
      labels:
        apps: static-site-pod  				# Name pod - if pod not found this name will be used for the created pod
    spec:
      containers:
      - name: hello   							  # Name of the container (name doesn't matter) if pod doesn't exist it will be created using this image.
        image: docker.io/seqvence/static-site:latest
        ports:
          - containerPort: 80

```


- Create Replica Controller 

```
kubectl.exe create -f <rc-yaml/json-file> --validate=false
```

- Update Replica Controller

```
kubectl apply -f <updated-rc-json/yaml-file> --record
```


- Delete Replica Controller

```
./kubectl delete rc <rc-name>
```

### The beauty of declarative  approach is that same commands are used for everything (Pod, Replica Controller, Deployment, Service) only content inside the file gets changed!