# Namespaces -
- A namespace is essentially a way to organize and group resources in Kubernetes.
- It providing a scope for resource names and allowing for the isolation of resources within the same cluster.

##  Default Namespaces -
- Kubernetes includes several default namespaces:
  - **default:** This is the default namespace that Kubernetes uses for resources that do not explicitly specify a namespace when they are created.
  - **kube-system:**  This namespace is reserved for system components that Kubernetes uses to manage and operate the cluster.
  - **kube-public:** The kube-public namespace is mainly reserved for resources that need to be publicly accessible or shared with all users in the cluster.
  - **kube-node-lease:** The kube-public namespace is mainly reserved for resources that need to be publicly accessible or shared with all users in the cluster.
 
## Creating a Namespace -
- **Using kubectl command:**

      kubectl create namespace <namespace-name>


- **Using Manifest File:**

      apiVersion: v1
      kind: Namespace
      metadata:
        name: <namespace-name>

### List the namespaces -

     kubectl get namespaces

## List Resources in a Specific Namespace: 
- To list all resources in a specific namespace.

      kubectl get pods -n my-namespace

## Delete a namespace -
- Deletes the specified namespace and all resources inside it.

      kubectl delete namespace <namespace-name>

# How to Create Pod Inside Specific Namespace- 
#### 1. Using kubectl Command -

    kubectl run pod-name --image=nginx --namespace=my-namespace


#### 2. Using a Pod YAML Manifest -

    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-name
      namespace: namespace
    spec:
      containers:
      - name: nginx-container
        image: nginx


#### 3. If Namesapce is already created but not specify in manifest file -

   kubectl apply -f pod.yaml -n my-namespace








