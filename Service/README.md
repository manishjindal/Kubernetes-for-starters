# Replica Controller


- Sample Service.yml file

```
apiVersion: v1
Kind: Service
metadata:
    name: static-site-svc     # Service name
    labels:
        app: static-site-svc-label    # Service label
spec:
    type: NodePort            # Load Balancer, it can be your own load balancer
    ports:
    - ports: 80               # Exposed container port inside the pod
      nodePort: 30001         # Exposed port to the outside world.
      protocol: TCP           # this will be TCP always 
    selector:
      app: static-site-pod    # All Pods which matches this name will be part of this service, if number of pods increases the service will \
                              # Will select all those node which matches the label and will perform the load balancing 

```


- Create Service 

```
kubectl.exe create -f <svc-yaml/json-file> --validate=false
```

- Update Service

```
kubectl apply -f <updated-svc-json/yaml-file> --record
```


- Delete Service

```
./kubectl delete svc <rc-name>
```

### The beauty of declarative  approach is that same commands are used for everything (Pod, Replica Controller, Deployment, Service) only content inside the file gets changed!