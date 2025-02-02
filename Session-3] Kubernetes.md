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








### 1. Creating a Pod -
- Create a Pod from a manifest file:

      kubectl apply -f manifest_file.yaml

### 2. Viewing Pods -
- To view all pods in current namespace.

      kubectl get pods
      kubectl get pod
      kubectl get po

### 3. Delete a Pod:

      kubectl delete pod <pod-name>

### 4. Delete all Pods:

      kubectl delete pod --all

  
