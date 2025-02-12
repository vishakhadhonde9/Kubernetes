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
        

## Create a ReplicaSet-

kubectl apply -f replicaset.yaml

## Check ReplicaSet-

kubectl get rs

## Scale Pods:

kubectl scale rs my-app-replicaset --replicas=5

## Delete ReplicaSet:

kubectl delete rs my-app-replicaset

## Get Pods with a Specific Label -

kubectl get pods -l labelkey=label value
kubectl get pods -l 'key in (value1,value2)'

kubectl get pods -l 'key!=value1'



# Daemon Set-
- DaemonSet in Kubernetes ensures that a specific pod runs on every node in your cluster.
- When a node is added to the cluster, the DaemonSet ensures that the pod (NGINX in this case) runs on that node.
- If a node is removed, the pods on that node are automatically deleted.
- If a pod crashes, the DaemonSet will recreate it on the same node to ensure it’s always running.


apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
spec:
  selector:
    matchLabels:
      name: nginx
  template:
    metadata:
      labels:
        name: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80


## List DaemonSets:

kubectl get daemonsets
