# Kubernetes -
- Kubernetes is an open-source platform used for managing containers.
- Kubernetes is abbreviated as K8s.
- Kubernetes is orchestration tool as it manages and organizes the deployment of multiple containers across multiple machines (or nodes). It helps you handle a large number of containers and their complexity across many machines.
- It was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF).

## Features of k8s -
### 1. Container Orchestration -
- Kubernetes can manage containers across many machines in a distributed system (cluster). It automates tasks like deployment, scaling, and load balancing for large-scale applications.
- It ensures that your app’s containers are running in a consistent and reliable way across multiple nodes (servers).

### 2. Restart and Recreate Containers -
-  Kubernetes automatically restarts failed containers, replaces containers that are unresponsive, and reschedules containers to healthy nodes.
-  It ensures your application is highly available with minimal downtime.

### 3. Auto-Scaling -
- Kubernetes has built-in auto-scaling.
- It can automatically increase or decrease the number of containers (pods) running based on demand (e.g., when traffic increases).
- It helps you efficiently handle fluctuations in load without manual intervention.

### 4. Load Balancing -
- Kubernetes provides automatic load balancing.
- It ensures traffic is distributed evenly across containers, and if new containers are added or old ones are removed, traffic is routed to the right place.

### 5. Rolling Updates and Rollbacks -
- Kubernetes supports rolling updates, meaning you can update your application without taking it offline.
- If something goes wrong during an update, Kubernetes allows you to roll back to a previous version seamlessly.

### 6. Networking -
- Every container in Kubernetes can be given a unique IP, and services can be defined to expose those containers to the outside world.
- Kubernetes takes care of how containers communicate, even across multiple machines.

### 7. Multi-Cloud and Hybrid Cloud Support -
- Kubernetes can run containers across multiple cloud providers or on hybrid cloud environments.
- It abstracts away the underlying infrastructure, meaning you can move workloads between clouds or between on-premises and cloud environments with ease.

# Terminologies in K8s-
- **Pods:** In Kubernetes, containers are grouped into units called "pods". A pod can contain one or more containers that work closely together.
- **Nodes:** A node is a machine (either physical or virtual) where containers are actually run. A Kubernetes cluster consists of several nodes.
- **Cluster:** A cluster is a set of nodes that work together to run your containers and manage workloads.

# Architecture of K8s -

![image](https://github.com/user-attachments/assets/1b15e6a7-4932-45e4-9117-57c7d2e75332)


- It consists of two main components:
- **1. Control Plane (master):** The part responsible for managing and controlling the entire Kubernetes cluster where all decision are made and keeps everything running smoothly.
- **2. Node (Worker Node):** These are the actual machines where your applications (containers) run.

## Control Plane -
- It again divided into further parts-

### a. kube-apiserver (API Server) -
- API server is the central component that handles all API requests (from users, nodes, or other Kubernetes components).
- It serves as the gateway for all interactions with the Kubernetes cluster. Everything in Kubernetes communicates with the cluster through the API server, including internal and external requests.

### b. etcd -
- etcd is a distributed key-value store that holds the state of the entire Kubernetes cluster.
- etcd as a simple database that stores all the important information about a Kubernetes cluster.
- It stores configuration data, secrets, and the desired state of the system.
  - It remembers everything about your cluster, like which nodes exist, which pods are running, and configuration settings.
  - It keeps multiple copies of the data across different servers, so if one fails, the others still have the information.
  - It updates in real time, so whenever something changes in Kubernetes (like a new pod being created), etcd knows immediately.

### c. kube-scheduler -
- Scheduler is responsible for deciding which node (worker machine) should run the newly created pods. It selects nodes based on factors like resource availability, policies, and constraints.
- Scheduler makes decisions like placing pods where there are enough CPU or memory resources and balancing workloads across nodes.\
- Kube-Scheduler is a core component of Kubernetes that decides where to run new Pods in the cluster. It ensures that workloads are distributed efficiently across worker nodes.

### d. kube-controller-manager -
- Controller manager runs a set of controllers that regulate the state of the cluster. Each controller ensures that the current state matches the desired state; if it not matches then fix it accordingly.
- It checks the system and makes sure everything is as it should be. For example, if some containers have stopped, it will automatically try to start new ones to replace them.

### e. cloud-controller-manager (Optional) -
- Cloud controller manager is responsible for interacting with cloud provider APIs (like AWS, GCP, Azure).
- It manages cloud-specific resources like load balancers, storage volumes, and networking based on the cluster’s needs.
- Not all Kubernetes clusters have this component, but it’s used when Kubernetes is running in a cloud environment.

## 2. Worker Nodes-
### a. kubelet -
- Kubelet is an agent that runs on each node in the cluster.
- It ensures that containers are running in a pod and are healthy.
- It communicates with the API server to get instructions (e.g., which pods to run) and reports back the status of the node and containers.

### b. kube-proxy -
- Kube-proxy is responsible for maintaining network rules on each node. It manages the network communication inside the cluster, ensuring that services can reach the right pods, even when pods are created or destroyed.
- kube-proxy can use different mechanisms to manage traffic routing and load balancing.

### c. Container Runtime -
- Container runtime is the software that runs and manages containers.
- This could be Docker, containerd, or any other supported container runtime.
- Kubernetes relies on the container runtime to pull container images and manage their lifecycle on the node.

### d. Pods -
- A pod is the smallest deployable unit in Kubernetes. It can contain one or more containers that share the same resources (like storage and networking).
- Pods are the actual entities where the applications run on the nodes. The kubelet makes sure the pod and its containers are healthy and running.

















