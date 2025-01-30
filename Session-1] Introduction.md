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

























