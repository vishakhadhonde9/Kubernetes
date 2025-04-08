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


### 3. Viewing Pods -
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
- You can add labels to resources during creation (using YAML) or afterward (using kubectl).

       kubectl label pod podname <label-key>=<label-value>

       kubectl label pod nginx-pod env=production


### Listing Labels on a Resource -

       kubectl get pod podname --show-labels

       kubectl get pod nginx-pod --show-labels

### Removing Labels from Resources

       kubectl label pod podname <label-key>-

       kubectl label pod nginx-pod env-

### Selecting Pods Based on Labels -


      kubectl get pods --selector key=value



### Selecting Multiple Labels

       kubectl get pods -l <label-key1>=<label-value1>,<label-key2>=<label-value2>

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

## Lebeling to Nodes -

            kubectl label nodes <node-name> <label-key>=<label-value>
            kubectl label nodes <node-name> gpu=true
      

## Check Namespaces -

            kubectl get nodes --show-labels


## Labelling using YAML file -

            apiVersion: v1
            kind: Pod
            metadata:
              name: nginxpod
            spec:
              nodeSelector:
                gpu: "true"  # Only schedules the Pod on nodes labeled with gpu=true
              containers:
                - image: nginx
                  name: mynignx
                  ports:
                    - containerPort: 80

