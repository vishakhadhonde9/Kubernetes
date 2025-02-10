#  Replication Controller-
- ReplicationController (RC) is a Kubernetes resource that ensures the desired number of identical Pods are running at any given time.
- If a Pod crashes the ReplicationController will automatically create a new Pod to replace it, ensuring that your application is always running with the desired number of Pods.
- If a Pod is deleted or crashes, the ReplicationController will recreate it.
- Scaling: You can scale your application by changing the number of replicas (Pods).
- Load Balancing: When combined with services, ReplicationControllers help distribute traffic among Pods.

    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: nginx-rc
    spec:
      replicas: 3  # This means 3 Pods will be created
      selector:
        app: nginx  # Select Pods with label 'app=nginx'
      template:
        metadata:
          labels:
            app: nginx  # Label for the Pods
        spec:
          containers:
          - name: nginx
            image: nginx  # Use the 'nginx' container
            ports:
            - containerPort: 80  # Expose port 80 on each container





## List ReplicationControllers:

   kubectl get replicationcontrollers

## View Details of a ReplicationController:

   kubectl describe replicationcontroller / rc podname

## Scale the ReplicationController:
- To change the number of replicas (e.g., scale to 5 Pods):

   kubectl scale rc podname --replicas=5

## Delete a ReplicationController:

   kubectl delete replicationcontroller podname










