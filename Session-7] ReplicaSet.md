# Replica Set-
- ReplicaSet (RS) in Kubernetes is a controller that ensures a specified number of pod replicas are running at any given time.
- It automatically creates, deletes, and manages pods to maintain the desired number of replicas, ensuring high availability and reliability.
- It handels complex selectors.



apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-app-replicaset
spec:
  replicas: 3  
  selector:
    matchLabels:
      app: my-app 
  template:
    metadata:
      labels:
        app: my-app  
    spec:
      containers:
      - name: my-app-container
        image: nginx 
        ports:
        - containerPort: 80
        
