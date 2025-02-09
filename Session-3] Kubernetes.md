# Node -
## 1. List Nodes in the Cluster -
- Displays all nodes in your Kubernetes cluster.
- Shows each node’s status, roles, age, and version.

      kubectl get nodes

## 2. Get Detailed Information about a Node -

      kubectl describe node <node-name>



# Pod -
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

- **MySQL File-**

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
      kubectl get pod -o wide  ---> Shows ip address of container.

  
#### Get Detailed Information about pod -
- Shows events, container status, IP address, and node details.
  
     kubectl describe pod my-pod

#### Check Logs -

      kubectl logs my-pod
     

### 3. Delete a Pod:

      kubectl delete pod <pod-name>

### 4. Delete all Pods:

      kubectl delete pod --all

### 5. Execute a Command Inside the Pod -

      kubectl exec -it pod_name -- /bin/sh




# K8s Cluster -
## 1. Get Cluster Information -
- Displays the Kubernetes cluster information, including the API server and DNS addresses.

            kubectl cluster-info




# Labels -
- labels are key-value pairs attached to resources (like Pods, Services, Deployments, etc.) to organize and select subsets of objects.
- Labels help categorize resources, making it easier to identify, manage, and group them based on certain attributes.

### Adding Labels to Resources -
- You can add labels to resources during creation (via YAML) or afterward (via kubectl).

       kubectl label <resource-type> <resource-name> <label-key>=<label-value>

       kubectl label pod nginx-pod env=production


### Listing Labels on a Resource -

       kubectl get <resource-type> <resource-name> --show-labels

       kubectl get pod nginx-pod --show-labels

### Removing Labels from Resources

       kubectl label <resource-type> <resource-name> <label-key>-

       kubectl label pod nginx-pod env-

### Selecting Multiple Labels

       kubectl get <resource-type> -l <label-key1>=<label-value1>,<label-key2>=<label-value2>

       kubectl get pods -l env=production,app=nginx

### Labeling Resources at Creation (YAML) -

            apiVersion: v1
            kind: Pod
            metadata:
              name: nginx-pod
              labels:
                app: nginx
                env: production
            spec:
              containers:
              - name: nginx
                image: nginx

### Using Label Selectors in Services -

            apiVersion: v1
            kind: Service
            metadata:
              name: nginx-service
            spec:
              selector:
                app: nginx
              ports:
                - protocol: TCP
                  port: 80
                  targetPort: 80




