# What is this

Demo for CNCF Id

[Slide](https://docs.google.com/presentation/d/1YuND5A1mWlmiAvxchDxmlhc6yBayKhtMFqPaJqPpbCY/edit?usp=sharing)

# Prerequisites

- docker-desktop with kubernetes enabled
- lens (optinal)

# Run Docker Container

```bash
# Create + run container
docker run --name random-generator \
    -e LOG_FILE=/logs/random.log \
    -p 8080:8080 \
    k8spatterns/random-generator:1.0

# Accessing the container
docker exec -it random-generator bash

# Remove container
docker rm random-generator
```


# Get and set context

```bash
# set context
kubectl config use-context docker-desktop
kubectl config current-context

# optionally set namespace, otherwise you need to add `-n` flag everytime:
# kubectl -n default get pods
kubectl config set-context --current --namespace=default
kubectl config view --minify --output 'jsonpath={..namespace}'
```

# Apply simple pod


```bash
# apply pod
kubectl apply -f simple-pod.yml
kubectl get pods
kubectl delete pod random-generator

# Remote access container
kubectl exec -it random-generator --container main -- /bin/bash

# port forward
kubectl port-forward pod/random-generator 8080:8080

# delete pod
kubectl delete pod random-generator

```

# Apply simple replicaset


```bash
kubectl apply -f replicaset.yml
kubectl get replicaset
kubectl get pods


# delete pod
kubectl delete replicaset random-generator-replicaset
```

# Apply adapter pod

```bash
# apply pod
kubectl apply -f adapter-pod.yml
kubectl get pods
kubectl describe pod random-generator

# Remote access container
kubectl exec -it random-generator --container main -- /bin/bash
kubectl exec -it random-generator --container adapter -- /bin/bash

# port forward
kubectl port-forward pod/random-generator 8080:8080
kubectl port-forward pod/random-generator 9889:9889

# delete pod
kubectl delete pod random-generator
kubectl get pods
```