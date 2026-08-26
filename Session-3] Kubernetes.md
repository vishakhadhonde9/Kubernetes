# Manifest File -
- Kubernetes YAML manifest is a configuration file used to create, configure, and manage objects in a Kubernetes cluster.
- A typical Kubernetes manifest contains four main top-level fields:

            apiVersion:
            kind:
            metadata:
            spec:

## 1. apiVersion
- apiVersion specifies the Kubernetes API version used to create the object.
- The correct API version depends on the type of Kubernetes object.
- Example:

      apiVersion: v1

For a Deployment, you commonly use:

      apiVersion: apps/v1

## Kubernetes Resources and API Versions

| Resource | `kind` | Common `apiVersion` |
|---|---|---|
| Pod | `Pod` | `v1` |
| Service | `Service` | `v1` |
| ConfigMap | `ConfigMap` | `v1` |
| Secret | `Secret` | `v1` |
| Namespace | `Namespace` | `v1` |
| PersistentVolume | `PersistentVolume` | `v1` |
| PersistentVolumeClaim | `PersistentVolumeClaim` | `v1` |
| ServiceAccount | `ServiceAccount` | `v1` |
| Deployment | `Deployment` | `apps/v1` |
| ReplicaSet | `ReplicaSet` | `apps/v1` |
| StatefulSet | `StatefulSet` | `apps/v1` |
| DaemonSet | `DaemonSet` | `apps/v1` |
| Job | `Job` | `batch/v1` |
| CronJob | `CronJob` | `batch/v1` |
| Role | `Role` | `rbac.authorization.k8s.io/v1` |
| RoleBinding | `RoleBinding` | `rbac.authorization.k8s.io/v1` |
| ClusterRole | `ClusterRole` | `rbac.authorization.k8s.io/v1` |
| ClusterRoleBinding | `ClusterRoleBinding` | `rbac.authorization.k8s.io/v1` |
| HorizontalPodAutoscaler | `HorizontalPodAutoscaler` | `autoscaling/v2` |
| Ingress | `Ingress` | `networking.k8s.io/v1` |
| NetworkPolicy | `NetworkPolicy` | `networking.k8s.io/v1` |
| ResourceQuota | `ResourceQuota` | `v1` |
| LimitRange | `LimitRange` | `v1` |
| StorageClass | `StorageClass` | `storage.k8s.io/v1` |
| PersistentVolumeAttachment | `PersistentVolumeAttachment` | `storage.k8s.io/v1` |
| PriorityClass | `PriorityClass` | `scheduling.k8s.io/v1` |
| PodDisruptionBudget | `PodDisruptionBudget` | `policy/v1` |
| RuntimeClass | `RuntimeClass` | `node.k8s.io/v1` |
| Lease | `Lease` | `coordination.k8s.io/v1` |

You need to use the API version supported for that particular Kubernetes resource.

2. kind

kind specifies what type of Kubernetes object you want to create.

Examples:

kind: Pod
kind: Deployment
kind: Service

For example:

apiVersion: v1
kind: Pod

This tells Kubernetes:

"I want to create a Pod using the v1 API."

3. metadata

metadata contains information that identifies and describes the Kubernetes object.

Example:

metadata:
  name: ubuntu-sleeper-pod
  labels:
    app: ubuntu
    environment: dev

Important fields include:

name

Specifies the name of the Kubernetes object.

metadata:
  name: ubuntu-sleeper-pod

You can then refer to it using:

kubectl get pod ubuntu-sleeper-pod
labels

Labels are key-value pairs used to identify, organize, and select Kubernetes objects.

Example:

labels:
  app: ubuntu
  environment: dev

Labels are very important because Kubernetes objects such as Services can use selectors to find Pods based on their labels.

annotations

Annotations store additional metadata or descriptive information about an object.

Example:

annotations:
  description: "Ubuntu sleeper pod"

Unlike labels, annotations are not intended for selecting or grouping objects. They are commonly used by Kubernetes tools, controllers, and other applications to store additional information.

4. spec

spec defines the desired state or configuration of the Kubernetes object.

The contents of spec depend on the kind of object.

For example, a Pod's spec can contain:

spec:
  containers:
    - name: ubuntu
      image: ubuntu

A Deployment has a different spec.

A Service also has a different spec



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

