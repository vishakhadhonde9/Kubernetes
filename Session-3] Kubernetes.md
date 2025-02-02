# How to Create Pod -
## Manifest file -
- A Kubernetes manifest file is a YAML or JSON file that defines the desired state of objects (like Pods, Deployments, Services, etc.) in a Kubernetes cluster.

      apiVersion: v1
      kind: Pod
      metadata:
        name: my-pod
      spec:
        containers:
        - name: my-container
          image: nginx

- **MYSQl File-**

      apiVersion: v1
      kind: Pod
      metadata:
        name: mysql
      spec:
        containers:
         - name: mysql
           image: mysql
          env:
           - name: MYSQL_ROOT_PASSWORD
             value: "rootpassword"
           - name: MYSQL_DATABASE
             value: "mydatabase"
          ports:
           - containerPort: 3306
             hostPort: 3306



### 1. Creating a Pod from manifest file -
- Create a Pod from a manifest file:

      kubectl apply -f manifest_file.yaml

### 2. Creating a Pod from Image -

      docker pull img_name
      kubectl run my-pod --image=img_name


### 2. Viewing Pods -
- To view all pods in current namespace.

      kubectl get pods
      

### 3. Delete a Pod:

      kubectl delete pod <pod-name>

### 4. Delete all Pods:

      kubectl delete pod --all

### 5. Execute a Command Inside the Pod -

      kubectl exec -it pod_name -- /bin/sh
