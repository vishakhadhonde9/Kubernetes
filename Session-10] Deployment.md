# Deployment -
- Deployment in Kubernetes is a way to manage and maintain a set of identical pods that ensure your application runs smoothly, reliably, and with the desired number of instances.
- It automatically ensures that the correct number of pod replicas are running.
- If a pod crashes or is deleted, the deployment will automatically create new pods to replace them.
- Kubernetes allows you to **update** your application without downtime by updating one pod at a time.
- If something goes wrong after an update then you can easily **roll-back** to the previous version.
