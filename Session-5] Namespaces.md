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
